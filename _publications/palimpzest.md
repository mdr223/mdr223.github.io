---
title: "A Declarative System for Optimizing AI Workloads"
collection: publications
permalink: /publication/palimpzest
excerpt: 'A long-standing goal of data management systems has been to build systems which can compute quantitative insights over large corpora of unstructured data in a cost-effective manner. Until recently, it was difficult and expensive to extract facts from company documents, data from scientific papers, or metrics from image and video corpora. Today’s models can accomplish these tasks with high accuracy. However, a programmer who wants to answer a substantive AI-powered query must orchestrate large numbers of models, prompts, and data operations. For even a single query, the programmer has to make a vast number of decisions such as the choice of model, the right inference method, the most cost-effective inference hardware, the ideal prompt design, and so on. The optimal set of decisions can change as the query changes and as the rapidly-evolving technical landscape shifts. In this paper we present PALIMPZEST, a system that enables anyone to process AI-powered analytical queries simply by defining them in a declarative language. The system uses its cost optimization framework to implement the query plan with the best trade-offs between runtime, financial cost, and output data quality. We describe the workload of AI-powered analytics tasks, the optimization methods that PALIMPZEST uses, and the prototype system itself. We evaluate PALIMPZEST on tasks in Legal Discovery, Real Estate Search, and Medical Schema Matching. We show that even our simple prototype offers a range of appealing plans, including one that is 3.3x faster and 2.9x cheaper than the baseline method, while also offering better data quality. With parallelism enabled, PALIMPZEST can produce plans with up to a 90.3x speedup at 9.1x lower cost relative to a single-threaded GPT-4 baseline, while obtaining an F1-score within 83.5% of the baseline. These require no additional work by the user.'
date: 2024-05-23
venue: 'TBD'
paperurl: 'https://arxiv.org/pdf/2405.14696'
citation: 'Liu, Chunwei; Russo, Matthew; Cafarella, Michael; Cao, Lei; Baille Chen, Peter; Franklin, Michael; Kraska, Tim; Madden, Samuel; Vitagliano, Gerardo. (2024). &quot;A Declarative System for Optimizing AI Workloads&quot; <i>Arxiv</i>. 1(3).'
---

- [Paper](https://arxiv.org/pdf/2405.14696)
- [Repository](https://github.com/mitdbg/palimpzest/)
- [Blog Post](https://dsg.csail.mit.edu/projects/palimpzest/)
- [Discord](https://discord.gg/znFN2baN)
