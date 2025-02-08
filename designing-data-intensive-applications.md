## Designing Data Intensive Applications

### Reliability

<ins>Fault Tolerance</ins>

Refers to a system’s ability to continue functioning correctly when some of its components have faults. This capability is integral to designing reliable systems, which aim to deliver consistent and expected performance despite hardware faults, software bugs, or human errors.

Key principles of fault tolerance and reliability include:
1. Faults vs. Failures:  
    - A fault is a deviation of a system component from its specification, while a failure occurs when the entire system fails to deliver the required service. Fault tolerance focuses on preventing faults from escalating into failures.
2. Building Reliable Systems from Unreliable Components:  
    - Distributed systems often consist of unreliable components. By employing good error handling, redundancy, and fault-handling protocols, systems can maintain reliability beyond the sum of their parts.
3. Testing Fault Tolerance:  
    - Techniques like deliberately inducing hardware and software faults during testing (chaos engineering) ensure the system’s fault-tolerance mechanisms are effective under realistic conditions.
4. Cascading effect:  
    - Software faults can have a cascading effect, triggering other faults in the system.

<ins>Error handling</ins>

1. Functional Error Handling:  
    - **Many critical bugs stem from poor error handling**, emphasizing the importance of handling errors at a functional and component level.
    - Strong typing
    - Validation: input and return types/shapes, value boundaries (for example check number is within allowed range/size)
2. Network Faults:  
    - Communication over networks is inherently unreliable, and error handling is crucial for managing scenarios like lost, delayed, or corrupted packets.
3. DB Transactional Error Handling:  
    - Transactions can be aborted and retried safely if an error occurs, aligning with the principles of ACID (Atomicity, Consistency, Isolation, Durability) databases. This ensures the database does not end up in an inconsistent state.  
    - Retrying aborted transactions is a fundamental error-handling mechanism but has challenges, such as deduplication of actions if retries inadvertently duplicate successful transactions.

<ins>Redundancy</ins>

Refers to the intentional duplication of critical system components or functions to enhance reliability and fault tolerance. Its main purpose is to ensure that a system can continue functioning despite individual component failures.  
It is mostly used at the hardware level and is critical in large-scale systems where hardware faults are inevitable.

<ins>Process Isolation</ins>

Refers to separating running processes from one another to ensure that a failure or malfunction in one process does not affect others or compromise the system. This approach enhances fault tolerance and system reliability by enforcing boundaries between processes.

1. Encapsulation:  
    - Processes operate independently, with their own memory and execution contexts, which ensures that issues in one process do not propagate to others.
2. Failure Containment:  
    - By isolating processes, failures such as deadlocks or infinite loops in one process can be handled or terminated without impacting the rest of the system. This principle is critical in distributed systems where nodes or components may fail unpredictably.
3. Concurrency and Isolation:  
    - Isolation is particularly relevant in environments with multiple concurrent transactions or tasks, as it prevents interference and maintains system integrity.
4. Tools and Mechanisms:  
    - Systems use mechanisms like virtual machines, containers, and process managers to enforce isolation, providing an abstraction layer that separates processes while allowing them to interact securely when needed.

<ins>Improving System Reliability</ins>

1. Error Handling
2. Rigorous Testing:  
    - Automated testing at multiple levels, from unit to integration tests.  
    - Identify critical flows and implement coverage for the steps within them.  
    - Implement chaos testing inside a controlled environment to test fault tolerance.  
3. Process Isolation
4. Monitoring and Telemetry:  
    - Implement detailed monitoring to provide early warnings of potential issues. Metrics such as performance indicators and error rates are invaluable for diagnosing problems and preventing failures.  
    - Create a system of alerts that will notify of possible faults in the system.

---

### Scalability

<ins>System Load</ins>

Describes the current workload that a system is handling, often measured in terms of specific parameters related to the architecture and operations including: CPU load, memory usage, database reads and writes, network bandwidth, number of users.

Accurately measuring and describing load is a prerequisite for evaluating scalability and system performance.

<ins>Factors that can affect system load</ins>

1. High Request Volume:  
    - A surge in the number of requests per second, such as web traffic spikes or concurrent database queries, directly increases system load.
2. Fan-Out Operations:  
    - In systems like social media, where one operation (e.g. a post) triggers numerous downstream processes (e.g. delivering the post to all followers), fan-out can lead to significant load amplification ￼.
3.	Concurrency:  
    - An increase in the number of simultaneous users or connections, especially in applications like chat systems or online gaming, raises system demands.
4. Data Volume Growth:   
    - Larger datasets require more resources for storage, retrieval, and processing. Operations that were once fast may slow down as data scales up.
5. Backend Dependencies:  
    - Systems relying on multiple backend services may experience increased load due to delays or failures in one of those services, amplifying latency and causing queuing delays.
6. Contention for Resources:  
    - Limited CPU, memory, or I/O resources can become bottlenecks when load increases, leading to higher queuing times and degraded performance.
7. Unexpected Traffic Patterns:  
    - Unpredictable spikes caused by special events, promotions, or errors (e.g. retries from poorly designed clients) can quickly overload a system.

<ins>Measuring system load</ins>

1. Identify Load Parameters:  
    - Select metrics that are most relevant to the system’s architecture and primary functions: eg. requests per second, database read-to-write ratio, number of concurrent users, cache hit rate.
2. Understand Load Characteristics:  
    - Evaluate the distribution of load. For instance, some users or operations may account for disproportionately high demands (e.g. post from user with many followers).
3. Analyze Read vs. Write Load:  
    - Assess the balance of read and write operations. Systems often need to optimize for one depending on the workload. For example, write-heavy systems might distribute writes across replicas, while read-heavy systems might use caching strategies
4. Account for Variability:  
    - Measure not only average load but also peak and worst-case scenarios to ensure the system can handle spikes. For instance, analyzing request throughput during peak hours provides insights into system stress levels.  
    - Use percentiles to describe variability.

<ins>Effect of high load on fault tolerance</ins>

System load impacts fault tolerance significantly, as increasing load can exacerbate system fragility and complicate fault recovery. Here are the key effects and considerations:

1. Timeouts and Load Spikes:  
    - Under high load, nodes may experience delays, causing slower responses. Short timeouts may misinterpret these delays as failures, triggering unnecessary errors or retries. This misinterpretation can increase the load further, potentially leading to cascading failures.
2. Cascading Failures:  
    - When a node is declared dead prematurely due to load-induced delays, its responsibilities may be transferred to other nodes. If these nodes are also near capacity, the additional load can overwhelm them, leading to a chain reaction of failures.
3. Error Recovery Complexity:  
    - High system load increases the likelihood of faults occurring during critical operations. For example, retries in distributed transactions under load might lead to duplicate operations or inconsistent states if not handled carefully.
4. Resource Contention:  
    - High load can cause resource contention, such as CPU or memory exhaustion, impacting a system’s ability to recover from faults. Systems under load may spend excessive time on recovery rather than serving new requests.
5. Fault-Tolerant Design:  
    - Systems must be designed to handle load gracefully, employing strategies like load shedding, queuing mechanisms, and rate-limiting. Proper design helps maintain fault tolerance even under stress.

<ins>Methods to handle increased system load</ins>

1.	Scaling Approaches:
    - Vertical Scaling (Scaling Up): Upgrading the existing machine with more CPU, RAM, or storage to handle more load.
    - Horizontal Scaling (Scaling Out): Distributing load across multiple machines, often using load balancers and distributed databases.
    - Elastic systems: systems that automatically scale with increasing load.
2.	Caching:
    - Application-Level Caching: Storing frequently accessed data in-memory (e.g., Redis, Memcached) to reduce database load.
    - Content Delivery Networks (CDNs): Serving static files from geographically distributed edge locations.
3.	Load Balancing:
    - Distributing requests across multiple servers to prevent any single point from becoming overwhelmed.
4.	Database Optimization:
    - Indexing: Reducing query response time.
    - Sharding: Distributing database tables across multiple servers.
    - Replication: Creating read replicas to offload database read queries.
5.	Asynchronous Processing:
    - Message Queues: Decoupling services using Kafka, RabbitMQ, or SQS to prevent synchronous bottlenecks.
    - Batch Processing: Deferring non-urgent operations to off-peak hours.
6.	Rate Limiting and Throttling:
    - Preventing API abuse by capping the number of requests from a single user or service.

<ins>SLA, SLO<ins>

A Service Level Agreement (SLA) is a formal contract between a service provider and its customers that outlines the expected level of service.

A Service Level Objective SLO is a specific, measurable target within an SLA.  It defines the performance goals that the service provider strives to meet over a defined period.
Measurable targets could include: response times (in percentiles), error rates, uptime percentages.

---

### Maintainability

Maintainability ensures that a system remains easy to manage, debug, and modify over time. The primary principles include:

1. Operability (Ease of Operations):  
    - Ensure systems are easy to monitor, deploy, and troubleshoot.
    - Provide logging, monitoring, and alerting mechanisms to track system health.
    - Automate maintenance tasks, such as backups and failover processes.
2. Simplicity (Reducing Complexity):  
    - Favor clear, modular, and well-structured designs over overly complex architectures.
    - Use consistent naming conventions and coding patterns.
    - Remove unnecessary dependencies and avoid tight coupling between components.
3. Evolvability (Ease of Making Changes):  
    - Design systems to accommodate future modifications with minimal disruption.
    - Use versioned APIs and backward-compatible data schemas.
    - Implement automated tests to verify changes without breaking existing functionality.
4. Documentation and Knowledge Sharing:  
    - Maintain up-to-date documentation on system architecture, configurations, and operational procedures.
    - Use code comments, README files, and internal wikis for team knowledge sharing.
    - Train engineers on system design principles and best practices.
5. Resilience and Fault Handling:  
    - Design for graceful degradation, where partial failures do not take down the entire system.
    - Implement rollback mechanisms for rapid recovery from faulty deployments.
    - Use circuit breakers and retries to handle transient failures effectively.

---