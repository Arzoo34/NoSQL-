## SINGLE SERVER

A single server deployment is the simplest distribution model where all data and all processing reside in one machine.

#### Why it matters in NoSQL
The single server model is the baseline against which all distribution models are compared. Understanding its limitations explains why distribution is necessary.

#### Key Characteristics:
- All data stored on one machine
- All reads and writes go to that machine
- No network coordinate overhead
- No replication or sharding complexity

#### Limitations:
- **Storage limit**: The machine's disk capacity caps total data volume
    
- **Throughput limit**: The machine's CPU, RAM, and I/O bandwidth cap request processing
    
- **Single point of failure**: If the machine fails, the entire system is unavailable
    
- **Vertical scaling ceiling**: You can only buy so big a machine

- A single server is simple but cannot scale beyond machines physical limits.

## SHARDING

Sharding (also called partitioning or horizontal partitioning) distributes different data across multiple servers. Each server acts as the single source for a subset of the data.

Instead of putting all data on single server, you split the data by some key (the shard key) and route each piece to a different server. Each server holds a distinct portion of the total dataset.

### How Sharding Works:

1. Choose a shard key -> a field or combination of fields that determines which shard stores each record.
2. Apply a routing function -> typically a hash function on the shard key. Records with the same shard key value go to the same shard.
3. Route Requests -> when a client request the data, the system computes the shard key, determines which shard holds that data, and routes the request there.

In Azure Cosmos DB, documents with the same partition key value are routed to and stored in the same physical partition. The partition key is a required document property that ensures predictable routing

### Key Characteristics

|Aspect|Description|
|---|---|
|**Data distribution**|Different data on different servers|
|**Query routing**|Requires knowledge of which shard holds which data|
|**Scalability**|Add more shards to handle more data|
|**Complexity**|Cross-shard queries are expensive or impossible|
## Hot Shards
A hot shard (or hot partition) occurs when the shard key chosen results in highly asymmetric storage or throughput patterns. One shard receives disproportionately more data or traffic than the others.
#### Storage hot shard
Occurs when one shard key value has much more data than others. Example: A multi tenant application using tenantid as shard key where one tenant is massive and reaches the 20GB logical partition limit, while other tenants are tiny.

#### Throughput hot shards:
Occurs when most or all requests go to the same logical partition. The shard key must distribute requests evenly across physical shards.

```text

Choosing a shard key with high cardinality (many distict values) helps avoid the 20 GB limit by spreading data across more logical partitions. Understanding access patterns before choosing a shard key is critical.
```
### Advantages and Disadvantages of Sharding

**Advantages**:

- **Near-linear scalability**: Add shards to handle more data and throughput
    
- **Fault isolation**: If one shard fails, others continue operating
    
- **Geographic distribution**: Shards can be placed in different regions
    

**Disadvantages**:

- **Cross-shard queries are expensive**: JOINs across shards are not practical
    
- **Hot shard risk**: Poor shard key choice creates bottlenecks
    
- **Operational complexity**: Managing many shards is harder than one server
    
- **Rebalancing complexity**: When adding shards, data must be redistributed

## Master-Slave Replication

In master-slave replication (also called leader-follower replication), one node is designated the master (leader) and handles all the write operations. One or more slave (follower) nodes replicate data from the master and may handle read operations.

```text
The master is the authoritative copy. Slaves synchroniz with the master to maintain copies of the data.
```

### How it works:
1. All write go to the master node.
2. The master generates an operation log (oplog) recording each write operation.
3. Slaves asynchronously replicate from the master by reading and applying operations from the master's oplog.
4. Slaves can serve read requests with their local copy of the data.
5. If the master fails, a slave can be elected as the new master.
### Key Characteristics

|Aspect|Description|
|---|---|
|**Write handling**|Only the master accepts writes|
|**Read handling**|Slaves can serve reads|
|**Replication**|Asynchronous (typically) — slaves may lag behind the master|
|**Consistency**|Eventually consistent (slaves may serve stale data)|
|**Failure handling**|Slaves participate in electing a new master if the master fails|
### Advantages

- **Reduces chance of update conflicts**: Since only one node accepts writes, there is no write-write conflict between nodes[](https://www.martinfowler.com/articles/nosqlKeyPoints.html)
    
- **Read scalability**: Reads can be distributed across slaves
    
- **Data durability**: Multiple copies of data reduce risk of data loss
    
- **Simple conflict model**: No need for complex conflict resolution
    

### Disadvantages

- **Write bottleneck**: All writes go to a single master, limiting write throughput
    
- **Single point of failure**: If the master fails before failover completes, writes are unavailable
    
- **Replication lag**: Slaves may serve stale data
    
- **Read-your-writes problem**: A client that writes to the master and then reads from a slave may not see its own write if the slave hasn't replicated yet

### 1.1 Read-Your-Writes Consistency
Read-your-writes consistency guarantees that a client can write a value and then immediately read that same value, seeing its own write

**Why it's a problem in master-slave replication**: If the client writes to the master and then reads from a slave that hasn't yet received the write, the client sees stale data.

**How it's solved**: The client obtains a **commit token** (or sync token) from the master after writing. When reading from a slave, the client passes this token, and the slave must be current enough to have applied that commit before serving the read

### 1.2Comparison with Peer-to-Peer

|Aspect|Master-Slave (Leader-Follower)|Peer-to-Peer|
|---|---|---|
|**Writes**|Only master accepts writes|Any node accepts writes|
|**Conflicts**|No write-write conflicts|Write-write conflicts possible|
|**Bottleneck**|Master is write bottleneck|No single write bottleneck|
|**Complexity**|Simpler conflict model|Complex conflict resolution needed|
|**Failure**|Master failure requires failover|Any node failure tolerated|

"Leader-follower replication reduces the chance of update conflicts but peer-to-peer replication avoids loading all writes onto a single point of failure"

## Peer-to-Peer Replication
In peer-to-peer replication (also called **multi-master replication** or **master-master replication**), **any node** can accept both **reads and writes**. Nodes coordinate to synchronize their copies of the data.

```text
There is no single authoritative master. All nodes are peers, and writes can be accepted anywhere.
``` 
### How It Works

**Step 1**: A client sends a write to any node in the cluster.

**Step 2**: That node writes the data locally and then replicates the write to other nodes.

**Step 3**: Other nodes apply the write to their local copies.

**Step 4**: Read requests can be served by any node.

**Step 5**: If two nodes receive conflicting writes concurrently, a conflict resolution mechanism is needed.
### Key Characteristics

|Aspect|Description|
|---|---|
|**Write handling**|Any node accepts writes|
|**Read handling**|Any node serves reads|
|**Replication**|Asynchronous (typically) — nodes may have different data at any moment|
|**Consistency**|Eventually consistent — nodes converge over time|
|**Conflicts**|Write-write conflicts are possible and must be resolved|
### Advantages

- **No single point of failure**: Any node can fail without losing write capability
    
- **No write bottleneck**: Writes distributed across all nodes
    
- **Geographic distribution**: Nodes can be placed close to users for lower latency
    
- **High availability**: System remains available even if some nodes fail
    

###  Disadvantages

- **Write-write conflicts**: Two nodes may receive conflicting updates to the same data simultaneously
    
- **Complex conflict resolution**: Need mechanisms to detect and resolve conflicts
    
- **Eventual consistency only**: Strong consistency is difficult to maintain
    
- **More complex to reason about**: Application logic must handle potential inconsistencies

### Conflict Resolution Strategies

**Last Write Wins (LWW)**: The update with the latest timestamp wins. Simple but can lose data.

**Version-based resolution**: Each data item has a version number. Conflicts are detected by comparing versions.

**Application-assisted resolution**: The server exposes conflicting values to the application, and the application resolves conflicts using domain-specific business logic.
### Examples

- **Cassandra**: Uses peer-to-peer replication with eventual consistency[](https://browse.library.kiwix.org/content/stackoverflow.com_en_all_nopic_2022-07/questions/49062198/multi-master-vs-peer-to-peer-db-architectures#1)
    
- **CouchDB**: Supports peer-to-peer replication where any node can accept writes[](https://stackoverflow.com/feeds/question/5460183#1)
    
- **Riak**: Peer-to-peer replication with vector clocks for conflict detection

## Combining Sharding and Replication

Combining sharding and replication means using both techniques together in the same system: data is sharded (split into partitions across servers) and each shard is replicated (copied to multiple servers for durability and read scalability).

```
Sharding handles data volume and write throughput; replication handles availability, durability, and read throughput.
```
### How It Works

**Step 1**: Data is partitioned into shards based on a shard key.

**Step 2**: Each shard is replicated across multiple nodes.

**Step 3**: One replica of each shard acts as the master (for leader-follower within the shard), or all replicas can accept writes (for peer-to-peer within the shard).

**Step 4**: Clients route to the correct shard based on the shard key, then to the appropriate replica.

## Consistency

#### Update Consistency (Write Consistency)
Update consistency (also called **write consistency**) concerns what happens when **two clients try to write the same data at the same time**. These are called **write-write conflicts**

###### **Types of write conflicts**:

**Write-write conflict**: Two clients update the same record simultaneously. The system must decide which update wins (or whether to reject one).

**Pessimistic approach**: Lock the data record before writing, preventing other clients from writing until the lock is released. This prevents conflicts but reduces concurrency 

**Optimistic approach**: Allow writes to proceed, detect conflicts after the fact, and resolve them (e.g., reject one write, merge changes, or ask the application to resolve)

#### Read Consistency
Read consistency concerns what a client sees when reading data. **Read-write conflicts** occur when one client reads data **in the middle of another client's write**
**The problem**: In a distributed system, different nodes may have different versions of the same data at any given moment. A read from one node may return a different value than a read from another node.

**Types of read inconsistency**:

**Read-read inconsistency**: Two clients read the same key at the same time but get different values. This happens when replicas are not synchronized

**Read-write inconsistency**: A client reads data that is partially updated or inconsistent with a concurrent write

### Read-Your-Writes Consistency 

**Definition**: A client that performs a write should be able to immediately read that same write, seeing its own update.

**Why it matters**: In a distributed system, a client writes to one node and reads from another. If the second node hasn't received the write yet, the client sees stale data — a confusing and often incorrect behavior.

**Solution**: The client passes a **commit token** or **sync token** with the read request. The replica uses this token to determine if it is current enough to serve the read
### Monotonic Reads

**Definition**: Monotonic reads guarantee that a user sees an **increasing version** of a data item. Any successive read on a data item should return a newer version or a previously observed version — never an older version [](https://theses.hal.science/tel-01359621/file/ThKUMARSathiya.pdf#23#9).

**How it works**: The client passes its last observed version of the data item with each read. The server ensures it does not return a version older than the one the client has already seen

## Relaxing Consistency
 Relaxing consistency means **deliberately weakening** the consistency guarantees of a system in exchange for **better availability, lower latency, or higher scalability**.

```
Strong consistency requires coordination between nodes, which adds latency. Relaxing consistency removes that coordination, improving performance at the cost of potentially returning stale data.
```

### Eventual Consistency

Eventual consistency means that if no new updates are made, **all replicas will eventually converge** to the same value. However, at any given moment, different replicas may return different values

```
"At some point the system will become consistent once all the writes have propagated to all the nodes"
```

### Adaptive Consistency

Adaptive consistency means using **different consistency levels for different operations** depending on their criticality. Instead of relying on a single consistency protocol, the system mixes eventual consistency with stronger options as needed

```
TACT (Tunable Availability and Consistency Trade-offs) is a toolkit that allows services to dynamically choose their availability/consistency trade-offs using three metrics: unseen writes, uncommitted writes, and staleness
```

## CAP Theorem
Any networked shared-data system can have only two of three desirable properties simultaneously:
1. Consistency (C) : Every read receives the most recent write or error.
2. Availability (A): Every request receives a response, without guarantee that it contains the most recent write.
3. Partition Tolerance (P): The system continues to operate despite network partitions (communication breakdown between nodes).
### The Three Combinations

**CA (Consistency + Availability)**: The system provides strong consistency and availability but **cannot tolerate network partitions**. This is only achievable in a single-node system or a system where partitions never occur. Traditional relational databases on a single server are CA systems

**CP (Consistency + Partition Tolerance)**: The system provides strong consistency and partition tolerance but sacrifices **availability**. During a partition, some nodes may become unavailable to preserve consistency. Examples: HBase, MongoDB (with strong consistency settings), Redis (with strong consistency).

**AP (Availability + Partition Tolerance)**: The system provides availability and partition tolerance but sacrifices **strong consistency**, offering only **eventual consistency**. During a partition, all nodes remain available but may return stale data. Examples: Cassandra, CouchDB, DynamoDB.

### Why CAP Matters for NoSQL

**Key insight (Brewer)**: By explicitly handling partitions, designers can optimize consistency and availability, achieving some trade-off of all three. The CAP theorem is not a strict "pick two" — it's a framework for understanding trade-offs 

**Practical implication**: Since network partitions are unavoidable in distributed systems, real-world distributed databases must choose between **CP** and **AP**. CA is only possible on a single machine.

```
The CAP theorem states that if you get a network partition, you have to trade off availability of data versus consistency
```

## Relaxing durability

Relaxing durability means **accepting the risk that some committed writes may be lost** in exchange for **lower latency** or **higher availability**.

**Traditional durability**: In ACID, durability guarantees that once a transaction commits, it survives system failures. This is typically achieved by writing to disk before acknowledging the commit.

**Relaxed durability**: The system may acknowledge a write before it is safely persisted to disk (or replicated to enough nodes). If the system fails before the write is persisted, the write is lost.
### Why Relax Durability

**Latency**: Waiting for disk writes (or replication to multiple nodes) adds latency. Acknowledging writes earlier improves response time.

**Throughput**: Less durability coordination means higher write throughput.

**Trade-off**: You accept a small risk of data loss in exchange for better performance.

## QUORUMS
A quorum is the **minimum number of nodes** that must agree on a read or write operation for it to be considered successful.
```
Instead of requiring all replicas to participate in every operation (which is slow and fragile), you require only a subset — a quorum — to participate.
```

### Quorums Rules:
For a system with N replicas:
1. Write quorums (W): The number of replicas that must acknowledge a write for it to succeed.
2. Read Quorums (R): The number of replicas that must respond to a read.

```
Key rule for strong consistency: R + W > N ensures that at least one node in every read quorum has seen every write quorum
```

### Quorum Systems

**Definition**: A quorum system is a set of quorums such that **every two quorums intersect**. This intersection property ensures that information written to one quorum can be read by another

**Minimal quorum system**: No quorum is a proper subset of another.

**Majority quorum system**: Every quorum has `floor(n/2) + 1` nodes. This is the simplest and most common quorum system

## Version Stamps

A version stamp is a **unique identifier** attached to a data item that indicates its **version** or **state** at a particular point in time.

Version stamps help **detect concurrency conflicts**. When you read data and then update it, you can check the version stamp to ensure nobody else updated the data between your read and your write

### How Version Stamps Work

**Step 1**: Client reads a data item and receives its version stamp.

**Step 2**: Client modifies the data item.

**Step 3**: Client sends the write with the original version stamp.

**Step 4**: The system checks: has the version stamp changed since the client read it? If yes, someone else updated the data — conflict detected. If no, the write succeeds and the version stamp is updated.

## Business and System Transactions

#### 1. Business Transactions
A business transaction is a **logical unit of work** that represents a meaningful business operation, such as "place an order" or "transfer funds"

**Characteristics**:

- Long-lived (seconds, minutes, hours, or days)
    
- May span multiple systems and services
    
- Cannot always be implemented as a single ACID transaction
    
- Often uses **sagas** or **compensating transactions** for rollback
    

**Example**: Booking a trip involves reserving a flight, reserving a hotel, and charging a credit card. These are separate operations that together form a business transaction.

#### 2. System Transactions
A system transaction is a **technical unit of work** that ensures data consistency at the database level. It is typically short-lived and ACID-compliant.

**Characteristics**:

- Short-lived (milliseconds)
    
- Usually confined to a single database or system
    
- ACID-compliant
    
- Managed by the database's transaction manager

**Example**: Debiting one account and crediting another within the same database.
### The Relationship

**Business transactions** may be composed of **multiple system transactions**. The system transactions handle technical consistency; the business transaction handles logical consistency across systems.

**Exam Point**: Fowler notes that business transactions are long-lived and cannot always be implemented as simple ACID transactions. Advanced transaction concepts like **sagas** and **nested transactions** are used

## Version Stamps on Multiple Nodes
### The Problem

In a **single-node** system, version stamps can be simple counters. In a **multi-node** system, different nodes may have different versions of the same data, and a simple counter doesn't capture **which node** made which update.
### Vector Clocks

**Definition**: A vector clock is a **vector of version stamps**, one per node. Each node maintains its own counter and includes other nodes' counters when communicating.

**How it works**:

- Node A's vector clock might be `{A: 5, B: 3, C: 2}`
    
- Node B's vector clock might be `{A: 4, B: 7, C: 2}`
    
- Comparing these vectors reveals that B has updates A hasn't seen (B=7 > 3), and A has updates B hasn't seen (A=5 > 4). This means the updates are **concurrent** — a conflict exists.