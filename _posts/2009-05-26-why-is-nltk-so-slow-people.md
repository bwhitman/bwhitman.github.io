---
title: "Why is NLTK so slow people"
date: "2009-05-26"
---

AMZN small instance (snail style)

 ### Took 92.35s to parse 10005 words 351 sentences (76.64% passed.) 0.26s per sentence.

Mac Pro

\### Took 26.47s to parse 10005 words 351 sentences (76.64% passed.) 0.08s per sentence.

Where “parse” is pos\_tag, an NP chunker (RegexpParser w/ our own grammar.) Most of the work is in pos\_tag.
