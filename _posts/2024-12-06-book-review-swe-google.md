---
layout: post
title: "Book review: Software Engineering at Google"
comments: true
tags: [sofware engineering google]
excerpt_separator: <!--more-->
---

Ever wondered how Google builds (or shall we say, engineers) its products?

For over two decades, Google has been regarded as an elite software engineering company. From groundbreaking technologies like MapReduce, BigTable, TensorFlow, Borg (the predecessor to Kubernetes), Blaze, Spanner, Search, etc., it's no secret that many software engineers (outside of Google) wonder what their engineering process looks like.
<!--more-->

> **Summary**: This book is intended for practitioners who want to deepen their understanding of software engineering and learn some of the best practices from Google.

![swe-google-book](/assets/img/books-library/swe-google.jpg){:style="float: right; width: 10em; height: 10em; margin-left: 2em; margin-bottom: 2em"}

Software engineering is a relatively young field compared to traditional engineering disciplines like Civil and Mechanical engineering. There is also the question of whether the process of building software should be considered an engineering discipline in the first place. And what exactly is _software engineering_?

The book _Software Engineering at Google_ looks at the discipline of software engineering from Google's perspective. It’s intended for practitioners who want to deepen their understanding of software engineering and learn some of the best practices from Google's own experience, accumulated over time.

This 500-page volume is divided into several parts.

## Part I: Thesis

The first part presents the thesis upon which the rest of the book is based. The single chapter in this part begins by defining what software engineering is:

> **Definition**: Software engineering is programming integrated over time.

The chapter emphasizes the point that while programming is certainly a significant part of software engineering (after all, programming is how you generate new software in the first place), it isn't the only part. A distinction is made between programming tasks (the actual act of writing software) and software engineering tasks (development, modification, maintenance, etc.)

The chapter also highlights the three primary themes in software engineering:
* Time (and Change) - Discussions of change and maintenance (of software) over time must be aware of _Hyrum’s Law_: With a sufficient number of users of an API, it does not matter what you promise in the contract: _all observable behaviors of your system will be depended on by somebody_.
* Scale (and Efficiency) - Everything your organization relies upon to produce and maintain code should be _scalable_ in terms of overall cost and resource consumption.
* Trade-offs (and Costs) - This is mainly centered around your organization's decision-making processes. What are the _trade-offs_ involved? What are the _costs_ of making a particular decision (or foregoing the alternative)? Cost roughly translates to the “effort” required to accomplish something (e.g., financial costs - money, resource costs - CPU time, personnel costs - engineering effort, transaction costs, opportunity costs, societal costs).

## Part II: Culture

This part emphasizes the collective nature of a software development enterprise, and that proper cultural principles are essential for an organization to grow and remain healthy.

This section is made up of six chapters:

* How to work well on Teams
* Knowledge Sharing
* Engineering for Equity
* How to Lead a Team
* Leading at Scale
* Measuring Engineering Productivity

The main theme for this part is that software engineering is a _team sport_, and that cultivating healthy social interaction constructs like humility, respect, trust, and knowledge sharing is crucial for team success. This section also highlights the need for building diverse teams - that your team should ideally represent the _users_ of your product.

The chapter on measuring engineering productivity was among the most interesting to me. Google uses the **Goals/Signals/Metrics (GSM)** framework to guide metrics creation:

*  A _goal_ is a desired end result. It’s phrased in terms of what you want to understand at a high level and should not contain references to specific ways to measure it.
* A _signal_ is how you might know that you’ve achieved the end result. Signals are things we would _like_ to measure, but they might not be measurable themselves.
* A _metric_ is a proxy for a signal. It is the thing that we actually can measure. It might not be the ideal measurement, but it is something that we believe is close enough.

The chapter also identifies the five core components of productivity (_QUANTS_):

* Quality of code: What is the quality of the code produced by the engineers?
* Attention from engineers: How frequently do engineers reach a state of flow?
* Intellectual complexity: How much cognitive load is required to complete a task?
* Tempo and velocity: How quickly can engineers accomplish their tasks?
* Satisfaction: How happy are engineers with their tools?

## Part III: Processes

Most of what is covered in this part are standard software engineering processes that should be familiar to many software engineers.

This section is made up of eight chapters:

* Style Guides and Rules
* Code Review
* Documentation
* Testing Overview
* Unit Testing
* Test Doubles
* Larger Testing
* Deprecation

Out of the eight chapters in this part, four chapters are dedicated to _testing_ (shows you how testing is taken seriously at Google). One of the interesting things about testing at Google is the criteria/dimensions for classification of test suites:

* Test **size**: _Size_ refers to the resources that are required to run a test case: things like memory, processes, and time. Using this dimension, a test can be _small_ (runs in a single process), _medium_ (can span multiple processes, use threads, and can make blocking calls, but must be contained within a single machine), or _large_ (can span across multiple machines).
* Test **scope**: _Scope_ refers to the specific code paths we are verifying and how much code is being validated by a given test. Using this dimension, a test can be _narrow-scoped_ (commonly called “unit tests”), _medium-scoped_ (commonly called “integration tests”), or _large-scoped_ (commonly called “functional, end-to-end, or system tests”).

## Part IV: Tools

This section is all about the tools that support the software engineering processes at Google (this is an area in which the company has invested heavily).

This part is made up of ten chapters:

* Version Control and Branch Management
* Code Search
* Build Systems and Build Philosophy
* Critique: Google's Code Review Tool
* Static Analysis
* Dependency Management
* Large-Scale Changes
* Continuous Integration
* Continuous Delivery
* Compute as a Service

Some of the tools mentioned in this section are:

* Piper: a Google in-house-developed centralized VCS, built to run as a distributed microservice (reminds me of _Pied Piper_ from the [Silicon Valley TV series](https://en.wikipedia.org/wiki/Silicon_Valley_(TV_series))).
* [Kythe](https://kythe.io/): a compiler-based indexing tool that powers Google's code search UI.
* Blaze: Google's internal artifact-based Build system. [Bazel](https://bazel.build/) is the open-source equivalent.
* Critique: Google's internal code review tool. This is similar to [Gerrit](https://www.gerritcodereview.com/) (open-source equivalent).
* Tricorder: Google’s static analysis platform.
* Test Automation Platform (TAP): Google’s global Continuous Integration (CI) system.
* Cider: Google's internal online/cloud-based IDE. This is similar to [Project IDX](https://idx.dev/).

Something to note from this section is that Google follows _trunk-based_ development (no dev branches), and its entire codebase (with a few exceptions) is hosted in a single repository (a _monorepo_).

## Conclusion (and missed opportunity)

Overall, _Software Engineering at Google_ is a fantastic book. Curated by veteran Google software engineers, the book gives us an overview of some of the important aspects of software engineering as a discipline. 

One thing that I think is missing from the book is a discussion about _Engineering for Sustainability_. Environmental sustainability is such an important consideration in modern engineering that at least one chapter should have been dedicated to the topic (just like _Engineering for Equity_). Such a topic would cover things like using _energy-efficient_ algorithms and/or [programming languages](https://greenlab.di.uminho.pt/wp-content/uploads/2017/10/sleFinal.pdf). Perhaps the authors will consider adding a chapter on sustainability in the next edition of the book?
