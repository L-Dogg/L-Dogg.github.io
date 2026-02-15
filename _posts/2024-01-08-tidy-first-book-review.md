---
layout: post
title: "Book review: Tidy First? A Personal Exercise in Empirical Software Design"
date: 2024-01-08
categories: [Reviews]
tags: [kent-beck, refactoring, book-review]
reading_time: 2
---

I appreciate authors who can concisely convey meaningful thought without elaborating on their life story, philosophy, etc. This is the case with Kent Beck's "Tidy First?". In circa 100 pages, we get 15 concrete examples of small refactorings, the theory of tidying, and an answer to the eponymous question.

These refactorings - or as the author calls them: tidyings - aren't some new, groundbreaking ideas, but rather simple observations on pitfalls of most codebases and how to get rid of them. Any junior engineer should be able to wrap their head around these concepts. Yet, many experienced programmers strayed far from these rules - why is that the author asks?

Managing tidyings is a separate topic. "Managing the urge to keep tidying is a key tidying skill.". Beck outlines the rules and heuristics that should nudge us in the right direction when trying to refactor a piece of code. For example: is it a truly static system, the ancient one that hasn't been changed for 10 years? Well, leave it as it is: if it ain't broken, don't fix it. There are often short-term economics that may discourage us from tidying, but here's Kent Beck's book to the rescue. "A little bit of tidying as self-care is justified".

The economics of software changes is a matter I wish was discussed more frequently in engineering teams. There's always some technical debt to address and a neverending discussion on whether to tackle it or not. However, not a lot of teams consider these in terms of return on investment. Some problems may not be worth solving - it may be cheaper to pay your cloud provider $10 more a month instead of designing a convoluted scaling solution. The same stands for tidyings, and Beck introduces a wonderful Constantine Equivalence alongside discounted cash flows and options for refactorings.

To me, the gist of the book is that a happy programmer is a much better programmer. Then, we should treat tidyings as small bits of joy in our daily jobs and ask ourselves the "Should I tidy first" question more often.

"Tidy First" concentrates on programmer's relationship with themselves, imposed by their low-level code design choices. Two more books in Beck's series are coming: the next one is supposed to be about relationships between engineers, and another between business and technology. I'm looking forward to reading these, as "Tidy First?" is one of these tiny little gems you can recommend to any software engineer, no matter how experienced he is.
