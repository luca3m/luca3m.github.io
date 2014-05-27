---
layout: post
title:  "Median filter with Redis"
date:   2014-05-27 20:56:00
categories:
---

Recently I had to implement a median filter algorithm, I found Redis very powerful
to accomplish this! A very simple solution and scalable.

As described in [Wikipedia](http://en.wikipedia.org/wiki/Median_filter), Median
filter works in this way: given a signal, output sample is the median of last N input
samples, where N can be any positive integer number. Higher is N, more aggressive
will be your filter.

We need a store for last samples, in a FIFO way, pushing a new one
will drop an older one. Redis provides [lists](http://redis.io/commands#list),
which are perfect for this scope. Also we need to sort these samples in numerical order, to get every
time we want the median value. Redis provides a
[SORT](http://redis.io/commands/sort) command, which does this job.

I implemented these two primitives:

* `add_sample(value)` - called every time to add a new sample
* `get_median()` - to get the median value

For the former we need to call two Redis commands:

```
MULTI
LPUSH <yourkey> <value>
LTRIM 0 <N>-1
EXEC
```
`LPUSH` will simply add a new sample to the list, `LTRIM` ensures that at least
N elements will be stored, no more.

To get median value we need to call `SORT` and then calculate median. SORT returns
all values on that list, sorted by numerical order. I have preferred
to use a script, so Redis avoids returning non useful data to clients:

```lua
local list_key = KEYS[1] -- Key of samples list

-- Sort values
local sorted_values = redis.call("SORT", list_key)
local size = #sorted_values
local median = 0.0

-- Calculate median
if size % 2 == 0 then
  median = (sorted_values[size/2]+sorted_values[size/2+1]) / 2
else
  median = sorted_values[(size+1)/2]
end
-- Use tostring because median value may be floating point
return tostring(median)
```

Calling this script it's easy:

```
EVAL <script> 1 <yourkey>
```

Done!
