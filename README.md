# Olga Durham st#: 040687883

## CST8917 - Serverless Applications | Winter 2026

## Assignment 1: Serverless Computing - Critical Analysis

---

## Part 1: Paper Summary (400-600 words)

### 1.1 Main Argument (central thesis)

Hellerstein et al. (2019) argue that first-generation serverless platforms, particularly Functions-as-a-Service (FaaS), represent a significant operational improvement in cloud computing but ultimately fall short of enabling modern data-intensive and distributed applications. The authors describe this tension as "one step forward, two steps back". The "one step forward" refers to the autoscaling, pay-per-use model of FaaS platforms. Developers can upload event-driven functions without provisioning servers, and the platform automatically allocates and deallocates compute resources based on demand. This model reduces operational overhead, simplifies deployment, and offers seemingly unlimited elasticity.

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
