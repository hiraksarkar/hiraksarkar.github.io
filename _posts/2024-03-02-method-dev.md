---
layout: post
title: Method development in computational biology
date: 2021-06-08
description: I absoltuely loved Teng Gao's blog on method development
usemathjax: true
giscus_comments: true
toc:
  sidebar: left
tags:
  - method_development
  - computational
  - science
---

## Teng's wisdom

My dear friend and colleague [Teng Gao](https://teng-gao.github.io/) posted a short blog last year on developing computational methods in the space of computational biology. I feel his pointers are extremely insightful to avoid possible pitfalls and long-running rabbitholes that are very easy to fall into while working with biological data. Moreover Teng's ideas can be easily translated to other domains of applied science. In my academic life I have way more failed long rounding projects than I had successful projects.

Research is hard, so sometimes these long efforts fail and we can't do much about that. But, often we can avoid evident failures if we become strategic about research. This strategization can save careers and undue anxiety and depression. Therefore, I felt I can reproduce Teng's main ideas and further extend them a bit from my own experience.

Teng mentioned in his [blog](https://teng-gao.github.io/blog/2023/method/) a list of common strategies to avoid long-failures and quick iterations. I will refer to the his list as ``TMS''(short for Teng's method of success).

> “Progress in science depends on new techniques, new discoveries and new ideas, probably in that order.” - Sydney Brenner

Quoting Teng --

Developing a new computational method for genomics is exciting but can also be daunting at first. It seems to be as much an art as it is an exact science. Here I summarize some lessons learned having gone through the whole process once:

1. **Avoid reinventing the wheel.** Reuse existing solutions that people have tested before and innovate on top of them.
2. **Simplicity is key.** Fancy math does not always lead to better results. Before applying any complex techniques, try a simple one first.
3. **Iterate fast and optimize later.** Experimental code does not have to be tidy or efficient (it just has to be correct). Optimize once you know something works.
4. **Establish a benchmark early on.** A benchmark is not only essential for demonstrating performance improvements in the end, but also for testing out various approaches while developing a tool.
5. **If you can’t solve a big problem, solve a subpart first.** Similarly, if you can’t solve a general problem, first solve a specific case. Recognize inter-dependencies of sub-problems and modularize. Eventually you will get there!
6. **Be systematic.** I spent a lot of time adjusting model parameters on an ad-hoc basis in order to make my method work better for specific cases. What really helped me understand how things work was systematically studying the effect of different parameter values across all possible scenarios.
7. **Abstraction is the best way to make a method generalize.** I remember a time when I kept finding new cases where my method breaks down. Instead of addressing each problem individually, it was much better to summarize them into classes of errors, and solve them all at once by extending the model.

If I translate the question by Sydney Brenner, the above transition can be summarized as,

New technology —> **New method** —> **New discovery**

Often (and it should be always) a new method starts with a motivation. One can generate a method just for method’s sake, but in applied science these papers are rarely useful. When you read a new technology/data paper, to find motives and consequently find motivation, you can ask yourself some question. We may have ask these questions anyway, but writing them in clear words might be more helpful. Some motivations could be —

1. **Lack of data** — Paper X have answered a biological question, but enough data was not produced to prove it with statistical significance.

2. **Lack of resolution** — The results produced in the paper are noisy. **Ideal** place to invent a method, you can first formulate a problem and than apply *Teng’s list* to the problem.

3. **Inconsistency in findings** — If multiple papers or technology papers are published in the same biological domain and the results are somewhat contradictory, that’s a perfect space to see if the methods used across the papers are consistent, or the assumptions taken to solve these problems are right.

4. **Lack of accuracy** — There is a need for a better model to close the gaps in the current data.

5. **Lack of theoretical understanding** — The theory behind the model is not clear therefore there is a gap explaining the observed data. Again an ideal place to start with *Teng’s list*.

Given the currency of academic world **is** publishing, one way to move forward is reading papers effectively which definitely involve summarizing. One effective way of summarizing is the following —

Summarizing a paper — QEC trick

- Make connections between paper using https://researchrabbitapp.com/ or such tools. Question, Evidence and Conclusion. One needs to highlight these each time he/she reads a paper.

People hailing from CS are in general not technology creators (leaving a few exceptions). We as CS Ph.Ds primarily work on technologies invented by other folks. Finding a computational problem that is both nontrivial and interesting to biologists is reaching almost half way to the goal of publishing good computational biology paper. (To be cont.)