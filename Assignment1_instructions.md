# Assignment 1: Serverless Computing - Critical Analysis

## CST8917 - Serverless Applications | Winter 2026

---

## Overview

In this assignment, you will read and critically analyze the research paper **"Serverless Computing: One Step Forward, Two Steps Back"** by Hellerstein et al. (2019). You will then evaluate how **Azure Durable Functions**, the orchestration framework used in this course, has addressed (or failed to address) the paper's criticisms.

This is a **research-based assignment**. There is no coding required.

| | |
|---|---|
| **Weight** | 5% of final grade |
| **Due Date** | One week from assigned date (see Brightspace for exact deadline) |
| **AI Policy** | AI tools are permitted with mandatory disclosure (see Deliverables) |

---

## Learning Objectives

By completing this assignment, you will be able to:

- Read and summarize an academic research paper
- Identify the key limitations of first-generation serverless computing
- Evaluate how Azure Durable Functions addresses serverless limitations
- Assess how serverless computing has evolved since 2019
- Form and defend evidence-based technical opinions

---

## The Paper

> Hellerstein, J. M., Faleiro, J., Gonzalez, J. E., Schleier-Smith, J., Sreekanti, V., Tumanov, A., & Wu, C. (2019). *Serverless Computing: One Step Forward, Two Steps Back.* Conference on Innovative Data Systems Research (CIDR).

**PDF:** [https://www.cidrdb.org/cidr2019/papers/p119-hellerstein-cidr19.pdf](https://www.cidrdb.org/cidr2019/papers/p119-hellerstein-cidr19.pdf)

**arXiv:** [https://arxiv.org/abs/1812.03651](https://arxiv.org/abs/1812.03651)

---

## Assignment Tasks

### Part 1: Paper Summary

Read the paper thoroughly and write a summary (400-600 words) that addresses the following:

1. **Main Argument**: What is the central thesis of the paper? What do the authors mean by "one step forward, two steps back"?

2. **Key Limitations Identified**: Summarize the specific limitations the authors identify with first-generation serverless/FaaS platforms. Your summary should cover:
   - Execution time constraints
   - Communication and network limitations (I/O bottleneck, lack of direct addressability)
   - The "data shipping" anti-pattern (moving data to code vs. code to data)
   - Limited hardware access (no GPUs or specialized hardware)
   - Challenges for distributed computing and stateful workloads

3. **Proposed Future Directions**: What characteristics do the authors propose for future cloud programming? Mention at least three of their proposals.

Write this summary in your own words. Direct quotes from the paper should be minimal and must be cited.

---

### Part 2: Azure Durable Functions Deep Dive

Using your own research (official Microsoft documentation, technical blogs, or published articles), explain how Azure Durable Functions works and how it relates to the paper's criticisms. Address **each** of the following areas:

| Topic | What to Research and Explain |
|-------|------------------------------|
| Orchestration model | How do orchestrator, activity, and client functions work together? How does this differ from basic FaaS? |
| State management | How does Durable Functions manage state (event sourcing, checkpointing, replay)? How does this address the paper's criticism that functions are stateless? |
| Execution timeouts | How do orchestrators bypass the timeout limits that apply to regular Azure Functions? What limits still apply to activity functions? |
| Communication between functions | How do orchestrator and activity functions communicate? Does this address the paper's criticism about functions needing slow storage intermediaries? |
| Parallel execution (fan-out/fan-in) | How does the fan-out/fan-in pattern work? How does it address the paper's concern about distributed computing? |

For each topic, write **100-150 words** explaining the capability and connecting it to the paper's criticisms.

All claims must be supported with cited sources.

---

### Part 3: Critical Evaluation

**1. Limitations That Remain Unresolved**

Although Azure Durable Functions improves workflow coordination and state management, two major criticisms identified by Hellerstein et al. (2019) remain only partially resolved.

First, **storage-mediated communication and the data-shipping architecture persist**. Durable Functions formalizes orchestration and state handling through event sourcing and checkpointing, but all coordination between orchestrator and activity functions still relies on Azure Storage (Microsoft, 2024a). Tasks are scheduled via queues, and results are persisted to storage before replay. This means computation and data remain physically separated. Hellerstein et al. (2019) argue that shipping data to stateless compute rather than colocating computation with data creates performance inefficiencies and I/O bottlenecks. Durable Functions does not eliminate this architectural separation; instead, it manages it more elegantly. While programmability improves, the underlying performance constraints associated with storage-backed coordination still apply.

---

## Deliverables

Submit your work as a **GitHub repository** with everything written in `README.md`.

### Repository Structure

```
CST8917-Assignment1-YourName/
└── README.md
```

### Contents of `README.md`

Your `README.md` must include the following sections in order:

1. **Header**: Your name, student number, course code (CST8917), assignment title, and date
2. **Part 1**: Paper Summary (400-600 words)
3. **Part 2**: Azure Durable Functions Deep Dive (5 topic explanations)
4. **Part 3**: Critical Evaluation (400-600 words)
5. **References**: All sources cited with working hyperlinks
6. **AI Disclosure Statement**: State whether you used AI tools. If yes, describe how (e.g., "Used ChatGPT to help summarize Section 3 of the paper"). If no AI tools were used, state that explicitly.

### Submission Instructions

1. Create a **public** GitHub repository with the structure above
2. Submit the **repository URL** on Brightspace before the deadline

**Expected length:** Approximately 1300-1950 words (excluding header, references, and AI disclosure).

---

## Grading Criteria

- **Accuracy**: Information about the paper and Azure Durable Functions is correct and well-sourced
- **Depth of analysis**: Explanations go beyond surface-level descriptions and show genuine understanding
- **Connection to the paper**: Each part clearly ties back to the paper's specific criticisms and proposals
- **Critical thinking**: Part 3 takes a clear position supported by evidence, not just a summary
- **Writing quality**: Well-organized, clear, and properly cited
- **Completeness**: All required sections and topics are addressed

---

## Academic Integrity

- This is an **individual** assignment. You may discuss ideas with classmates, but all written work must be your own.
- AI tools are **permitted** with mandatory disclosure (see Deliverables).
- Plagiarism or undisclosed AI use will result in a grade of zero and may be referred to Academic Integrity procedures.
- All sources must be properly cited.

---

## Tips for Success

- **Read the paper carefully.** It is only 10 pages and written in accessible language. Do not rely solely on summaries.
- **Use official documentation** for your research: [Azure Durable Functions docs](https://learn.microsoft.com/azure/azure-functions/durable/).
- **Connect your analysis back to the course.** You have built Durable Functions in Weeks 3-4. Use that hands-on experience to inform your evaluation.
- **Take a position in Part 3.** There is no single correct answer. Your grade depends on the quality of your reasoning and evidence, not on which side you take.
