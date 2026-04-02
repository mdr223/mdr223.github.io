---
title: "Deep Research is the New Analytics System: Towards Building the Runtime for AI-Driven Analytics"
collection: publications
permalink: /publication/deep-research
excerpt: 'With advances in large language models (LLMs), researchers are creating new systems that can perform AI-driven analytics over large unstructured datasets. Recent work has explored executing such analytics queries using semantic operators -- a declarative set of AI-powered data transformations with natural language specifications. However, even when optimized, these operators can be expensive to execute on millions of records and their iterator execution semantics make them ill-suited for interactive data analytics tasks. In another line of work, Deep Research systems have demonstrated an ability to answer natural language question(s) over large datasets. These systems use one or more LLM agent(s) to plan their execution, process the dataset(s), and iteratively refine their answer. However, these systems do not explicitly optimize their query plans which can lead to poor plan execution. In order for AI-driven analytics to excel, we need a runtime which combines the optimized execution of semantic operators with the flexibility and more dynamic execution of Deep Research systems. As a first step towards this vision, we build a prototype which enables Deep Research agents to write and execute optimized semantic operator programs. We evaluate our prototype and demonstrate that it can outperform a handcrafted semantic operator program and open Deep Research systems on two basic queries.'
date: 2026-01-12
venue: 'CIDR'
paperurl: 'https://arxiv.org/pdf/2509.02751'
citation: 'Russo, Matthew; Kraska, Tim. (2026). &quot;Deep Research is the New Analytics System: Towards Building the Runtime for AI-Driven Analytics.&quot; <i>CIDR</i>.'
---

- [Paper](https://arxiv.org/pdf/2509.02751)
- [Repository](https://github.com/mitdbg/carnot/)
