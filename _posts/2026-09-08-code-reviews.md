---
layout: post
title: "Code reviews at Spark: What we changed after AI"
date: 2026-09-08
canonical_url: https://insights.sparkhq.ai/p/code-reviews
original_publication: "Spark's blog"
---

It’s an understatement to say that in 2026 the engineering job function changed a lot. With stronger models like Claude Opus/Fable and GPT Astra, we can now delegate most coding tasks to agents while humans focus on other things.

This created a huge imbalance in the usual coding → code review flow that's barely changed since GitHub popularized pull requests almost two decades ago.

I reviewed a lot of code manually, since that is what I was used to, and I’m also very passionate about my craft. But with agents writing code quickly and humans reviewing it slowly, we became the bottleneck in **a fight we can't win**. I realized then that something needed to change.

## Agents review code now

There are two aspects to code reviews where agents are as good as, or even better than, humans: catching bugs and enforcing code style.

We leverage Greptile for the former; in our experience, it has been very good at catching bugs big and small, such as an edge case in the business logic we forgot to handle, or a data migration step we forgot to add.

For code style adherence, we created our own agent with a set of homegrown rules that, via Claude Code, verifies whether the code pushed is compliant.

These are rules related to code quality that we have learned over time and that traditional linters usually don’t catch easily, for example:

1. **Each service should query data models it owns**: we don’t want every service to query just any data model in the DB. This helps with refactoring and keeps custom logic on top of data models in one place.

2. **Pass data models, not IDs**: it’s easy to pass an ID to a service in order to perform an action, but it's not safe. For example: `void addAlert(locationId: int)` versus `void addAlert(location: Location)`.

   An ID is just an int, so anything passes type checks: `addAlert(user.id)` compiles fine even though you meant `user.locationId`.

   We prefer passing the entire data model instead, so the compiler helps ensure type safety. And as a bonus, the method becomes more future-proof in case the `addAlert` method needs other attributes of the `Location` itself.

3. **Don’t hand-roll a custom helper**: AI tends to hand-roll functions like Markdown parsing, URL parsing, template expansion. We prefer to pull in a good production-ready library instead to keep our code robust and avoid maintaining logic that isn’t core to our business.

When humans notice new patterns like this, we update the rules, and our code review agents start using them next time.

## Agents get more context

We provided a few MCP servers to our coding agents that make a huge difference in day-to-day work and also help with testing alongside code reviews.

1. **Production read replica access**: agents can backtest assumptions and verify bugs by reading our production DB (permits, meeting minutes, local news, moratoria filings). Sensitive customer data is obscured, of course.

2. **Browser access**: agents can quickly log in and use the browser to test the Spark app live, verify bugs, and test their code.

3. **Metrics and logs**: agents can access our production logs and product metrics to troubleshoot bugs more easily.

All this makes our agents more autonomous and able to write better code with less steering.

## What remains human

First, we focus human work on testing and evaluating that the feature is what our customers want, which is the most important part of our work.

Second, we review the architecture using a design document, written straight from Pull Requests, Notion docs, or live discussions when speed matters more than a written record.

Finally, we do focused reviews of more important parts of our app, like data models and platform/infrastructure services. Changing them may require a multi-stage data migration, with humans walking through each one. Better to get them as “right” as possible early.

Ultimately, **humans are still accountable** for the changes we ship; we just review one step back. If needed, though, we still look at code. For example, for platform-related changes or when we notice something during spot-checking.

More examples of human-led decisions: for a new feature, should we scrape and process in our batch pipeline, or do it on the fly when users request it? How do user requests/responses flow from frontend to backend?

## The result

With this strategy and these guardrails in place, we shipped features in weeks that used to take months.

It takes a bit to adapt to this new way of working, but it definitely feels better. Once agents have run, I can focus my review on pure architectural changes rather than the code itself. If, while spot-checking, I notice code I should dig into, I do it. Otherwise, I can confidently sign off on changes without reading every line of code!

For our customers, this means one thing: the features they've been asking for, like [Stakeholder Intelligence and Alerts](https://insights.sparkhq.ai/p/spark-august-product-update-stakeholder-intelligence-alerts-upgrades), arrive sooner, while keeping quality consistent.
