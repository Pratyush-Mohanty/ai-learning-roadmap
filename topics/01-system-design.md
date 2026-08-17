# System Design — Concepts That Matter

Teach-yourself content for a data engineer. The goal is **tradeoff reasoning**, not memorizing diagrams.

## 1. The Core Skills
System design is decision-making under constraints. Every question reduces to:
- **Scale:** how many requests/data? What's the growth curve?
- **Consistency vs availability:** which do you sacrifice when?
- **Cost vs latency vs complexity:** which lever matters for THIS business?

### Requirements gathering (first 5 minutes, always)
1. Functional requirements: what must the system DO?
2. Non-functional: latency (p50/p99), availability (SLA), durability, scale, cost budget
3. Constraint discovery: read-heavy vs write-heavy? Real-time vs batch? Multi-region?

## 2. Scalability Ladder (learn in order)
1. **Vertical scaling** — bigger machine. Simple, hits a ceiling, single point of failure.
2. **Horizontal scaling** — more machines behind a **load balancer**. Statelessness required.
3. **Stateless services** — move session/user state OUT of the app tier into a shared store (Redis, DB). This is what makes horizontal scaling work.
4. **Database scaling:**
   - Read replicas (reads scale, writes don't)
   - Sharding/partitioning (split by key: user_id hash, geography, tenant)
   - Write-heavy → append-only stores, queues to decouple

**Key insight for a data engineer:** sharding is your friend — you already partition tables. System design just applies it at service scale.

## 3. CAP Theorem (and the myth)
- **C**onsistency (all replicas see same data), **A**vailability (every request gets a response), **P**artition tolerance (system works when network splits).
- Under a network partition you choose **CP or AP**. You don't get to "have all three."
- In practice: most distributed systems are **CP** (databases: Postgres, Spanner) or **AP** (eventual consistency: DynamoDB, Cassandra, Kafka-based).
- **Real lesson:** choose your consistency model explicitly (strong, causal, eventual) and document the business tradeoff.

## 4. Storage Engine Fundamentals (what's under the hood)
- **B-tree** (Postgres/MySQL default): balanced tree, good point + range reads, in-place updates.
- **LSM-tree** (Cassandra, RocksDB, Bigtable): append-only + memtable + compaction. Fast writes, slower reads, great write throughput. **This is why Bigtable/Dataproc workloads are write-optimized.**
- **Log-structured append**: Kafka, WAL. Everything is a sequential log.
- **Storage vs memory**: index in memory, data on disk; caching layers (Redis) for hot data.

## 5. Caching (the highest-ROI trick in interviews)
- **Types:** client cache, CDN (static), reverse proxy, in-app cache, distributed cache (Redis/Memcached).
- **Patterns:**
  - **Cache-aside (lazy):** read cache → miss → read DB → write cache. Simple, stale risk.
  - **Write-through:** write DB + cache together. Consistent, slower writes.
  - **Write-back:** write cache, flush to DB async. Fast, data-loss risk on crash.
- **Invalidation is the hard part.** TTLs, versioned keys, event-driven invalidation via CDC (you know this from data).
- **Hot-key problem:** one key hammered → replicate/read-through fan-out.

## 6. Load Balancing & Routing
- **Layers:** L4 (TCP) vs L7 (HTTP/application). L7 can route by path/header → enables canary + A/B.
- **Algorithms:** round-robin, least-connections, consistent hashing (stable under node churn — used by sharded caches/DBs).
- **Health checks + failover:** active (probe) vs passive (error counting).

## 7. Asynchronous Processing & Queues
- **Why:** decouple producers from consumers; smooth load spikes; enable retries.
- **Message queues:** Kafka (log, replay, ordered by partition), RabbitMQ/SQS (task queues), Pub/Sub (event delivery).
- **At-least-once vs exactly-once:** most systems do at-least-once + **idempotent consumers** (dedupe by key). Exactly-once = transactional outbox + dedup.

## 8. Data Consistency Patterns
- **Two-phase commit (2PC):** correct but slow, blocking.
- **Saga pattern:** sequence of local transactions + compensating actions on failure. The modern default for distributed transactions.
- **Transactional outbox:** write intent + event atomically in same DB; a relay publishes to the queue. Avoids dual-write problem.
- **Event sourcing:** state = replay of events. Powerful for auditability.

## 9. Availability & Failure Handling
- **Numbers:** 99.9% ≈ 8.7h/yr downtime, 99.99% ≈ 52min/yr. SLAs cost money — don't over-promise.
- **Techniques:** redundancy, failover (active-passive / active-active), retries with exponential backoff + jitter, circuit breakers, bulkheads (isolate failure domains), graceful degradation (degrade features, not whole system).
- **Idempotency:** retries MUST be safe. Idempotency keys on write endpoints.

## 10. Worked Decision Framework
For each requirement, ask: *where does this data live, how does it get there, and what breaks if a node dies?*

**Classic practice set (10-12):**
1. URL shortener — hashing, read-heavy, no consistency problem → design read path + cache
2. Rate limiter — in-memory counters + distributed (Redis) token bucket
3. Chat system — websocket fan-out, presence, message ordering
4. News feed — fan-out-on-write vs fan-out-on-read (the classic tradeoff)
5. Key-value store — LSM vs B-tree, replication, consistency
6. Web crawler — queue + dedup + politeness
7. Notification system — multi-channel, retries, backpressure
8. Ride-sharing / matching — geospatial index, real-time updates
9. Video streaming — CDN, DASH segments, buffering
10. Payment system — exactly-once semantics, idempotency, sagas
11. Distributed ID generator — snowflake-style (timestamp + worker + seq)
12. Search — inverted index, ingestion pipeline

## Go Deeper (pick 1-2, not all)
- **Book:** Designing Data-Intensive Applications — https://www.oreilly.com/library/view/designing-data-intensive-applications/9781098119058/ (read this; your background makes it fast)
- **Book:** System Design Interview Vol 1 — https://www.amazon.com/System-Design-Interview-Second-Insiders/dp/B08CMF2CQF
- **Repo:** System Design Primer — https://github.com/donnemartin/system-design-primer (reference, not cover-to-cover)
- **Repo:** karanpratapsingh/system-design (course-style, linear) — https://github.com/karanpratapsingh/system-design
- **Video:** ByteByteGo YouTube — https://www.youtube.com/@ByteByteGo
- **Practice:** ashishps1/awesome-system-design-resources — https://github.com/ashishps1/awesome-system-design-resources
- **Diagrams:** Excalidraw — https://excalidraw.com