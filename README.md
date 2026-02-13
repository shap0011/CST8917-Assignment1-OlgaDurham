# Olga Durham st#: 040687883

## CST8917 - Serverless Applications | Winter 2026

## Assignment 1: Serverless Computing - Critical Analysis

---

## Part 1: Paper Summary (400-600 words)

### 1.1 Main Argument (central thesis)

Hellerstein et al. argue that today’s **serverless** offerings, especially Functions-as-a-Service (FaaS) like AWS Lambda, deliver a real breakthrough in operational simplicity and autoscaling: developers can upload code, trigger it by events, and pay only for execution without managing servers. That’s the **one step forward**.

But they claim serverless also takes **two steps back** because the current model is built around short-lived, isolated, non-addressable functions that are separated from data and from each other. This design makes many modern workloads, especially data-intensive and distributed/stateful systems, inefficient or impractical, and it nudges developers toward proprietary managed services instead of enabling broad innovation on the cloud platform. (Hellerstein et al., 2019)

### 1.2 Key Limitations Identified

Hellerstein et al. identify several structural limitations in first-generation FaaS platforms that undermine their usefulness for modern cloud computing.

**Execution time constraints.**
Functions in platforms like AWS Lambda are short-lived (e.g., 15-minute maximum). Because function instances are ephemeral and cannot rely on being reused, developers must externalize all persistent state to storage systems. This makes long-running analytics, iterative machine learning training, and stateful workflows inefficient or fragmented across multiple invocations (Hellerstein et al., 2019).

**Communication and network limitations.**
Serverless functions are not directly addressable over the network. They cannot communicate with each other via low-latency messaging and must instead exchange data through intermediary storage services like S3 or DynamoDB. The authors show that this introduces severe I/O bottlenecks and high latency. Network bandwidth per function is limited, and performance degrades as concurrency increases, making distributed coordination slow and expensive.

**The `data shipping` anti-pattern.**
FaaS separates compute from data and forces functions to pull data across the network rather than executing computation near where data resides. This **shipping data to code** model ignores well-known principles of database and systems design, where moving computation closer to data reduces latency, bandwidth usage, and cost.

**Limited hardware access.**
FaaS platforms provide only basic virtualized CPU and memory slices, with no access to GPUs or specialized accelerators. Given the growing importance of hardware acceleration (e.g., for machine learning), this constraint restricts innovation and prevents efficient execution of many modern workloads.

**Challenges for distributed and stateful systems.**
Because functions cannot maintain identity or persistent state and cannot directly message one another, implementing distributed protocols (e.g., leader election, coordination, consistency) becomes extremely slow and costly. The authors demonstrate that even simple coordination mechanisms require repeated reads and writes to storage, making large-scale distributed systems impractical under the FaaS model.

### 1.3 Proposed Future Directions

To unlock the cloud’s full potential, Hellerstein et al. argue that serverless must evolve beyond the current FaaS model toward a more flexible and performance-aware programming environment.

**Fluid code and data placement.**
The authors propose that cloud systems should support dynamic co-location of code and data. Instead of always shipping data to stateless functions, infrastructure should be able to move computation closer to where data resides when beneficial. High-level, declarative languages (e.g., SQL, MapReduce-style DSLs) could help the platform optimize placement decisions automatically.

**Long-running, addressable virtual agents.**
They advocate for persistent, logically identifiable software entities (e.g., actors or services) that can maintain state over time and communicate efficiently. These agents should remain elastic and be dynamically mapped to physical resources, but without the extreme ephemerality of current FaaS functions.

**Heterogeneous hardware support.**
Future cloud programming models should expose or intelligently utilize specialized hardware such as GPUs or accelerators. Developers should be able to express performance requirements or hardware affinity, while the platform manages allocation dynamically.

**Disorderly, scalable programming models.**
The authors emphasize the need for programming paradigms that naturally support asynchronous, distributed, and loosely consistent computation. Rather than sequential metaphors, cloud-native systems should encourage small, movable units of computation that scale across time and space.

Overall, they envision a programmable cloud that preserves autoscaling and pay-per-use economics while restoring efficient data processing, distributed coordination, and innovation capacity (Hellerstein et al., 2019).

---

## Part 2: Azure Durable Functions Deep Dive (5 topic explanations)

---

## Part 3: Critical Evaluation (400-600 words)

---

## References

All sources cited with working hyperlinks

---

## AI Disclosure Statement

State whether you used AI tools. If yes, describe how (e.g., "Used ChatGPT to help summarize Section 3 of the paper"). If no AI tools were used, state that explicitly.
