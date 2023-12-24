
+++
title = "Reading AI research papers"
date = 2023-09-04
description = "🌳"
+++

This is a part of my collection of "Learn in Public" notes. This comes from
[Harvard's CS197](https://www.cs197.seas.harvard.edu/).

1. [Online Textbook](https://docs.google.com/document/d/1uvAbEhbgS_M-uDMTzmOWRlYxqCkogKRXdbKYYT98ooc/edit#heading=h.2z3yllpny6or)
1. [Course Page](https://www.cs197.seas.harvard.edu/)

When learning new topics, follow this order of steps:
1. **Read wide**, read parts of papers
2. **Read deep** into papers of interest

See [the lecture notes](https://docs.google.com/document/d/1bPhwNdCCKkm1_adD0rx1YV6r2JG98qYmTxutT5gdAdQ/edit) for full examples

### Reading Wide
Use this to "learn what you don't know" and build a mental model of the topic

Example - We want to learn about image captioning. What we may do:
1. Look it up, maybe find the Papers With Code page on image captioning, take notes
2. Learn about popular benchmarks (e.g. COCO, nocaps in-domain, etc.), read some papers of models that do well on these benchmarks (e.g. mPLUG)
	1. Read the abstracts. Write down some key information in our notes (what is the problem, what is the model, how does it solve the problem, important results, etc.)
    2. We don't have to understand everything, just try to extract the key ideas!
	3. Write thoughts/comments down, too!
3. Look at some datasets, take similar notes
4. Look at Google Scholar for survey papers, take notes
    1. Older survey papers are great for background/foundations, but they may not include state-of-the-art (SOTA) methods

At this point we've read wide, and we have notes of things we should know and terms that commonly show up (e.g. "encoder-decoder architecture"). Write a summary paragraph of all these findings.

### Reading Deep
> Most papers are written for an audience that shares a common foundation: that's what allows for the papers to be relatively concise. Building that foundation takes time, in the span of months, if not years.

Take multiple passes over the paper. You don't have to understand 100%, usually 70-80% is good enough!

Papers with a good introduction will often give some good background. They tend to introduce a problem-solution-problem-solution-problem chain, to which they will then offer a subsequent solution.
- Reading this will give us a good idea of what concepts we need to learn - and what relevant papers we should read

Often we won't understand the specific methods right away - we'll need to read more papers to understand the relevant context. That's okay! Do your best and keep reading through it.

Put relevant methods/benchmarks/concepts on a "To Understand/Learn" list

### Summary
The first step to learning is knowing what you don't know. Read wide to first get a framework of knowledge, so you can fill in the gaps later.

Read deep to get a better idea of what techniques researchers use, what benchmarks are popular, and to get context in the field. (It's hard to understand how SOTA speech-to-text—like [Whisper](https://github.com/openai/whisper)—works without knowing a bit about transformers!)

Don't worry about understanding everything right away! Start by focusing on the main ideas and gradually work your way into the details.

The dense terminology is intimidating because these papers are written by domain experts for other experts. You need to break in by learning their language (their jargon, etc.), which happens best by immersion.

