---
layout: post
title: "Fast LLM Evaluations at Spark: CSVs Over Frameworks"
date: 2026-01-19
canonical_url: https://insights.sparkhq.ai/p/evals
---

*Note: This post was originally published on [Spark's blog](https://insights.sparkhq.ai/p/evals) as part of their AI Engineering Series.*

---

## TL;DR

Building AI products at a startup requires speed over sophistication. We ditched complex evaluation frameworks for simple CSVs and custom scripts—and it's been surprisingly effective. Here's why simplicity wins: engineers label their own data and learn faster, we only evaluate high-impact features, and we can swap models in minutes. If you're a nimble team building AI products, don't let evaluation complexity slow you down. Start simple, stay flexible, and invest in rigor only where it really matters.

## The Evaluation Dilemma

When you're a lean team building an AI-heavy product, you very quickly run into a tension: you want rigor, but you don't have the time, headcount, or appetite for heavyweight infrastructure.

At Spark, we scrape and ingest millions of documents related to large-scale energy projects and data centers. We use these documents to provide key insights like permitting regulations, social sentiment, and opposition to our customers. Initially, we developed these features solely based on cursory manual testing, but we quickly realized that in order to have a real estimate about how good our data extraction is, we needed to run evaluations.

Evaluation is the process by which you test that your AI software is performing tasks as you expect it to.

This post is about our current approach to evals: why it looks "simpler than expected," where it's been surprisingly effective, and how it enables us to move fast without flying blind.

## Start Simple: CSVs, Not Frameworks

Our evaluation stack is intentionally unglamorous. Here's what this looks like in practice.

At the core, we use plain CSV files to track labeled data. When we want to evaluate a task, we manually create a small but high-quality dataset ourselves, with engineers labeling data directly. These CSVs act as our golden datasets. In the example below, you can see a dataset snippet related to a location tagging task:

<div class="my-6 overflow-x-auto rounded-lg border border-gray-200 dark:border-gray-700">
  <table class="min-w-full text-sm">
    <thead class="bg-gray-50 dark:bg-gray-800">
      <tr>
        <th class="px-4 py-3 text-left font-semibold">filename</th>
        <th class="px-4 py-3 text-left font-semibold">url</th>
        <th class="px-4 py-3 text-left font-semibold">location</th>
      </tr>
    </thead>
    <tbody class="divide-y divide-gray-200 dark:divide-gray-700">
      <tr>
        <td class="px-4 py-3 font-mono text-xs">content-1002065.html</td>
        <td class="px-4 py-3 text-gray-600 dark:text-gray-400 text-xs">pv-magazine.com/…/pinal-county-approves…</td>
        <td class="px-4 py-3">Pinal County, AZ</td>
      </tr>
      <tr>
        <td class="px-4 py-3 font-mono text-xs">content-1002066.html</td>
        <td class="px-4 py-3 text-gray-600 dark:text-gray-400 text-xs">maricopa.gov/…/Minutes/_03142024-1847</td>
        <td class="px-4 py-3">Maricopa County, AZ</td>
      </tr>
      <tr>
        <td class="px-4 py-3 font-mono text-xs">content-1002067.html</td>
        <td class="px-4 py-3 text-gray-600 dark:text-gray-400 text-xs">federalregister.gov/…/blm-arizona-environmental…</td>
        <td class="px-4 py-3">Arizona</td>
      </tr>
    </tbody>
  </table>
</div>

From there, we write lightweight, bespoke scripts that:

* Run our classification code against the dataset
* Produces another CSV file with expected/generated task-specific outputs
* Computes metrics like accuracy, precision, and recall (especially for classification problems)

That's it. No dashboards, no complex orchestration, no dedicated evaluation service. From there, we usually review the metrics and the CSV output in order to understand what we can improve and iterate on.

We did experiment with existing evaluation frameworks early on, like popular open-source evaluation frameworks for LLM prompts. They're powerful and have many features already included, but for our use case, they introduced more complexity. Ultimately, what matters is to evaluate the exact business logic that runs in production. It isn't just a prompt — it may do a database lookup, custom logic, and more. Therefore, hooking others' tools into our own code was complicated and hard to maintain.

Writing custom evaluation code turned out to be straightforward, easier to reason about, and far more flexible. For now, CSVs and a script that writes output to a console give us exactly what we need.

## Engineers Doing Labeling

Our engineers do labeling here. It is a very tedious job, but there are a couple of advantages to doing it ourselves.

You learn a lot about what output you really want and the data you are actually feeding to the models. This is especially valuable for building domain expertise and customer focus.

You also learn about how the AI reasons, what it does right and wrong, allowing you to think if iterating on the prompt is what you need -- or on the output schema, or something else.

For example, when building our social media data ingestion feature, our engineers discovered that the model consistently failed on labelling posts as solar, wind, or battery because we used `projectType` as a schema field. Posts weren't always related to a project, but more generally about the type of energy. The model was confused by this, so we changed the prompt and the schema to call the label `topic`, and we got some strong improvements on the results.

This insight came from changing a manual label—no amount of automated metrics would have surfaced this nuance early on. We adjusted our prompt and schema to explicitly handle this case, improving accuracy from 40% to 70%+ just with this change.

## Evaluation Is Expensive, So Be Selective

The biggest constraint isn't tooling; it's time. For a lean team of engineers, creating and maintaining even a 100-row labeled dataset for a single task can take 2-3 hours—time we'd rather spend shipping features.

Labeling data, even in small quantities, is expensive. Running evaluations, maintaining datasets, and interpreting results all take real effort. Doing this for *every* piece of extracted information would slow us down dramatically.

So we don't.

Instead, we apply evaluations only to the highest-leverage signals in our system:

* Location tagging
* Content topic or contextual classification
* Other core signals that materially affect product behavior

For these, correctness really matters. Small drops in quality show up immediately in user experience or downstream logic.

On the other hand, there are extractions where being wrong occasionally is acceptable. For example, if we slightly mis-extract metadata from a specific Facebook post, the impact is low.

We use a simple framework: evaluate if (1) the feature directly impacts user trust or product core value, and (2) small errors have high visibility or downstream costs. Everything else can be monitored through user feedback and production logs. In those cases, we consciously choose not to invest in full evaluation pipelines.

This selectivity is critical. Evaluations are a product decision as much as a technical one.

## Knowing What's Happening in Production

One of the biggest benefits of even a simple evaluation setup is clarity.

Having labeled datasets and repeatable evaluation code gives us a concrete understanding of how our system behaves—not just an intuition. When a user complains about a misclassified document, we can add it to the dataset, tweak the prompt, and verify that our metrics stay on track.

This forces us to be explicit about what "good" means for each task. Metrics are imperfect, but they anchor conversations that would otherwise be vague and subjective.

## Model Swapping Without Fear

Another major upside of our approach is how easy it makes model iteration.

We've built a small in-house framework that allows every part of the app to run the same query against different models. Evaluations are fast and repeatable; we can swap models—different providers, versions, or prompting strategies—and immediately see how they compare on our golden datasets.

This has been invaluable. It lets us:

* Make cost–quality trade-offs deliberately
* Respond quickly when models change upstream
* Avoid over-committing to any single vendor or approach

Crucially, this only works because evaluations are cheap to run and easy to understand.

## Pragmatism Over Perfection

Our evaluation setup is not "best practice" in the abstract sense. It's not designed to scale infinitely, and it's not meant to impress anyone browsing our stack.

What it does do is match our current reality:

* A nimble team
* A fast-moving product
* AI systems that must be reliable in specific, high-impact areas

As Spark grows, this approach will evolve. CSVs may turn into databases, scripts into services, consoles into dashboards. But the underlying philosophy won't change: invest in evaluation where it buys real leverage, and keep everything else as simple as possible.

If you're just starting to build with AI, don't be scared of evaluations—you don't need scale or big investments. Start small; it's already valuable.

## Key Takeaways

For those just starting to build AI features, here are the principles we'd recommend:

1. Start with CSVs and simple scripts for evaluations—fight the urge to build complex infrastructure upfront.
2. Have engineers label their own data, at least initially—the learning is invaluable
3. Be ruthlessly selective about what you evaluate—not everything deserves a full pipeline.
4. Make evaluations fast and repeatable so you can experiment with confidence
5. Let your setup evolve with your needs—simple today doesn't mean simple forever.
