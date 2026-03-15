## Kafka Fundamentals

### What is Apache Kafka
- Distributed event streaming platform capable of handling trillions of events a day.
- Originally developed at LinkedIn, now open-source under Apache Foundation.
- Combines messaging, storage, and stream processing in one system.
- Provides high throughput, low latency, fault tolerance, and scalability.
- Decouples data producers and consumers through a publish-subscribe model.

### Event streaming vs messaging systems
- Traditional messaging systems (e.g., RabbitMQ) deliver messages and delete them after consumption; Kafka retains messages based on retention policy.
- Kafka supports replayability of events, allowing consumers to re-read historical data.
- Messaging systems often use queues with point-to-point delivery; Kafka uses topics with multiple consumer groups for pub/sub and queue semantics.
- Kafka offers stronger ordering guarantees within partitions compared to many traditional brokers.
- Event streaming treats every piece of data as an immutable event, enabling event-driven architectures.

### Kafka architecture overview
- Kafka cluster consists of multiple brokers (servers) that store data in topics.
- Topics are divided into partitions for parallelism and scalability.
- Producers write data to topics; consumers read from topics.
- ZooKeeper (legacy) or KRaft (modern) manages cluster metadata and controller election.
- Replication ensures data durability by copying partitions across brokers.

### Kafka use cases
- Real-time data pipelines and streaming ETL.
- Metrics and log aggregation (e.g., collecting server logs).
- Microservices communication (event-driven architecture).
- Commit log for distributed systems (e.g., change data capture).
- Stream processing with Kafka Streams or ksqlDB for real-time analytics.

### Kafka ecosystem components
- Kafka Connect: Framework for integrating external systems (databases, cloud services).
- Kafka Streams: Lightweight stream processing library for Java applications.
- ksqlDB: Event streaming database for stream processing with SQL.
- Schema Registry: Manages and enforces data schemas (Avro, Protobuf, JSON).
- REST Proxy: Exposes Kafka over HTTP for non-Java clients.

### Kafka vs RabbitMQ / Pulsar / ActiveMQ
- RabbitMQ: General-purpose message broker, good for complex routing, but lower throughput; Kafka excels at high-throughput event streaming.
- Pulsar: Offers native geo-replication, multi-tenancy, and separate storage/compute layers; Kafka's architecture is simpler but evolving (e.g., KRaft, tiered storage).
- ActiveMQ: JMS-compliant, supports transactions and XA; Kafka is non-JMS, focuses on stream processing and replayability.
- Kafka provides stronger durability and ordering guarantees within partitions compared to traditional brokers.
- Pulsar and Kafka both support exactly-once semantics; Kafka's transactions are more mature in production.

### Kafka terminology (topic, partition, broker, offset)
- Topic: Logical channel where messages are published; categorized by name.
- Partition: Unit of parallelism within a topic; messages in a partition are ordered and assigned an offset.
- Broker: Kafka server that stores partitions and serves client requests.
- Offset: Unique sequential ID assigned to each message within a partition, used to track consumption progress.
- Consumer group: Set of consumers that collaborate to consume messages from topics, each partition assigned to one consumer in the group.

### Kafka message structure (key/value/header)
- Message (record) consists of key, value, timestamp, and optional headers.
- Key is used for partitioning (same key goes to same partition) and can be null.
- Value contains the actual data payload (may be serialized).
- Headers are key-value pairs for metadata (e.g., tracing info, content type).
- Timestamp can be set by producer or broker; used for log retention and windowing.

### Kafka log-based architecture
- Kafka stores messages in a commit log on disk, organized as segments.
- New messages are appended to the end of the log (sequential write).
- Consumers read from any offset, allowing replay of old data.
- Logs are immutable – messages cannot be modified after writing.
- This design enables high write throughput and efficient reads via page cache.

### Immutable commit log
- Once a message is written to a partition, it cannot be changed.
- Immutability simplifies replication and ensures consistency across replicas.
- Enables multiple consumers to read independently without contention.
- Supports event sourcing and auditing by preserving history.
- Retention policies delete old segments, not individual messages.

### Retention policies
- Time-based: Messages older than a threshold (e.g., 7 days) are deleted.
- Size-based: When log size exceeds limit, oldest segments are removed.
- Compacted retention: Keeps the latest message for each key, removing older duplicates.
- Can be combined (e.g., time + size) – whichever limit is reached first.
- Deletion is lazy; segments are closed and removed asynchronously.

### Log compaction
- Retention mechanism that retains at least the last known value for each key.
- Useful for restoring state (e.g., table snapshots) or changelog topics.
- Compaction runs in the background, discarding older records with same key.
- Does not guarantee all keys are kept if deleted (tombstone markers).
- Ensures that consumers reading from the start of the log see the latest state.

### Ordering guarantees
- Messages within a partition are ordered as they are appended.
- Across partitions, no global ordering; keys ensure ordering for same key.
- Producers can use idempotent delivery to avoid duplicates while preserving order.
- Consumers read messages in order from each partition assigned.
- To maintain total order, use single partition (limits parallelism).

### Throughput and latency characteristics
- Kafka achieves high throughput (millions of messages/sec) via batching, compression, zero-copy, and sequential I/O.
- Latency can be low (milliseconds) when acks=0 or 1; higher with acks=all.
- Batching introduces some latency to increase throughput (configurable linger.ms).
- End-to-end latency depends on producer, broker, consumer, and network.
- Tuning batch size, compression, and replication factor balances throughput and latency.

## Core Concepts

### Topics and partitions
- Topic is a logical stream; partitions are physical logs that allow parallel processing.
- Number of partitions determines max parallelism for consumers.
- Each partition is an ordered, immutable sequence of records.
- Partitions are distributed across brokers for load balancing and fault tolerance.
- Choosing partition count affects performance, ordering, and rebalance time.

### Partitioning strategy
- Messages with same key go to same partition (via hashing) to maintain order.
- If no key, round-robin or sticky partitioner distributes load.
- Custom partitioners can route based on message attributes.
- Partition count should be decided based on expected throughput and consumer count.
- Changing partition count after creation may affect key ordering.

### Key-based partitioning
- Producer uses key to determine target partition (hash(key) % numPartitions).
- Ensures all messages for a key are ordered within that partition.
- Enables stateful stream processing (e.g., aggregations per key).
- If key is null, producer uses default partitioner (round-robin or sticky).
- Key can be used for log compaction to keep latest value per key.

### Partition leadership
- Each partition has one leader broker and multiple followers (replicas).
- All reads and writes go to the leader to maintain consistency.
- Followers replicate data from leader and stay in sync.
- If leader fails, a new leader is elected from in-sync replicas (ISR).
- Leadership balances automatically via controller or manual reassignment.

### Replication factor
- Number of replicas for each partition (including leader), e.g., 3.
- Higher replication improves durability and availability at cost of storage and network.
- Replicas are placed on different brokers (and racks) to tolerate failures.
- Replication factor cannot exceed number of brokers.
- Producers can set acks to control durability: acks=all waits for all replicas.

### In-sync replicas (ISR)
- Set of replicas that are fully caught up with the leader.
- A follower is in ISR if it has not fallen behind beyond configured lag.
- Only ISR members are eligible to become leader.
- If a follower fails or lags, it is removed from ISR until it catches up.
- Configurable min.insync.replicas ensures minimum ISR size for writes.

### Leader and follower roles
- Leader handles all client requests for a partition.
- Followers replicate data asynchronously (or synchronously based on configuration).
- Followers serve no reads by default (though consumer reads can be allowed from followers with rack awareness).
- Leader maintains HW (high watermark) and LEO (log end offset) for itself and followers.
- Followers fetch from leader and update their own log.

### Controller node
- One broker in the cluster acts as controller, managing partition leadership.
- Monitors broker health and reassigns leadership when brokers fail.
- Handles partition reassignments and topic creation/deletion.
- In legacy Kafka, controller uses ZooKeeper for coordination.
- In KRaft mode, controller role is merged into metadata quorum.

### Broker responsibilities
- Store partitions assigned to it, manage log segments.
- Serve produce/fetch requests from clients.
- Participate in leader election and replica synchronization.
- Track HW and ISR for partitions it leads or follows.
- Communicate with controller for metadata updates.

### Offset management
- Each message has a unique offset within its partition.
- Consumers commit offsets to mark processed messages.
- Offsets are stored in internal topics (__consumer_offsets) or external stores.
- Offset resets define behavior when no committed offset exists (e.g., earliest, latest).
- Monitoring consumer lag helps detect slow consumers.

### Consumer offsets storage
- Kafka stores consumer offsets in a compacted topic `__consumer_offsets`.
- Each consumer group commits offsets for partitions it consumes.
- Offsets are committed either automatically (auto.commit) or manually.
- Broker replicates this topic for durability.
- Offsets are used to resume consumption after a consumer restart.

### Consumer lag
- Difference between latest offset in partition and consumer's committed offset.
- Indicates how far behind a consumer is in processing.
- High lag can mean consumer is slow, under-provisioned, or stuck.
- Monitoring lag is crucial for operational health.
- Can be reduced by adding more consumers (if partitions > consumers) or tuning consumer.

### High watermark
- Offset up to which all replicas in ISR have replicated; consumers can only see messages up to HW.
- Ensures consistency – consumers cannot read uncommitted (not yet replicated) data.
- Leader updates HW after followers acknowledge writes.
- Followers also track HW to know which messages are safe to serve (if follower reads enabled).
- Helps prevent data loss if leader fails.

### Segment files
- Each partition log is split into segments (files) for easier management.
- Active segment is the one currently being written; older segments are closed.
- Segment size is configurable (log.segment.bytes).
- Segments are the unit of deletion during retention cleanup.
- Index files accompany each segment for fast offset lookups.

### Index files
- Kafka maintains offset-to-position index files for each segment.
- Index maps offsets to physical file positions, enabling efficient seek.
- Indexes are sparse (every few bytes) to keep memory footprint low.
- Time index files map timestamps to offsets for time-based lookups.
- Indexes are memory-mapped for fast access.

### Message batching
- Producers batch messages to reduce network overhead and increase throughput.
- Batches are compressed together (if compression enabled) for efficiency.
- Broker stores and serves batches as units; consumers receive batches.
- Batch size is configurable (batch.size, linger.ms).
- Larger batches improve throughput but increase latency.

### Zero-copy transfer
- Kafka uses zero-copy (sendfile) to serve data from disk to network without copying to user space.
- Data is read from disk into page cache, then directly transferred to socket.
- Reduces CPU overhead and improves throughput.
- Especially effective for consumers reading large amounts of data.
- Works when data is in page cache (avoiding disk I/O).

## Producers

### Kafka producer architecture
- Producer is a client that publishes messages to Kafka topics.
- Internally, it has a buffer of records waiting to be sent.
- Sender thread batches records and sends them to brokers.
- Handles serialization, partitioning, compression, and retries.
- Configurable to ensure reliability, ordering, and idempotence.

### Producer internals
- `KafkaProducer` instance manages a pool of network connections.
- `RecordAccumulator` queues records per partition for batching.
- `Sender` thread fetches batches and sends produce requests.
- Metadata updates track partition leaders and cluster topology.
- Callbacks notify application of send success or failure.

### Record batching
- Records sent to same partition are batched to reduce requests.
- Batch size controlled by `batch.size` (max bytes per batch).
- `linger.ms` adds artificial delay to accumulate more records.
- Larger batches improve compression and throughput.
- Batches are compressed (if configured) as a unit.

### Producer buffer memory
- `buffer.memory` sets total memory available for batching.
- If buffer fills, producer blocks or throws exception (depending on `max.block.ms`).
- Buffer memory is shared across partitions.
- Insufficient buffer can lead to bottlenecks or blocking.
- Monitor producer metrics (e.g., buffer available bytes) for tuning.

### Acknowledgement modes (acks=0/1/all)
- `acks=0`: Producer doesn't wait for any acknowledgment; high throughput but possible data loss.
- `acks=1`: Leader writes to its local log and acknowledges; follower may not have replicated.
- `acks=all` (or `-1`): Leader waits for all in-sync replicas to acknowledge; strongest durability.
- Trade-off between latency, throughput, and data safety.
- With `acks=all`, `min.insync.replicas` controls minimum ISR size for success.

### Idempotent producer
- Enabled by setting `enable.idempotence=true`.
- Prevents duplicates caused by retries by assigning a producer ID and sequence numbers.
- Broker deduplicates messages with same PID and sequence.
- Ensures exactly-once semantics for producer within a partition.
- Requires `acks=all` and `max.in.flight.requests.per.connection=5` (or <=5).

### Producer retries
- Configurable `retries` (default: max long) and `retry.backoff.ms`.
- Retries occur for transient errors (e.g., leader election, network timeouts).
- Combined with idempotence to avoid duplicates.
- If `delivery.timeout.ms` exceeded, producer gives up.
- Retries can cause ordering issues if not idempotent (duplicates or out-of-order).

### Delivery timeout
- `delivery.timeout.ms` bounds total time from send to acknowledgment or failure.
- Includes time in buffer, retries, and network delays.
- Default 120 seconds.
- If timeout expires, producer aborts send and invokes callback with error.
- Larger timeout allows more retries but may delay error detection.

### Compression types
- Supported: `none`, `gzip`, `snappy`, `lz4`, `zstd`.
- Compression reduces network and storage usage at cost of CPU.
- Applied on batch level, improving ratio.
- Choose based on data type and CPU availability (zstd best compression, gzip/snappy good balance).
- Broker decompresses only if needed (e.g., for re-compressing with different type).

### Partitioners
- Default partitioner: if key present, hash to partition; if null, sticky round-robin.
- Sticky partitioner improves batching by sending same partition for multiple messages.
- Custom partitioner can implement `Partitioner` interface.
- Can route based on message fields or external logic.
- Partitioners must be thread-safe.

### Sticky partitioning
- Introduced to improve batching when key is null.
- Producer sticks to one partition for a period to fill batches.
- Reduces number of small batches and improves throughput.
- Sticky period ends when batch is full or linger.ms expires.
- Default in recent Kafka versions.

### Producer transactions
- Enable exactly-once semantics across multiple partitions.
- Producer initializes a transactional.id and uses `beginTransaction()`, `commitTransaction()`.
- All messages sent within transaction are committed atomically.
- Uses transaction coordinator and transaction log topic.
- Requires idempotence enabled and appropriate `acks`.

### Exactly-once producer semantics
- Combination of idempotent producer and transactions.
- Idempotence prevents duplicates within a partition.
- Transactions ensure atomic writes across partitions and consumers read committed data.
- For end-to-end exactly-once, consumer must also be transactional and process reads.
- Used in Kafka Streams for stateful operations.

### Producer performance tuning
- Adjust batch.size and linger.ms to balance throughput and latency.
- Compression reduces bandwidth but adds CPU overhead.
- Buffer.memory must be sufficient to handle bursts.
- max.in.flight.requests.per.connection affects ordering and parallelism.
- Monitor producer metrics (request latency, error rate, buffer usage).

### Producer interceptors
- Implement `ProducerInterceptor` to intercept records before send and after acknowledgment.
- Useful for custom metrics, logging, or modifying records.
- Interceptors run in producer's thread, so they should be fast.
- Can affect ordering if not careful.
- Configured via `interceptor.classes`.

## Consumers

### Consumer architecture
- Consumer subscribes to topics and polls for records.
- Works in a consumer group for load balancing and fault tolerance.
- Poll loop fetches data, processes, and commits offsets.
- Maintains connection to group coordinator for rebalance handling.
- Supports manual or automatic offset commit.

### Pull vs push model
- Kafka uses pull model: consumer requests data when ready.
- Push model (like some messaging systems) can overwhelm consumers.
- Pull allows consumer to control pace and batch size.
- Enables backpressure handling by adjusting poll frequency.
- Consumer can rewind or fetch from any offset.

### Consumer groups
- Group of consumers that jointly consume a set of subscribed topics.
- Each partition is assigned to exactly one consumer in the group.
- Allows scaling consumption (more consumers = more parallelism).
- If a consumer fails, partitions reassign to remaining members.
- Group coordinator manages membership and assignments.

### Group coordinator
- One broker designated as coordinator for each consumer group.
- Handles heartbeats, rebalances, and offset commits.
- Stores group metadata in `__consumer_offsets` topic.
- Detects consumer failures via session timeout.
- Coordinates partition assignment during rebalance.

### Rebalancing process
- Triggered when consumers join/leave or topic partitions change.
- All consumers in group stop consuming, revoke partitions, and rejoin.
- Group coordinator assigns new partition mapping (using strategy).
- During rebalance, no messages are processed (stop-the-world).
- Newer cooperative rebalancing allows incremental reassignments.

### Static membership
- Allows consumers to retain partition assignment across restarts.
- Uses a unique `group.instance.id` configured on consumer.
- Coordinator treats static member as still present for a timeout.
- Reduces rebalances on temporary restarts (e.g., rolling updates).
- Still triggers rebalance if member does not return within session timeout.

### Cooperative rebalancing
- Improves upon eager rebalancing by allowing incremental partition reassignment.
- Consumers only revoke a subset of partitions at a time.
- Reduces complete consumption pauses.
- Supported by assignors like CooperativeStickyAssignor.
- Requires consumer clients supporting protocol.

### Partition assignment strategies
- RangeAssignor: Assigns per-topic contiguous ranges (may cause imbalance).
- RoundRobinAssignor: Distributes partitions evenly across consumers.
- StickyAssignor: Maintains as many assignments as possible during rebalance.
- CooperativeStickyAssignor: Works with cooperative rebalance.
- Custom assignors can implement `PartitionAssignor`.

### Offset commit strategies
- Auto commit: Offsets committed periodically (auto.commit.interval.ms) based on polled records.
- Manual commit: Application calls `commitSync()` or `commitAsync()`.
- Auto commit can lead to duplicate processing if poll triggers rebalance before commit.
- Manual gives control: commit after processing ensures at-least-once.
- Async commit is non-blocking but may miss errors; sync retries.

### Auto vs manual commit
- Auto commit simplifies code but risks duplicates or message loss.
- Manual commit with `enable.auto.commit=false` allows precise control.
- Common pattern: process records then commit sync with retry.
- Async commit with callback can improve performance.
- Idempotent consumers can tolerate duplicates with manual commit.

### Offset reset policies
- `auto.offset.reset` config: what to do when no committed offset or offset out of range.
- `earliest`: start from beginning of partition.
- `latest`: start from end (new messages only).
- `none`: throw exception if no offset.
- Used when consumer group is new or offsets expired.

### Consumer lag handling
- Monitor lag via Kafka metrics or tools (kafka-consumer-groups).
- High lag may indicate need to increase consumers or tune processing.
- Lag can be reduced by parallelizing processing (more partitions/consumers).
- Use lag to detect consumer failures or slow downstream.
- Alerting on lag thresholds helps proactive remediation.

### Consumer fault tolerance
- Consumer failures are detected via heartbeat/session timeout.
- Partitions reassigned to other members in group.
- Committed offsets allow new consumer to resume from last processed.
- Processing must be idempotent to handle duplicate messages after rebalance.
- Use manual commits after processing to avoid losing work.

### Heartbeats and session timeout
- Consumer sends heartbeats to coordinator to indicate liveness.
- `session.timeout.ms` determines max time without heartbeat before considered dead.
- `heartbeat.interval.ms` controls frequency (should be lower than timeout).
- If coordinator does not receive heartbeats, triggers rebalance.
- Long processing can cause false timeouts; use `max.poll.interval.ms` to bound processing time.

### Consumer poll loop
- `poll()` fetches records, sends heartbeats, and triggers rebalance callbacks.
- Loop should handle batches efficiently (avoid long processing in loop).
- `max.poll.records` limits records per poll to control batch size.
- `max.poll.interval.ms` max time between polls; if exceeded, consumer considered dead.
- Must call `poll()` frequently to keep session alive.

### Backpressure handling
- Consumer can control flow by limiting `max.poll.records` or pausing partitions.
- Pause/resume API allows dynamic backpressure.
- If processing is slow, consumer can pause until caught up.
- Buffer records in application queue for controlled processing.
- Cooperative rebalancing helps avoid pauses during rebalance.

### Dead letter queue patterns
- Messages that cannot be processed (poison pills) are sent to a DLQ topic.
- Consumer can catch processing exceptions and publish original message to DLQ.
- DLQ messages can be inspected and reprocessed later.
- Requires careful handling to avoid infinite loops.
- Kafka Connect supports DLQ for sink connectors.

## Delivery Semantics

### At-most-once delivery
- Messages may be lost but never redelivered.
- Achieved by committing offset before processing.
- If consumer crashes after commit but before processing, message is lost.
- Producer with acks=0 can also cause message loss.
- Suitable for non-critical data where occasional loss acceptable.

### At-least-once delivery
- Messages are never lost but may be duplicated.
- Achieved by processing then committing offset.
- If consumer crashes after processing but before commit, message is reprocessed.
- Producer with acks=1 or all and retries can cause duplicates without idempotence.
- Requires idempotent consumers to handle duplicates.

### Exactly-once semantics
- Each message is processed exactly once, no loss, no duplicates.
- Achieved end-to-end using idempotent producer, transactions, and transactional consumer.
- Kafka Streams provides exactly-once for stream processing.
- Requires careful coordination between producer and consumer.
- Performance cost due to transaction overhead.

### Idempotency
- Operation can be applied multiple times with same result.
- Producer idempotence prevents duplicates from retries.
- Consumer idempotence via deduplication (e.g., using unique IDs).
- Stream processing uses state stores to deduplicate.
- Essential for exactly-once in distributed systems.

### Transactions
- Allow atomic writes across multiple partitions.
- Producer uses `initTransactions()`, `beginTransaction()`, `commitTransaction()`.
- Consumer can set `isolation.level=read_committed` to read only committed messages.
- Transactions use a transaction coordinator and log.
- Ensure that either all messages in a transaction are written or none.

### End-to-end exactly-once guarantees
- Involves producer, broker, consumer, and processing.
- Producer: idempotent + transactions.
- Broker: durable storage, transactional coordination.
- Consumer: read_committed, transactional offset commits.
- Processing: idempotent writes to external systems.
- Achievable but complex; Kafka Streams simplifies.

### Duplicate message handling
- Duplicates arise from producer retries, consumer rebalances, or network issues.
- Use idempotent producer to avoid duplicates in Kafka.
- Consumer can deduplicate by tracking processed message IDs in state store.
- Design idempotent operations (e.g., upserts) in downstream systems.
- Enable `enable.idempotence` and use transactional producers where needed.

### Consumer idempotency patterns
- Store last processed offset per partition in external database.
- Use unique message IDs to check for duplicates before processing.
- Leverage Kafka's offset commit as part of transaction with external system (two-phase commit).
- In Kafka Streams, state stores automatically deduplicate.
- Idempotent writes (e.g., SET in Redis) simplify handling.

## Kafka Architecture

### Broker cluster architecture
- Cluster composed of multiple brokers; each broker identified by unique ID.
- Brokers store partitions; partitions replicated across brokers.
- Clients connect to any broker; metadata requests return leader info.
- Cluster can scale horizontally by adding brokers.
- Controller manages cluster metadata and leadership.

### Controller election
- In ZooKeeper-based Kafka, one broker is elected controller via ZooKeeper ephemeral node.
- If controller fails, other brokers attempt to become controller.
- Controller handles partition leader elections, topic creation, etc.
- In KRaft, a quorum of controller nodes uses Raft to elect a leader.
- Controller failure detection and failover are automatic.

### ZooKeeper architecture (legacy)
- ZooKeeper maintains cluster metadata: broker list, topic configs, controller, etc.
- Brokers register ephemeral nodes; ZooKeeper detects failures.
- Controller uses ZooKeeper to store state and coordinate.
- Scalability limited by ZooKeeper; used up to Kafka 2.x.
- Removed in Kafka 3.x with KRaft.

### KRaft mode (ZooKeeper removal)
- Kafka Raft metadata quorum replaces ZooKeeper.
- Controllers run in a Raft group to maintain metadata log.
- Simplifies deployment and improves scalability.
- Metadata log is replicated across controller nodes.
- KRaft is production-ready from Kafka 3.x.

### Metadata quorum
- In KRaft, a set of nodes (controller quorum) manages metadata.
- Uses Raft consensus for leader election and log replication.
- Clients and brokers fetch metadata from controllers.
- Quorum size determines fault tolerance (3 or 5 recommended).
- No external dependency, reducing operational complexity.

### Raft consensus in Kafka
- Raft ensures consistency of metadata across controller nodes.
- Leader elected among controllers; all metadata writes go through leader.
- Followers replicate metadata log; commits require majority.
- Provides strong consistency and automatic failover.
- Used in KRaft mode for metadata management.

### Cluster metadata storage
- Metadata includes topics, partitions, replicas, configs, etc.
- In ZooKeeper mode, stored in ZooKeeper tree.
- In KRaft, stored in a compacted metadata log.
- Brokers cache metadata and update via notifications.
- Clients receive metadata updates on demand.

### Partition leadership election
- When leader fails, controller selects new leader from ISR.
- If ISR is empty, controller may choose out-of-sync replica if `unclean.leader.election.enable=true`.
- New leader starts accepting writes and updates followers.
- During election, partition is unavailable for writes.
- Election time impacts availability.

### Rack awareness
- Distributes replicas across racks to tolerate rack failure.
- Configure brokers with `broker.rack`.
- Replica assignment ensures no two replicas of same partition in same rack.
- Consumers can read from nearest replica using `client.rack`.
- Improves availability and network efficiency.

### Multi-datacenter replication
- Kafka MirrorMaker replicates topics between clusters.
- Used for disaster recovery, geo-replication, aggregation.
- MirrorMaker 2 (MM2) supports bidirectional replication and topic renaming.
- Can replicate consumer offsets for failover.
- Considerations: replication latency, bandwidth, conflict resolution.

### MirrorMaker architecture
- MirrorMaker consists of source cluster, target cluster, and connectors.
- MM2 uses Kafka Connect framework for replication.
- Monitors source topics and replicates data to target.
- Supports offset sync and automatic failover.
- Can replicate topics with different names (e.g., with cluster prefix).

## Storage Internals

### Log segments
- Partition log split into segments; each segment is a file on disk.
- Active segment is append-only; old segments are read-only.
- Segments facilitate retention cleanup and index management.
- Segment size configurable; smaller segments increase file count.
- When segment is full, new segment created.

### Page cache usage
- Kafka relies heavily on OS page cache for performance.
- Writes go to page cache before flushing to disk (configurable flush).
- Reads served from page cache if data is cached, avoiding disk I/O.
- Page cache management is offloaded to OS, reducing JVM heap pressure.
- Benefits: simple, efficient, uses all available memory.

### Disk I/O optimization
- Sequential writes to disk (appending) are fast.
- Reads are also sequential for large batches, but random seeks for index lookups.
- Multiple disks can be used for different partitions (JBOD or RAID).
- Avoid swapping; ensure disk throughput matches required I/O.
- Use SSDs for high performance, though HDDs can work with sequential access.

### Sequential writes
- Kafka writes all messages sequentially to partition log.
- Sequential I/O is much faster than random I/O on spinning disks.
- Enables high throughput even on HDDs.
- Index files allow quick offset-based seeks with minimal random reads.
- Write-ahead logging pattern ensures durability.

### Message indexing
- Offset index: maps offset to file position for quick seeks.
- Time index: maps timestamp to offset for time-based lookups.
- Indexes are sparse (one entry per few bytes) to save space.
- Indexes are memory-mapped for fast access.
- Index corruption can cause issues; Kafka rebuilds if needed.

### File descriptor usage
- Each log segment and index file consumes a file descriptor.
- Kafka broker opens many files (partitions * segments * 2).
- OS file descriptor limits must be high enough (e.g., 100k+).
- Monitor file descriptor usage; failure to open leads to broker errors.
- Configure `ulimit -n` accordingly.

### Log cleanup policies
- `delete`: Remove old segments based on time/size.
- `compact`: Retain latest value for each key, delete older records.
- Can be set per topic.
- Cleanup runs in background threads.
- Policy affects storage usage and consumer view.

### Compaction internals
- Compaction scans segments, discarding older records with same key.
- Keeps at least the last record per key, plus tombstones.
- Runs in phases: building map of keys, cleaning segments.
- Uses thread pools; can be throttled.
- Produces new, compacted segments; old ones deleted.

### Retention cleanup
- Time-based: compares segment's last modified time with current.
- Size-based: when total log size exceeds threshold, oldest segments removed.
- Cleanup runs periodically; check `log.retention.check.interval.ms`.
- Deleted segments are removed from disk; free space reclaimed.
- Ensure sufficient disk headroom to avoid out-of-space.

### Tiered storage
- Offload older segments to cheaper storage (e.g., S3, HDFS).
- Allows infinite retention with low-cost storage.
- Broker still serves data from tiered storage when requested.
- Implemented via pluggable storage managers (early access).
- Reduces local disk requirements, enables larger retention.

### Storage scaling strategies
- Increase partition count to distribute load across more disks/brokers.
- Use JBOD with multiple disks per broker.
- Add more brokers to cluster.
- Tiered storage for cold data.
- Monitor disk usage and I/O to identify bottlenecks.

## Kafka Streams

### Kafka Streams architecture
- Lightweight Java library for stream processing.
- Runs as part of your application; no separate processing cluster.
- Uses Kafka topics for input and output, plus internal topics for state.
- Provides exactly-once processing semantics.
- Scales by running multiple instances of the application.

### Stream processing vs batch
- Stream processes data in real-time as it arrives; batch processes bounded datasets.
- Kafka Streams handles unbounded data with time windows.
- Low latency (milliseconds) vs batch (minutes/hours).
- Stateful operations (aggregations, joins) maintain local state.
- Fault-tolerant via changelog topics and standby replicas.

### Stateless vs stateful processing
- Stateless: map, filter, flatMap – no persistent state.
- Stateful: aggregations, joins, windowing – require state stores.
- State stores are local (RocksDB) and backed by changelog topics.
- Stateful operations require repartitioning for key co-location.
- State size can grow; monitor disk usage.

### State stores
- Key-value stores for maintaining processing state.
- RocksDB is default; can be in-memory or custom.
- Stores are fault-tolerant via changelog topics.
- Interactive queries allow reading state from other instances.
- Stores can be persistent or non-persistent.

### Changelog topics
- Compacted topics that store state store changes.
- Used for recovery and standby replicas.
- Each stateful operator has a changelog topic.
- Changelog topics are replicated and persisted.
- Enable restoring state after failure.

### Windowing
- Group records by time windows for aggregations.
- Types: tumbling (fixed, non-overlapping), hopping (overlapping), sliding (session).
- Window retention period defines how long windows are kept.
- Windowed aggregations produce results as new data arrives.
- Grace period allows late data within window.

### Joins
- Stream-stream join: join two streams within a window.
- Stream-table join: enrich stream with table (e.g., dimension data).
- Table-table join: combine two tables.
- Requires co-partitioning (same key) and may need repartitioning.
- Inner, left, outer joins supported.

### Aggregations
- Aggregate records over windows or globally.
- Examples: count, sum, reduce, aggregate.
- Stateful operations store intermediate results in state stores.
- Output results as streams or tables.
- Can be windowed or non-windowed.

### Exactly-once stream processing
- Kafka Streams configures processing.guarantee=exactly_once_v2.
- Uses producer transactions and consumer read_committed.
- State updates and output messages are committed atomically.
- Ensures no duplicates even on failure/recovery.
- Slight performance cost vs at-least-once.

### Fault tolerance
- State stores backed by changelog topics.
- If instance fails, tasks migrate to other instances; state restored from changelog.
- Standby replicas can reduce recovery time.
- Consumer offsets committed transactionally with processing.
- Exactly-once semantics prevent duplicates.

### Repartition topics
- Internal topics created automatically when repartitioning needed.
- Occurs when key changes (e.g., after map, selectKey).
- Data is re-partitioned to ensure keys co-located for joins/aggregations.
- Repartition topics are partitioned by the new key.
- Can impact performance; minimize unnecessary repartitioning.

### Interactive queries
- Query state stores directly from your application.
- Allows serving data from stream processing results (e.g., REST API).
- Need to discover which instance hosts which key.
- Kafka Streams provides metadata API for locating stores.
- Enables building real-time queryable applications.

### Streams scalability
- Scale by adding more application instances (same application.id).
- Partitions are redistributed across instances.
- Each instance processes a subset of partitions.
- State stores scale with partitions; more partitions mean more parallelism.
- Monitor task assignment and rebalance times.

## Kafka Connect

### Kafka Connect architecture
- Framework for scalable and reliable data integration.
- Runs as workers (standalone or distributed cluster).
- Workers execute connectors and tasks.
- Uses internal topics for configs, offsets, and status.
- REST API for management.

### Distributed vs standalone mode
- Standalone: single process, good for development.
- Distributed: cluster of workers for scalability and fault tolerance.
- Workers coordinate via internal topics.
- Connectors and tasks are distributed across workers.
- Distributed mode recommended for production.

### Source connectors
- Import data from external systems into Kafka.
- Poll external system and produce records to Kafka topics.
- Handle offset management to track progress.
- Support various formats (Avro, JSON, etc.) via converters.
- Examples: Debezium for CDC, JDBC source.

### Sink connectors
- Export data from Kafka to external systems.
- Consume records from topics and write to target.
- Handle idempotent writes, DLQ, retries.
- Can use converters and transformations.
- Examples: Elasticsearch sink, S3 sink.

### Connector tasks
- Connector work is split into tasks for parallelism.
- Tasks run on workers and perform actual data movement.
- Number of tasks configurable per connector.
- Tasks are rebalanced when workers join/leave.
- Task failures are retried by Connect framework.

### Offset management in Connect
- Source connectors store offsets in a dedicated Kafka topic (`connect-offsets`).
- Offsets track position in source system (e.g., file offset, DB sequence).
- Sink connectors use consumer offsets stored in Kafka.
- Enables exactly-once semantics (with idempotent producers).
- Offsets committed after successful processing.

### Fault tolerance
- Distributed Connect uses worker groups; if worker fails, tasks reassigned.
- Connector configs and status stored in internal topics.
- Offsets allow resuming from last committed point.
- Connect uses Kafka’s consumer groups for task rebalance.
- Ensure internal topics have sufficient replication.

### Connector scaling
- Increase tasks by updating connector config (tasks.max).
- Add more workers to distribute tasks.
- Tasks are rebalanced automatically.
- Scaling depends on partitions and parallelism of source/sink.
- Monitor task assignment and worker load.

### Schema evolution
- Connect integrates with Schema Registry for Avro, Protobuf, JSON.
- Converters handle serialization/deserialization.
- Support for schema evolution with compatibility rules.
- Sink connectors can update target schema if supported.
- Important for long-running pipelines.

### Dead letter queues in Connect
- Sink connectors can route failed records to a DLQ topic.
- Configure `errors.tolerance` and `errors.deadletterqueue.topic.name`.
- Errors like serialization or transformation failures.
- Allows main pipeline to continue despite bad records.
- DLQ records can be reprocessed or analyzed.

### Connect REST API
- REST endpoints for managing connectors: GET, POST, PUT, DELETE.
- Get connector status, list connectors, pause/resume.
- View task status and configurations.
- REST API used by automation tools.
- Secured via authentication/authorization plugins.

## Security

### Kafka security model
- Multi-layered: authentication, authorization, encryption.
- Clients authenticate using SASL or SSL.
- Authorization via ACLs or RBAC (with plugins).
- Encryption in transit with SSL/TLS.
- Encryption at rest via disk encryption (external to Kafka).

### Authentication methods
- SSL/TLS client authentication (mutual TLS).
- SASL/PLAIN (username/password, not secure without TLS).
- SASL/SCRAM (challenge-response, stores credentials in ZooKeeper/KRaft).
- SASL/GSSAPI (Kerberos).
- SASL/OAUTHBEARER (OAuth2 tokens).

### SSL/TLS encryption
- Encrypts communication between clients and brokers, and inter-broker.
- Requires keystores and truststores.
- Can be one-way (server auth) or two-way (mutual auth).
- Performance overhead but necessary for sensitive data.
- Configure `ssl.*` properties on brokers and clients.

### SASL mechanisms
- Simple Authentication and Security Layer framework.
- Each mechanism has specific configuration.
- Brokers can support multiple mechanisms simultaneously.
- Choose based on security requirements and existing infrastructure.
- SCRAM is good for dynamic credential management.

### SCRAM authentication
- Salted Challenge Response Authentication Mechanism.
- Credentials stored in ZooKeeper (or KRaft) as salted hashes.
- Supports credential creation and rotation via kafka-configs.sh.
- Works well without external Kerberos infrastructure.
- Must be used over TLS to protect password exchange.

### Kerberos authentication
- Enterprise-grade authentication using tickets.
- Kafka brokers need to be Kerberized (keytab files).
- Clients authenticate via kinit or keytab.
- Centralized identity management.
- Requires Kerberos KDC infrastructure.

### ACL authorization
- Access Control Lists define who can do what on which resources.
- Resources: topics, consumer groups, etc.
- Operations: read, write, describe, etc.
- Principal identifies user (e.g., `User:alice`).
- Stored in ZooKeeper (or KRaft metadata).

### RBAC models
- Role-Based Access Control (e.g., in Confluent Platform).
- Roles like Developer, Operator assigned to users/groups.
- Simplifies permission management compared to ACLs.
- Implemented via security plugins (custom authorizers).
- May integrate with LDAP or OIDC.

### Encryption in transit
- SSL/TLS for all network communication.
- Prevents eavesdropping and tampering.
- Can be enabled for client-broker and inter-broker.
- Performance impact; hardware acceleration helps.
- Required for compliance (PCI, HIPAA).

### Encryption at rest
- Kafka does not natively encrypt data on disk.
- Use disk-level encryption (LUKS, dm-crypt) or filesystem encryption.
- Or use tiered storage with encrypted object stores.
- Encrypting at rest protects against physical theft.
- Performance overhead varies.

### Multi-tenant Kafka security
- Isolate tenants using separate clusters or namespaces.
- Use ACLs to restrict access per topic/group.
- Quotas to limit resource usage per client.
- Security plugins for tenant isolation.
- Ensure brokers are properly configured to avoid cross-tenant leaks.

## Performance & Tuning

### Throughput optimization
- Increase batch size (`batch.size`, `linger.ms`) for producers.
- Use compression (`gzip`, `zstd`) to reduce network and disk I/O.
- More partitions allow higher parallelism.
- Tune `num.io.threads` and `num.network.threads` on brokers.
- Ensure sufficient page cache and disk I/O bandwidth.

### Latency tuning
- Reduce `linger.ms` and `batch.size` for lower latency.
- Set `acks=0` or `1` for faster writes (trade durability).
- Optimize network latency (use data centers with low RTT).
- Tune JVM GC to reduce pauses.
- Use SSDs for faster disk access.

### Broker configuration tuning
- `num.replica.fetchers` controls replication parallelism.
- `log.flush.interval.ms` and `log.flush.interval.messages` control flush to disk.
- `compression.type` can be set at broker to compress all data.
- `message.max.bytes` for larger messages.
- `replica.lag.time.max.ms` to detect slow replicas.

### Producer performance tuning
- `max.in.flight.requests.per.connection` – higher value increases throughput but risks ordering if not idempotent.
- `compression.type` – choose based on CPU/network trade-off.
- `buffer.memory` – enough to handle bursts without blocking.
- `retries` and `delivery.timeout.ms` – balance reliability and latency.
- `max.request.size` – large enough for batch.

### Consumer performance tuning
- `fetch.min.bytes` and `fetch.max.wait.ms` to control pull efficiency.
- `max.partition.fetch.bytes` – size per partition per fetch.
- `max.poll.records` – limit records per poll to avoid long processing.
- Use `pause()` and `resume()` for backpressure.
- Increase `heartbeat.interval.ms` and `session.timeout.ms` for long processing.

### Partition count strategy
- More partitions increase parallelism but also overhead (more files, leader elections).
- General rule: partitions per broker <= 4000 (depending on resources).
- Consider throughput per partition (e.g., 10 MB/s per partition).
- Use key-based partitioning only if needed; too many partitions can harm performance.
- Monitor and reassign partitions as cluster grows.

### Replication tradeoffs
- Higher replication factor improves durability and read locality (if follower reads enabled).
- Increases network and storage overhead, write latency (acks=all).
- For critical data, use 3 replicas.
- For less critical, 2 or 1 may suffice.
- Balance with `min.insync.replicas` for availability.

### Batch size tuning
- Larger batches improve compression and throughput.
- `batch.size` per partition; if batch fills, it's sent.
- `linger.ms` adds delay; too high increases latency.
- Monitor batch metrics to see if batches fill quickly.
- Compression efficiency improves with batch size.

### Compression tradeoffs
- Compression reduces storage and network usage.
- Adds CPU overhead on producer and broker (if broker decompresses).
- Choose compressor based on CPU vs ratio: zstd best ratio, lz4/snappy faster.
- Broker can recompress if different compression set.
- Enable compression if network or disk is bottleneck.

### JVM tuning
- Kafka runs on JVM; tune heap size (6-8 GB typical).
- Use G1GC for low pause times.
- Monitor GC logs for long pauses.
- Off-heap memory used for page cache, so don't over-allocate heap.
- Set `KAFKA_HEAP_OPTS` appropriately.

### GC tuning
- G1GC recommended for Kafka.
- Target young generation size to avoid frequent collections.
- Use `-XX:+UseG1GC`, `-XX:MaxGCPauseMillis=20`.
- Monitor GC pause times via JMX.
- If GC pauses cause replica lag, adjust heap or GC.

### Network tuning
- Increase TCP buffer sizes (`net.core.rmem_max`, `net.core.wmem_max`).
- Tune `socket.send.buffer.bytes` and `socket.receive.buffer.bytes`.
- Use multiple network threads (`num.network.threads`).
- Consider using separate networks for replication and clients.
- Monitor network interface saturation.

### Disk optimization
- Use multiple disks (JBOD) for different log directories.
- Mount with `noatime` to reduce writes.
- Use XFS or ext4 file systems.
- Ensure disk is not shared with other high-I/O processes.
- Monitor disk latency and I/O wait.

### Scaling strategies
- Add more brokers and reassign partitions.
- Increase partition count for topics.
- Use rack awareness for better failure tolerance.
- Consider tiered storage for cold data.
- Monitor cluster metrics to identify bottlenecks.

## Operations & Monitoring

### Kafka cluster setup
- Choose hardware: CPU cores, RAM (at least 16-32GB for page cache), fast disks.
- Determine number of brokers based on throughput and replication.
- Configure broker properties (broker.id, log.dirs, zookeeper.connect).
- Set up ZooKeeper or KRaft quorum.
- Test with sample producers/consumers.

### Capacity planning
- Estimate peak throughput (MB/s) and message size.
- Compute required disk space based on retention and replication.
- Plan for growth; leave headroom.
- Consider network bandwidth between brokers and clients.
- Monitor and adjust as usage evolves.

### Scaling brokers
- Add new brokers to cluster with unique broker.id.
- Use partition reassignment tool to move partitions to new brokers.
- Update client configurations if necessary.
- Monitor reassignment progress.
- Rebalance partition leadership if needed.

### Topic configuration management
- Topics can be created with specific settings (partitions, replication, retention).
- Use `kafka-topics.sh` or Admin API.
- Alter configs dynamically (e.g., retention.ms).
- Ensure consistent naming conventions.
- Document topic purpose and owners.

### Broker maintenance
- Rolling restarts for config changes or upgrades.
- Gracefully shutdown broker (controlled shutdown).
- Ensure ISR remains healthy during maintenance.
- Use `kafka-graceful-stop.sh` or `kill -SIGTERM`.
- Monitor after restart.

### Rolling upgrades
- Upgrade Kafka version one broker at a time.
- Follow upgrade guide for compatibility (e.g., inter.broker.protocol.version).
- Update clients gradually if protocol changes.
- Test in staging first.
- Have rollback plan.

### Partition reassignment
- Move partitions between brokers for load balancing or broker replacement.
- Use `kafka-reassign-partitions.sh` with generated plan.
- Monitor reassignment status; can be throttled.
- Can also change replication factor.
- May cause temporary performance impact.

### Broker failure recovery
- Broker detected as failed by controller via ZooKeeper/KRaft.
- Partitions with this broker as leader elect new leaders from ISR.
- Under-replicated partitions recover when broker comes back.
- Ensure broker restarts and rejoins ISR.
- Monitor recovery time.

### Disaster recovery
- Replicate data to another cluster (MirrorMaker).
- Plan for regional failure: active-passive or active-active.
- Test failover procedures.
- Consider backup of configuration and metadata.
- Define RPO and RTO.

### Monitoring metrics
- Broker metrics: request rate, error rate, under-replicated partitions, ISR size.
- Producer metrics: request latency, error rate, buffer availability.
- Consumer metrics: lag, commit latency.
- System metrics: CPU, disk I/O, network, GC.
- Use JMX, Prometheus, Grafana.

### Consumer lag monitoring
- Use `kafka-consumer-groups.sh` to describe group and view lag.
- Automated tools (Burrow, Kafka Lag Exporter).
- Lag alerts indicate consumer issues.
- Monitor per-partition lag to detect skew.
- Investigate high lag for bottlenecks.

### ISR monitoring
- Check under-replicated partitions metric.
- If partitions stay under-replicated, investigate follower failures.
- Ensure `min.insync.replicas` not violated.
- Monitor replica lag via `kafka-replica-verification.sh`.
- ISR health is critical for durability.

### Alerting strategies
- Set alerts for broker down, under-replicated partitions, high consumer lag.
- Disk space usage (e.g., >80%).
- Network errors or timeouts.
- Controller failures.
- Use thresholds and trends to avoid false alarms.

### Kafka CLI tools
- `kafka-topics.sh`: create, list, describe, alter topics.
- `kafka-console-producer.sh` / `kafka-console-consumer.sh`: test.
- `kafka-consumer-groups.sh`: manage offsets, lag.
- `kafka-reassign-partitions.sh`: reassign partitions.
- `kafka-dump-log.sh`: examine log segments.

### Admin API usage
- Java AdminClient for programmatic administration.
- Create topics, alter configs, describe cluster.
- List consumer groups, delete records.
- Useful for automation and integration.
- Supports asynchronous operations.

## Reliability & Fault Tolerance

### Broker failure handling
- Controller detects broker failure via session timeout.
- Triggers leader election for affected partitions.
- Consumers reconnect to new leaders.
- Replicas on failed broker become unavailable until broker returns.
- Cluster continues serving with remaining brokers.

### Leader election recovery
- Controller picks new leader from ISR.
- New leader begins accepting writes; followers truncate and catch up.
- If no ISR, election may use unclean leader election (if enabled).
- Downtime for partition during election (milliseconds to seconds).
- After election, producers/consumers resume.

### Replica synchronization
- Followers continuously fetch from leader and write to log.
- They acknowledge when messages are written (not flushed).
- Leader tracks followers' LEO (log end offset).
- Replicas that fall behind are removed from ISR.
- Synchronization ensures data redundancy.

### Under-replicated partitions
- Partition with fewer replicas than replication factor.
- Occurs when a broker is down or replica is out of sync.
- Indicates reduced fault tolerance.
- Monitor and resolve by fixing failed brokers.
- Can be caused by slow replication or network issues.

### Data durability guarantees
- With acks=all and min.insync.replicas >=2, writes are durable if at least 2 replicas confirm.
- Data written to Kafka is persisted to disk and replicated.
- If all replicas of a partition fail, data may be lost.
- Use replication factor 3 and min.insync.replicas=2 for strong durability.
- Flush settings (flush.messages) affect how quickly data hits disk.

### Failover strategies
- Consumer groups automatically rebalance on failure.
- Producers can retry on connection errors.
- MirrorMaker for cross-cluster failover.
- Client-side retries and idempotence.
- Plan for graceful degradation.

### Network partition handling
- Kafka is sensitive to network partitions; may cause split-brain in old versions.
- With Raft (KRaft), consensus reduces split-brain risk.
- If network partition isolates some brokers, they may be removed from ISR.
- After partition heals, they catch up.
- Clients may experience temporary leader not available.

### Split brain scenarios
- In ZooKeeper-based Kafka, controller failure could cause multiple controllers? (ZooKeeper prevents via ephemeral nodes).
- In KRaft, Raft ensures single leader.
- With improper configuration, two leaders for same partition could appear (rare).
- Use fencing mechanisms (epoch numbers) to prevent.
- Kafka's design minimizes split-brain.

### Message loss scenarios
- Producer with acks=0 may lose messages on broker failure.
- Unclean leader election may lose messages not yet replicated.
- Consumer auto-commit before processing leads to loss.
- Disk corruption or deletion of segments.
- Use acks=all, idempotence, and replication to mitigate.

### Recovery strategies
- For lost messages, restore from backups or source system.
- Use MirrorMaker for DR copy.
- Replay from beginning if retention allows.
- Consumer reprocessing with reset to earliest.
- Ensure monitoring catches gaps.

## Schema & Data Management

### Schema Registry
- Centralized service for storing and managing schemas.
- Integrates with Kafka producers/consumers using Avro, Protobuf, JSON Schema.
- Ensures schema compatibility during evolution.
- Stores schemas with a subject (topic-key or topic-value).
- Reduces payload size by sending only schema ID in messages.

### Avro serialization
- Compact binary format with schema evolution support.
- Uses Schema Registry to store schemas.
- Producer serializes data with schema, consumer deserializes using same schema.
- Supports many languages.
- Popular for Kafka due to strong compatibility checks.

### Protobuf serialization
- Protocol Buffers, developed by Google.
- Efficient binary serialization.
- Schema defined in .proto files.
- Schema Registry supports Protobuf.
- Good for cross-language, versioning.

### JSON schema
- Human-readable, but larger payload.
- JSON Schema for validation.
- Schema Registry supports JSON Schema.
- Can be used with JSON converters.
- Suitable for simpler systems or when human readability needed.

### Schema evolution
- Change schema over time as requirements evolve.
- Schema Registry enforces compatibility rules.
- Producer can send new version; consumer must be able to read.
- Evolution may require data transformation.
- Plan for backward/forward compatibility.

### Compatibility modes
- BACKWARD: new schema can read old data (default).
- FORWARD: old schema can read new data.
- FULL: both backward and forward.
- NONE: no checks.
- Can be set per subject.

### Backward/forward compatibility
- Backward: consumers using new schema can read data written with old schema.
- Forward: consumers using old schema can read data written with new schema.
- Achieved by adding optional fields, not removing required.
- Critical for rolling upgrades of applications.
- Test compatibility before deploying.

### Data contracts
- Agreement between producer and consumer about data format and semantics.
- Documented via schema and compatibility rules.
- Enforced by Schema Registry.
- Helps avoid breaking changes.
- Part of API governance.

### Event versioning
- Events may have multiple versions over time.
- Include version field in payload or use different topics.
- Use schema evolution to handle changes.
- Consider using Avro/Protobuf with schema registry.
- Plan for handling old events in consumers.

## Advanced Topics

### Exactly-once end-to-end pipelines
- Use idempotent producers and transactions.
- Consumer reads with read_committed and processes idempotently.
- External writes must be transactional (e.g., two-phase commit) or idempotent.
- Kafka Streams provides exactly-once for stream processing.
- Achievable but adds complexity; often at-least-once with deduplication suffices.

### Event sourcing with Kafka
- Store all state changes as events in Kafka.
- Current state is derived by replaying events.
- Kafka's immutable log serves as event store.
- Enables auditing, replay, and temporal queries.
- Combine with CQRS for read models.

### CQRS with Kafka
- Command Query Responsibility Segregation.
- Commands produce events to Kafka; queries read from materialized views.
- Use Kafka Streams to build read models (tables).
- Scales read and write independently.
- Events decouple command and query sides.

### Kafka in microservices architecture
- Services communicate via asynchronous events.
- Decouples services; improves resilience.
- Use topics for domain events.
- Schema registry ensures contract.
- Challenges: eventual consistency, debugging.

### Event-driven architecture patterns
- Event notification: services notify others of changes.
- Event-carried state transfer: events contain data to reduce queries.
- Event sourcing: state as event sequence.
- Saga pattern for distributed transactions.
- CQRS for read/write separation.

### Stream-table duality
- Stream: sequence of events (fact).
- Table: snapshot of latest state (current view).
- In Kafka, a stream can be converted to a table via aggregation (KTable).
- Table can be converted to stream by capturing changes (changelog).
- Core concept in Kafka Streams.

### Outbox pattern
- To reliably publish events when updating a database.
- Service writes to database and inserts event into outbox table in same transaction.
- A separate process (e.g., Debezium) reads outbox and publishes to Kafka.
- Ensures atomicity of DB update and event publication.
- Avoids dual-write problems.

### Saga pattern
- Manage distributed transactions across microservices.
- Each service performs local transaction and publishes event.
- If a step fails, compensating actions are triggered.
- Kafka can orchestrate saga via events.
- No distributed locking; eventual consistency.

### Kafka as system backbone
- Central nervous system for data flow.
- All important events pass through Kafka.
- Enables decoupling, scalability, real-time processing.
- Requires careful design of topics, schemas, retention.
- Must be highly available and monitored.

### Multi-region Kafka
- Deploy clusters in multiple geographic regions.
- Use MirrorMaker for replication.
- Active-active or active-passive setups.
- Consider latency, conflict resolution.
- Client can write to nearest cluster.

### Geo-replication
- Replicate data across data centers for disaster recovery.
- MirrorMaker 2 supports bidirectional.
- Can be used for global data aggregation.
- May need to handle topic name mapping.
- Monitor replication lag.

### Hybrid cloud Kafka
- Run Kafka across on-prem and cloud.
- Use VPN or direct connect for networking.
- Replicate between clusters (e.g., on-prem to cloud).
- Manage security and compliance.
- Challenges: network latency, split clusters.

### Kubernetes deployment
- Kafka can run on K8s using operators (Strimzi, Confluent Operator).
- Operators handle deployment, scaling, upgrades.
- StatefulSets for brokers; persistent volumes for storage.
- Use headless services for discovery.
- Leverage K8s features for rolling updates, monitoring.

### Kafka on cloud providers
- Confluent Cloud, MSK (AWS), GCP Pub/Sub (not native Kafka).
- Managed services reduce operational burden.
- Auto-scaling and pay-per-use.
- But may have limitations (feature set, cross-region).
- Integration with cloud services.

### Tiered storage architecture
- Offload older log segments to cheaper cloud storage.
- Broker fetches from remote when needed.
- Reduces local disk requirements; allows longer retention.
- Early adopters; may impact read latency for cold data.
- Implemented via pluggable storage managers.

## Real-World Design Scenarios

### Designing a high-throughput pipeline
- Estimate peak throughput (e.g., 1M messages/sec, 1KB each = 1GB/s).
- Choose partition count: e.g., 100 partitions to distribute load.
- Tune producers: batch.size, linger.ms, compression.
- Use multiple brokers with sufficient network and disk.
- Monitor for bottlenecks and scale.

### Handling billions of events per day
- Compute events per second: billions/day ~ tens of thousands per second, manageable.
- Ensure adequate partition count for parallelism.
- Plan retention and storage capacity.
- Use compression to reduce storage.
- Design idempotent consumers to handle duplicates.

### Kafka for log aggregation
- Collect logs from multiple services into central Kafka.
- Use log shippers (Filebeat, Fluentd) as producers.
- Organize topics by service or log type.
- Consumers: Elasticsearch, monitoring tools.
- Consider retention for logs.

### Kafka for analytics pipelines
- Ingest user activity events.
- Use Kafka Streams to compute real-time metrics (e.g., page views per minute).
- Sink to data warehouse (e.g., Snowflake) via Kafka Connect.
- Ensure data quality and schema evolution.
- Handle late-arriving data with windows.

### Kafka in fintech systems
- Critical requirements: exactly-once, high durability.
- Use acks=all, replication factor 3, min.insync.replicas=2.
- Enable idempotent producers and transactions.
- Ensure security (SSL, ACLs).
- Plan for disaster recovery and compliance.

### Kafka for real-time fraud detection
- Stream transactions through Kafka.
- Use Kafka Streams with stateful operations (aggregations, joins) to detect patterns.
- Low latency required (seconds).
- Scale with partitions; use interactive queries to serve model data.
- Handle backpressure and alerting.

### Kafka for IoT ingestion
- Billions of devices send data.
- Use many partitions (by device ID or region).
- Producers may need lightweight protocols (MQTT gateway).
- Consider small message sizes, high throughput.
- Monitor consumer lag for processing.

### Kafka for microservice communication
- Services communicate via events.
- Define domain topics (e.g., OrderCreated, PaymentProcessed).
- Use schema registry for contracts.
- Ensure idempotent consumers.
- Handle eventual consistency and sagas.

### Scaling consumer workloads
- Increase partitions and consumers (max parallelism = partitions).
- Ensure consumers are stateless or use external state.
- Monitor lag; if persistent, increase partitions or optimize processing.
- Use cooperative rebalancing to reduce downtime.
- Consider using Kafka Streams for automatic scaling.

### Backpressure strategies
- Consumer can pause partitions when processing queue full.
- Use bounded queues in application.
- Adjust fetch sizes and poll intervals.
- In stream processing, use `max.poll.records` and commit after processing.
- Monitor lag and scale consumers.

### Handling hot partitions
- Some keys receive disproportionate traffic.
- Use salting: add random suffix to keys to spread across partitions, but then ordering per key lost.
- Over-partition and let partitioner distribute.
- For ordered processing, consider splitting hot key into subkeys.
- Use custom partitioning based on some identifier.
- Monitor partition traffic and reassign if needed.

### Designing DLQ architecture
- For consumers, catch processing exceptions and publish to DLQ topic.
- Include original message, error details, headers.
- Have separate consumers for DLQ to analyze and reprocess.
- Set retention appropriately for DLQ.
- Alert on DLQ messages.
