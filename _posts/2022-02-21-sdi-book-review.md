---
layout: post
title: "Book review: System Design Interview: An Insider's Guide"
date: 2022-02-21
categories: [Reviews]
tags: [system-design, interview, book-review]
reading_time: 3
---

Recently, I started rendering a list of interview preparation resources. Having not been in an interview process for 3 years made me realize that my skills may be a little rusty. After I spent some time on Reddit, Hacker News and some esoteric Discord servers, I ordered Alex Xu's "System Design Interview: An Insider's Guide". Apart from that, I also purchased a LeetCode premium and some online courses\*. Long story short, I think Alex's SDI book might have been one of the best investments in my engineering prowess.

The book consists of 16 neatly ordered chapters. It starts with the usual "scale from 1 to million users" problem. Thus, it presents us with the building blocks for the system's scalability and resiliency. We're also presented with a detailed framework for the interview. This is the part that helps you with the mythic signals during your interview. There's also a timebox for each of the framework's parts: discussing the specification, high-level architecture, design deep dive and the follow-up questions. The next few chapters focus on the stepping stones for the distributed systems: consistent hashing, rate limiting, key-value stores and id generation. Here the book shines - the explanations are just-enough-detailed, sprinkled with some math, tradeoffs and further improvements. Before reading the SDI book I used to google the "consistent hashing" every three months just to see if my understanding is still correct (it wasn't). Now I can explain the general idea and the moving parts to my non-technical fiancee, which I consider a huge step-up (maybe not so huge for our relationship).

After the fundamentals, we get eight of the usual system design questions, ordered by the increasing complexity. Every chapter follows the framework guidelines. There are requirements questions examples, HLA, detailed component discussion and some follow-up questions. Compared to other resources (e.g. Exponent videos, Design Gurus' course) Alex gives us a consistently detailed view. It can be easily tuned to fit any interviewing process, doesn't matter whether you have 60 or 120 minutes. The diagrams (there are ~150 of them in the book) are well-thought-out. Moreover, if something's unclear, Alex usually presents the reader with a step-by-step walkthrough.

Each chapter contains a list of relevant blog posts, repositories and papers, e.g. how one company built a rate limiter using Redis sorted sets. Super useful if you'd like to see the theory executed in production. There are, in fact, some topics missing in the book. More could be said about cost management, which is sometimes an afterthought, but for scalable cloud solutions, it is a must. The same goes for databases - there's a notion of sharding and replication, but it could've been more detailed. For key components, it'd be great to have a list of metrics/KPIs, e.g. latencies, replication lag, messages per second or memory usage. These can be used as scalability drivers.

Nevertheless, SDI is a valuable book for all, not only those currently preparing for an interview. You can sense this is a solid material and it's coming from someone large-systems-savvy. Highly recommended to all backend engineers.

Reference links:

1. [Alex LinkedIn](https://www.linkedin.com/in/alex-xu-a8131b11/)
2. [Alex Twitter](https://twitter.com/alexxubyte)

\* Initially I had thought that after some time spent on large microservice systems in BigTech I'd be able to easily jump into the interviewing process. Suffice it to say, this assumption was quickly verified.
