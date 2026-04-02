---
title: "Abacus: A Cost-Based Optimizer for Semantic Operator Systems"
collection: publications
permalink: /publication/abacus
excerpt: 'LLMs enable an exciting new class of data processing applications over large collections of unstructured documents. Several new programming frameworks have enabled developers to build these applications by composing them out of semantic operators: a declarative set of AI-powered data transformations with natural language specifications. These include LLM-powered maps, filters, joins, etc. used for document processing tasks such as information extraction, summarization, and more. While systems of semantic operators have achieved strong performance on benchmarks, they can be difficult to optimize. An optimizer for this setting must determine how to physically implement each semantic operator in a way that optimizes the system globally. Existing optimizers are limited in the number of optimizations they can apply, and most (if not all) cannot optimize system quality, cost, or latency subject to constraint(s) on the other dimensions. In this paper we present Abacus, an extensible, cost-based optimizer which searches for the best implementation of a semantic operator system given a (possibly constrained) optimization objective. Abacus estimates operator performance by leveraging a minimal set of validation examples, prior beliefs about operator performance, and/or an LLM judge. We evaluate Abacus on document processing workloads in the biomedical and legal domains (BioDEX; CUAD) and multi-modal question answering (MMQA). We demonstrate that, on-average, systems optimized by Abacus achieve 6.7%-39.4% better quality and are 10.8x cheaper and 3.4x faster than the next best system.'
date: 2026-08-31
venue: 'VLDB'
paperurl: 'https://arxiv.org/pdf/2505.14661'
citation: 'Russo, Matthew; Liu, Chunwei; Sudhir, Sivaprasad; Vitagliano, Gerardo; Cafarella, Michael; Kraska, Tim; Madden, Samuel. (2026). &quot;Abacus: A Cost-Based Optimizer for Semantic Operator Systems.&quot; <i>VLDB</i>.'
---

- [Paper](https://arxiv.org/pdf/2505.14661)
- [Repository](https://github.com/mitdbg/palimpzest/)
