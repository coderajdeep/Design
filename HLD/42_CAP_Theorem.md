The **CAP theorem**, also known as **Brewer's theorem**, states that in a distributed data system, it’s impossible to simultaneously achieve all three of the following properties:

1. **Consistency (C)**: Every read receives the most recent write or an error. This means that all nodes in the system return the same data at any given time.
2. **Availability (A)**: Every request (read or write) receives a response, even if some nodes are unavailable. This ensures the system is always responsive.
3. **Partition Tolerance (P)**: The system continues to operate even if network partitions (communication breakdowns) occur, where some nodes cannot communicate with others.

According to the CAP theorem, in the presence of a _**network partition**_, a distributed system can support only **two of the three properties**: **Consistency** and **Availability** or **Consistency** and **Partition Tolerance**, but not all three.

### Understanding CAP with Examples

#### 1. **CP System (Consistency + Partition Tolerance)**
   - **Example**: **HBase** (an open-source, distributed database modeled after Google’s Bigtable).
   - **Explanation**: In HBase, if a partition occurs, the system prioritizes consistency over availability. It will reject read or write requests that might result in inconsistent data, ensuring that all replicas stay in sync. This can mean some requests may go unanswered during a network partition, trading availability for consistency.

#### 2. **AP System (Availability + Partition Tolerance)**
   - **Example**: **Cassandra** (a NoSQL database).
   - **Explanation**: Cassandra prioritizes availability and partition tolerance over strict consistency. If a network partition occurs, Cassandra continues to accept reads and writes, even if some nodes are unreachable. It later reconciles data across nodes through mechanisms like "eventual consistency," meaning the system might temporarily show stale data but will eventually be consistent once the partition is resolved.

#### 3. **CA System (Consistency + Availability)**
   - **Example**: Traditional **Relational Database Systems** (e.g., MySQL in a single-node setup).
   - **Explanation**: A single-node relational database can ensure consistency and availability because there’s no network partitioning within a single node. The data remains consistent, and all requests receive responses as long as the server is operational. However, this doesn’t scale well as a distributed system since it sacrifices partition tolerance when distributed across multiple nodes.

### Summary

In distributed systems:
- **CP systems** sacrifice **availability** during network partitions.
- **AP systems** sacrifice **consistency**, favoring **availability** with **eventual consistency**.
- **CA systems** are achievable only in non-distributed environments, as they cannot handle network partitions effectively.

Understanding the CAP theorem helps in choosing the right database and designing systems based on the priorities of consistency, availability, or partition tolerance, depending on the use case and requirements.