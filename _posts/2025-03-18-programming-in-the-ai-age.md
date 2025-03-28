---
layout: post
title: "Programming in the AI Age"
date: 2025-03-18
categories: english, tech
description: "Exploring how artificial intelligence is transforming software development and the role of programmers in an AI-augmented future"
tags: [programming, artificial intelligence, software development, future of coding]
---

Recently, I've been using AI extensively for my coding tasks. Mainly using [Cursor](https://www.cursor.com/) IDE and its super-powered tab-completion that makes writing code much easier.

# AI Tab completion

It seems it reads my mind in a certain way. When I start a code change, it will automatically suggest very relevant completions to finish it.

<div class="flex justify-center mb-5">
  <iframe width="560" height="315" src="https://www.youtube.com/embed/nXEVkU60UY8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div> 

As you can see in the video above, this makes me much more productive and writing code is less tedious. AI understands the intention of my change and provides very relevant completions that I can simply accept. This feature also helps reduce the amount of boilerplate code I need to write myself, which is very welcome.

But I wanted also to try something different — what if AI could write most of the code for me?

# Rewriting CSS of this website

I started this website a decade ago, when [Bootstrap](https://getbootstrap.com/) was the state-of-the-art CSS framework and [Bower](https://bower.io/) was the go-to tool for managing frontend code dependencies.

Many things changed since then, at certain point Bower has been retired and Bootstrap is still good but a bit bloated for what I need. I kept hanging on to it because I didn't have time to rewrite this blog from scratch.

Recently I started a couple of projects and [Tailwind](https://tailwindcss.com/) looked like a much better alternative CSS framework, so I wanted to convert my blog to use it as well.

Using Cursor [Agent mode](https://www.cursor.com/features) mode you can ask to write code for you by explaining in plain english what you want and it performs the changes you asked across your codebase.

I used the following prompt to rewrite my Jekyll layouts:

> Rewrite this layout using Tailwind instead of Bootstrap. Keep the structure similar but it does not need to be exactly the same.

The result was good enough to start with, though I had to tweak small things here and there. Still, it was remarkable that I was able to rewrite this website without knowing all the intricacies of Tailwind. I still don't know the framework very deeply, to be honest. 🙂

# Writing an iOS app

Sometimes I play card games with family and I prefer to use an app to keep track of scores — I know it sounds very nerdy! There are a ton of them in the App store but there was none I really liked. So I thought, why don't I just write one myself?

It has been a while since I wrote any iOS code, so I thought it would be a good idea to use AI to help me.

Using Cursor Agent mode I used prompts like:

> Show a list of players representing scores in a card game. For each player show the score and buttons to increase and decrease it.

> When the user taps the score, show a modal view that allows to change the score via a text field input

And so forth.

Not everything worked on the first try. Here and there I had to dig into SwiftUI to understand why a layout wasn't working as expected or why a tap didn't trigger what the code was supposed to do.

Nevertheless, the results were remarkable. I was able to write a fully functional app in just a few days, despite having limited knowledge of Swift, SwiftUI, and SwiftData. It wasn't just a prototype either — I successfully [shipped](https://apps.apple.com/us/app/tallup/id6743003388) it to the App Store!

While doing this, I also noted that AI is less knowledgeable about iOS development compared to web development. I guess this is because there is less public data available of iOS code.

# Conclusion

It's a very exciting time to be building! Turning ideas into code is much easier and faster than before. After leaving my previous job, I started coding daily and have been enjoying it much more, mainly due to Cursor's powerful auto-completion and the ability to have AI write code for me. While I still need to double-check and sometimes fix AI-generated code manually, it has given me a tremendous productivity boost.
