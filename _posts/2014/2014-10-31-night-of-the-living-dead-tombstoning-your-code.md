---
title: "Night of the Living Dead - Tombstoning Your Code"
date: "2014-10-31"
categories: 
  - "development"
tags: 
  - "code"
  - "development"
  - "performance"
  - "tombstone"
---

As code bases grow in size, so does the amount of cruft accumulated from years of refactoring, improvement, or feature abandonment. Dead code can become troublesome to troubleshoot, introduce potential security risks, and confuse developers who continue to work. In an ideal world we would do a simple grep the code base to find references to its use, and if none come up, prune the code. Unfortunately in the real world, this breaks down pretty quickly as people can do some pretty tricky things that make code usefulness non-obvious:

- Dynamic code generation - generating the path or dependency names automatically at run time (getters/setters, importing name spaces)
- External dependencies - projects external to the code base that may depend on some legacy components

There is an interesting idea from David Schnepper from Box who talks about the idea of "tombstones" in the code base. The core idea is that all methods in a code base be instrumented with a simple action called a tombstone such as "Tombstone('2014-10-31')" that are both logged and automatically removed from the code when the code path is determined to be live. Over time, tombstones of dead code will remain which indicates that it is likely safe to remove the zombie code from the code base.

\[embed\]https://www.youtube.com/watch?v=29UXzfQWOhQ\[/embed\]

While this is a fantastic idea for deprecating dead code, I don't think that this is ideal for a production system. There are a number of things to consider when deprecating code in this manner:

- What is the performance impact of adding a tombstone to my application?
- How do you determine an acceptable tombstone purge date? A month? A year?
- What risk does this introduce in third party integration or other dependent services?
- How do you handle cross-project dependencies?

Applied correctly though I think that adding tombstones is an interesting idea, one that perhaps could be automated with solid code instrumentation.
