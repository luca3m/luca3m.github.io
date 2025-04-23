---
layout: post
title: Working with AI
date: 2025-04-23
---

Over the past year, I've been working a lot more with AI. I'm not a machine learning or deep learning expert — and honestly, I never set out to be — but in the last few years AI has become a technology you just can't ignore. And actually learning how it works has actually been a lot of fun.

I've already written [here]({% post_url 2025-03-28-programming-in-the-ai-age %}) about how I use AI for coding, but today I want to share a few lessons from a slightly different angle: **building software that leverages AI**.

# Build your own golden dataset

If you want AI to perform a task for you, **you first have to do the task yourself**.

It may seem easier start writing prompts and check the results, but without a proper dataset to measure the performance you can't have confidence if it's working well or not.

Building such a dataset is very tedious, especially for software engineers (like me) who love to automate everything, but it is important.

**For example:** if you want the AI to classify documents and assign labels, you'll need to manually label at least 100–200 documents yourself. Basically, you're building a *questions and answers* set that will give you a **way to measure** if the AI is doing the job properly.

Without a solid dataset like this, you're blind. You can test a few inputs but you'll have no real confidence that it's going to work for your users at scale.

So my recommendation is: **start by building a small but good dataset**, then write code that checks if the AI's output matches what you expect. It gives you a reliable foundation to build and test against.

# Ask for reasoning

When working with AI, it's very useful to ask not just for an answer, but for a **reasoning field** or also known as **chain of thought** in the output — a little explanation of how the AI got there.

This helps in two ways:
1. it actually makes the AI **reason** better internally. When it generates tokens (words), it *thinks* better if you encourage it to explain itself.
2. it helps you **debug** your prompts. By reading the reasoning, you can quickly spot if the AI misunderstood what you wanted — and refine your instructions.

In my case, analyzing the reasoning output made it clear when the AI didn't fully get my intent. Without that feedback, I would've been trying things out blindly.

A small tip if you want to keep things clean: you can ask the AI to format its output in **JSON**, or use simple **HTML/XML tags** to separate the real response from the reasoning part. That way, your app can easily parse and ignore the reasoning when it's not needed.

# Be lean with frameworks

Right now, there's a **huge explosion** of AI frameworks. There are tools to evaluate model responses, to abstract building RAGs, to build agents... you name it.

But my advice is simple: **stay lean**.

Most of these frameworks are very early, and their abstractions might not survive long-term. It's better to get closer to the metal — build your own abstraction and understand how things work at a low level.

Writing some basic evaluation scripts yourself is usually easy, and it has a huge benefit:
You'll naturally design your code so it's easier to test and evaluate — which is critical when building anything AI-driven.

As you build, I think there are two things you absolutely want:
1. **Flexibility to swap LLM models easily**, depending on cost or performance.
2. **Evaluations for every core part of your app**, so you always know how well your AI is performing.

# Conclusion

Working with AI requires a different mindset than traditional software development. Building good datasets, understanding how models reason is definitely something new I needed to learn.

Overall, I'm excited for what's ahead in this space. The technology is evolving rapidly and will unlock lots of opportunities to create new kinds of products! 🚀
