---
layout: post
title:  "Redis patterns, scheduler"
date:   2013-12-03 17:09:10
categories: 
---

Using [Redis](http://redis.io) is pretty easy to create a simple,
distributed and robust scheduler.
How? Just using [sorted set](http://redis.io/commands#sorted_set) structure.
Sorted set allows you to put inside not only an object but also a score.
If you use timestamp as score, you have **done**!

A scheduler needs these three primitives:

1. enqueue(object, expiring_time) - enqueues an object to be scheduled
2. remove(object) - removes object from queue
3. get_expired() - get an expired object from the queue

The former two primitives are pretty easy to implement, just use `ZADD` and `ZREM` command of redis, using the current timestamp (better in UTC) on ZADD:

{% highlight bash %}
ZADD scheduled_objects <schedule_timestamp> <object-id>
{% endhighlight %}

and a basic ZREM
{% highlight bash %}
ZREM scheduled_objects <object-id>
{% endhighlight %}

`get_expired` is a bit harder, it needs to pop a fired element from the set and lock it
for an amount of time, required by the worker to do the job with it.
Before that time, the worker should already have removed the item from the set.
Otherwise, if for any reason it failed (crashes for example), the object will
fire again on another worker, we will handle failover in this way.

To achieve this primitive, basic Redis commands are not useful, we need to use a script:

{% highlight lua %}
local res = redis.call('ZRANGEBYSCORE',KEYS[1],0,ARGV[1],'LIMIT',0,1)
if #res > 0 then
  redis.call('ZADD', KEYS[1], ARGV[2], res[1])
  return res[1]
else
  return false
end
{% endhighlight %}

Used in this way:

{% highlight bash %}
EXEC <script> 1 scheduled_objects <now> <now+lock_for> 
{% endhighlight %}

Finally you need a process-worker, written in any language you want that
every N seconds polls redis using get_expired() primitive, getting jobs and running the work.

### Conclusion

Weakpoints of this scheduler are: polling approach and schedule time precision,
which is in the range *0 ≤ precision ≤ polling_interval*. But as a tradeoff,
the result is a scheduler with no master/slaves synchonizations, simple and with good failover.