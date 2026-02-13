# Olga Durham st#: 040687883

## CST8917 - Serverless Applications | Winter 2026

## Assignment 1: Serverless Computing - Critical Analysis

---

## Part 1: Paper Summary

### 1.1 Main Argument (central thesis)

Hellerstein et al. (2019) argue that first-generation serverless platforms, particularly Functions-as-a-Service (FaaS), represent a significant operational improvement in cloud computing but ultimately fall short of enabling modern data-intensive and distributed applications. The authors describe this tension as "one step forward, two steps back". The "one step forward" refers to the autoscaling, pay-per-use model of FaaS platforms. Developers can upload event-driven functions without provisioning servers, and the platform automatically allocates and deallocates compute resources as needed. This model reduces operational overhead, simplifies deployment, and offers seemingly unlimited elasticity.

However, the “two steps back” reflect architectural limitations that undermine the broader potential of cloud computing. According to the authors, current FaaS systems are built around short-lived, stateless, and isolated function invocations. While this design supports elasticity and management simplicity, it ignores key requirements of modern computing, especially efficient data processing, hardware acceleration, and distributed coordination. As a result, serverless platforms are well-suited for simple, embarrassingly parallel tasks but poorly suited for complex, stateful, and data-rich systems.

### 1.2 Key Limitations Identified

The authors identify several critical limitations of current FaaS platforms.

- **Execution time constraints:** Functions in platforms like AWS Lambda have strict lifetime limits (e.g., 15 minutes). Because function instances are ephemeral and cannot assume they will be reused, all persistent state must be stored externally. This makes long-running analytics, iterative machine learning training, and stateful workflows inefficient and fragmented across multiple invocations (Hellerstein et al., 2019).

- **Communication and network limitations:** FaaS functions are not directly network-addressable. They cannot communicate via low-latency messaging and must instead exchange data through storage services such as S3 or DynamoDB. The authors demonstrate that this introduces significant I/O bottlenecks, limited per-function bandwidth, and high latency. As concurrency increases, effective bandwidth per function decreases, making distributed coordination costly and slow.

- **The "data shipping" anti-pattern:** Because computation is separated from data, FaaS platforms require data to be moved to stateless functions rather than placing computation near the data. The authors argue that this contradicts established database design principles, where minimizing data movement improves performance, reduces cost, and lowers latency.

- **Limited hardware access:** FaaS platforms offer only generic virtualized CPU and memory slices, with no support for GPUs or specialized accelerators. Given the growing importance of hardware acceleration in machine learning and large-scale data processing, this restriction limits innovation and performance.

- **Challenges for distributed and stateful workloads:** Since functions cannot maintain identity or persistent communication channels, implementing distributed protocols such as leader election or consensus becomes extremely inefficient. Coordination requires repeated reads and writes to remote storage, dramatically increasing latency and cost.

### 1.3 Proposed Future Directions

To overcome these limitations, Hellerstein et al. (2019) propose a more advanced cloud programming model. They advocate for fluid code and data placement, allowing infrastructure to dynamically co-locate computation with data when beneficial. They call for support for heterogeneous hardware, enabling workloads to leverage GPUs and specialized accelerators. The authors also propose long-running, addressable virtual agents that maintain identity and communicate efficiently while remaining elastic. Finally, they emphasize the need for programming models designed for asynchronous, distributed environments rather than strictly sequential execution.

Overall, the paper argues that while serverless computing improves operational simplicity, it must evolve significantly to unlock the full distributed and data-centric potential of the cloud.

---

## Part 2: Azure Durable Functions Deep Dive (5 topic explanations)

### Orchestration Model

Azure Durable Functions extends basic Azure Functions by introducing a structured workflow model built on the Durable Task Framework (Microsoft, 2024a). Instead of isolated, stateless function invocations, Durable Functions separates execution into three roles: **client functions**, which start workflows; **orchestrator functions**, which define the workflow logic; and **activity functions**, which perform individual tasks. The orchestrator coordinates execution by scheduling activities and managing dependencies between them.

This model differs significantly from traditional FaaS, where each function invocation runs independently without built-in coordination. By introducing an explicit orchestration layer, Durable Functions addresses Hellerstein et al.'s (2019) criticism that first-generation serverless platforms lack structured composition for distributed workflows. However, orchestration logic is still persisted in underlying storage for reliability, meaning coordination remains storage-backed rather than direct peer-to-peer communication.

### State Management (Event Sourcing, Checkpointing, Replay)

Unlike basic FaaS, where functions are fully stateless and must manually externalize all data, Azure Durable Functions provides built-in state management through event sourcing and checkpointing (Microsoft, 2024b). Orchestrator functions do not store state in memory between executions. Instead, every action, such as scheduling an activity or receiving a result, is recorded in durable storage. When the function resumes, the runtime replays the stored execution history to reconstruct the orchestration state deterministically.

This checkpoint-and-replay model allows workflows to persist across restarts and recover from failures, enabling long-running, stateful processes. In this sense, Durable Functions partially addresses Hellerstein et al.’s (2019) criticism that FaaS platforms are fundamentally stateless. However, state persistence still relies on external storage, meaning the architecture continues to separate computation from data rather than colocating them.

### Execution Timeouts

Traditional Azure Functions, like other FaaS platforms, are subject to execution time limits depending on the hosting plan. Hellerstein et al. (2019) criticize similar limits in AWS Lambda (e.g., 15 minutes), arguing that they prevent long-running and iterative workloads. Azure Durable Functions mitigates this limitation through its checkpointing mechanism. Orchestrator functions do not run continuously for long periods; instead, they execute briefly, persist their state to storage, and then unload. When triggered again, the runtime replays the execution history and resumes from the last checkpoint (Microsoft, 2024a).

This design allows Durable workflows to run for hours, days, or even longer without being constrained by a single execution window. However, individual activity functions are still subject to standard Azure Function timeout limits. Therefore, Durable Functions works around execution limits at the workflow level but does not eliminate them entirely.

### Communication Between Functions

In Azure Durable Functions, communication between orchestrator and activity functions does not occur through direct network messaging. Instead, interactions are coordinated through the Durable Task Framework, which persists execution history and task messages in Azure Storage (typically using queues and tables) (Microsoft, 2024b). When an orchestrator schedules an activity, the request is written to storage, and the activity retrieves it asynchronously. Results are also written back to storage, where the orchestrator reads them during replay.

This design improves reliability and fault tolerance but still relies on storage-mediated communication. Hellerstein et al. (2019) argue that first-generation FaaS platforms force functions to communicate through slow storage intermediaries rather than direct, low-latency networking. Durable Functions does not fundamentally eliminate this architecture; it formalizes and manages it. Therefore, while communication is structured and automated, it still inherits the performance trade-offs identified in the paper.

---

## Part 3: Critical Evaluation (400-600 words)

---

## References

- Hellerstein, J. M., Faleiro, J., Gonzalez, J. E., Schleier-Smith, J., Sreekanti, V., Tumanov, A., & Wu, C. (2019). *Serverless computing: One step forward, two steps back.* CIDR.
https://www.cidrdb.org/cidr2019/papers/p119-hellerstein-cidr19.pdf
- Microsoft. (2024). *Durable Functions overview.*
https://learn.microsoft.com/azure/azure-functions/durable/durable-functions-overview
- Microsoft. (2024). *Orchestrator function code constraints and behavior.*
https://learn.microsoft.com/azure/azure-functions/durable/durable-functions-code-constraints
- Microsoft. (2024). *Durable Functions application patterns.*
https://learn.microsoft.com/azure/azure-functions/durable/durable-functions-overview#application-patterns
- Microsoft. (2024). *Durable Functions internals (storage and event sourcing).*
https://learn.microsoft.com/azure/azure-functions/durable/durable-functions-serialization-and-persistence
- Microsoft. (2024). *Durable Functions reliability and long-running workflows.*
https://learn.microsoft.com/azure/azure-functions/durable/durable-functions-overview#reliability
- Microsoft. (2024). *Fan-out/fan-in pattern in Durable Functions.*
https://learn.microsoft.com/azure/azure-functions/durable/durable-functions-cloud-backup

---

## AI Disclosure Statement

State whether you used AI tools. If yes, describe how (e.g., "Used ChatGPT to help summarize Section 3 of the paper"). If no AI tools were used, state that explicitly.
