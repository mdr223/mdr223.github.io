---
title: 'Palimpzest: A Declarative System for Optimizing AI Workloads'
date: 2024-05-30
permalink: /posts/2024/05/palimpzest
tags:
  - Palimpzest
  - AI Programming
  - LLMs
  - Relational Optimization
  - Semantic Operators
---

*A long-standing goal of data management systems has been to build systems which can compute quantitative insights over large corpora of unstructured data in a cost-effective manner. In this post we introduce Palimpzest, a system that enables anyone to process AI-powered analytical queries simply by defining them in a declarative language.*

**Links:** [Paper](https://arxiv.org/pdf/2405.14696) &#124; [Code Repository](https://github.com/mitdbg/palimpzest) &#124; [DSG Project Page](https://dsg.csail.mit.edu/projects/palimpzest/) &#124; [Discord](https://discord.gg/dN85JJ6jaH)

## Scaling Modern AI Systems to Large Corpora is Challenging

Advances in AI models have driven progress in applications such as question answering, chatbots, autonomous agents, and code synthesis. In many cases these systems have evolved far beyond posing a simple question to a chat model: they are complex [AI systems](https://bair.berkeley.edu/blog/2024/02/18/compound-ai-systems/) that combine elements of data processing, such as Retrieval Augmented Generation (RAG); ensembles of different models; and multi-step chain-of-thought reasoning.

It is easy for the runtime, cost, and complexity of these AI systems to escalate quickly, particularly when applied to large collections of documents. This places a burden on the AI system developer to marshal numerous optimizations in order to optimally manage trade-offs between runtime, cost, and output quality.

### Semantic Analytics Applications (SAPPs)

Consider the following AI-powered analytical tasks:

- **Legal Discovery:** Prosecutors conducting an investigation wish to identify emails from defendants which are (a) related to corporate fraud and (b) do not quote from a news article reporting on the business in question. Test (a) may be implemented using a regular expression or UDF, while (b) requires semantic understanding.

- **Real Estate Search:** A homebuyer wants to use online real estate listing data to find a place that is (a) modern and attractive, and (b) within two miles of work. Test (a) is a semantic search task that possibly involves analyzing images, while (b) is a more traditional distance calculation.

- **Medical Schema Matching:** A researcher would like to (a) download datasets associated with cancer research papers, (b) identify the datasets that contain patient experiment data, and (c) integrate those datasets into a single table.

These tasks: (1) interleave traditional data processing with AI-like semantic reasoning, (2) are data-intensive, and (3) can be decomposed into an execution tree of distinct operations over sets of data objects. Taken together, these criteria outline a broad class of AI programs that are important, complex, and potentially very optimizable; we call them **semantic analytics applications** (SAPPs).

## Challenges Scaling AI Systems to Process SAPPs

Naively scaling AI Systems to process SAPPs with thousands or millions of inputs appears to require spending a huge amount of runtime and money executing high-end AI models. Some high-quality LLMs process *fewer than 1 KB of text per second*, while others cost *5 USD for processing just 5MB of data*. These numbers are many orders of magnitude worse than any other component of the modern data processing stack.

Consider the wide range of technical decisions an AI engineer faces:
- **Prompt Design:** optimize wording and decide on a general prompting strategy (e.g., zero-shot, few-shot, chain-of-thought, etc.)
- **Model Selection:** pick the best model *for each subtask in the program*, balancing time, cost, and quality
- **Execution Strategy:** decide whether each subtask is best implemented by a foundation model query, synthesized code, or a locally-trained student model
- **System Scaling:** select an efficient execution plan when scaling to larger datasets
- **External Integration:** choose parameters (e.g., the number of chunks to return per RAG query) to yield the best trade-offs

## Declarative Programming to the Rescue (Again)

Our key insight is that machines, not human engineers, should decide how best to optimize semantic analytics applications. Engineers should be able to write AI programs at a high level of abstraction and rely on the computer to find an optimized implementation. A similar set of circumstances led to the development of the relational database query optimizer in the 1970s. Today's underlying technical challenges are very different, but the basic idea of declarative program optimization remains valuable.

Consider the declarative program below, which we used for the Legal Discovery workload:

```python
import palimpzest as pz

class Email(pz.TextFile):
    """Represents an email, which can subclass a text file"""
    sender = pz.StringField(desc="The email address of the sender", required=True)
    subject = pz.StringField(desc="The subject of the email", required=True)

# define logical plan
emails = pz.Dataset(source="enron-emails", schema=Email)
emails = emails.filter("The email is not quoting from a news article or an article ...")
emails = emails.filter("The email refers to a fraudulent scheme (i.e., \"Raptor\", ...)")

# user specified policy
policy = pz.MinimizeCostAtFixedQuality(min_quality=0.8)

# execute plan
results = pz.Execute(emails, policy=policy)
```

Programs written with Palimpzest are executed lazily—no actual data processing occurs until the `Execute()` call, at which point the system generates a logical plan, creates multiple optimized physical plan candidates, chooses one according to the specified policy, and executes it.

## Palimpzest System

In our paper we lay out a vision and a prototype for **Palimpzest**, a system that enables engineers to write succinct, declarative code that can be compiled into optimized programs. When running an input user program, Palimpzest considers a range of logical and physical optimizations, then yields a set of possible concrete executable programs. Palimpzest estimates the cost, time, and quality of each one, then chooses a program based on runtime user preferences.

The main system steps are:

1. The user writes a declarative AI program using the Palimpzest (PZ) library; this gets compiled to an initial logical plan.
2. Palimpzest applies logical optimizations to transform the initial logical plan into a set of logical plans.
3. Palimpzest applies physical optimizations to create a much larger set of candidate physical plans.
4. Palimpzest executes **sentinel plans** to gather sample execution data.
5. The sample execution data is used to perform better cost estimation for the candidate physical plans.
6. The final plan is selected based on plan estimates and a user-specified policy for choosing among plans.
7. The final plan is executed and results are yielded to the user.

## Key Results

**Palimpzest Creates Plans with Diverse Trade-offs.** Palimpzest is able to create useful plans at a number of different points in the trade-off space. On the Legal Discovery workload, Palimpzest suggested physical plans that dominated the GPT-3.5 and Mixtral baselines, with an F1-score that is 7.3x and 1.3x better (respectively) at lower runtime and cost. Compared to the GPT-4 baseline, Palimpzest produced plans that are ~4.7x faster and ~9.1x cheaper, while still achieving up to 85.7% of the GPT-4 plan's F1-score.

**Palimpzest Optimizer Selects High-Quality Plans.** On the Legal Discovery workload, the plan selected by Palimpzest achieved a runtime and financial cost that are 80.0% and 89.7% lower than the GPT-4 baseline, respectively, at an F1-score within 81.1% of the baseline.

**Palimpzest Can Provide Speedups of up to 90.3x.** With parallelism enabled, Palimpzest achieved a **90.3x speedup at 9.1x lower cost while obtaining an F1-score within 83.5% of the GPT-4 baseline**. On Real Estate Search and Medical Schema Matching, the optimized plans dominated the GPT-4 baseline on all metrics with speedups of 20.0x and 5.6x as well as cost savings of 2.9x and 1.5x, respectively.

## Read the Paper

[Chunwei Liu\*, Matthew Russo\*, Michael Cafarella, Lei Cao, Peter Baille Chen, Zui Chen, Michael Franklin, Tim Kraska, Samuel Madden, Rana Shahout, Gerardo Vitagliano. Palimpzest: Optimizing AI-Powered Analytics with Declarative Query Processing. CIDR 2025.](https://arxiv.org/pdf/2405.14696)

\* Denotes equal contribution.
