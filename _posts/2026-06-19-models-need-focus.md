---
layout: post
title: "Models Need Focus"
date: 2026-06-19
---

One thing I realized building AI products is how much focus matters.

At [Spark AI](https://sparkhq.ai), we build features that extract structured data from documents. Most of the time results look solid, but then you hit one field that consistently misbehaves. Location in a news article, say, where three different places are mentioned and the model has to decide which one counts.

The instinct is to add more instructions. It helps a little. But the real problem is that you're asking the model to do two different things in one prompt: extract fields verbatim and make a nuanced judgment. Those are different tasks, and bundling them degrades performance on both.

The fix is to split the prompt. One handles the straightforward extraction, another focuses exclusively on the hard field: tighter context, clearer task, room to reason through the ambiguity. Results improve immediately.

It's not surprising in hindsight: asking too many things at once confuses models the same way it confuses people. A focused prompt gives the model one job to do well.

This has shown up consistently to us across simple and complex tasks, smaller and larger models alike. When something's underperforming, the first question we ask now is: what are we actually asking this prompt to do, and can it be split?
