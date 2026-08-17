# System Design - Comprehensive Study Guide

**Estimated time: 3 weeks.** Goal: reason about tradeoffs, not memorize diagrams. This file is your study material — work through it section by section, then do the exercises.

---

## 1. What System Design Actually Tests

System design is **decision-making under constraints**. Every question reduces to:

- **Scale:** how many requests/data? What is the growth curve?
- **Consistency vs availability:** which do you sacrifice when they conflict?
- **Cost vs latency vs complexity:** which lever matters for THIS business?

### The 5-Minute Requirements Gathering (do this first, always)

1. **Functional requirements** — what must the system DO? (1-3 sentences)
2. **Non-functional** — latency (p50/p99), availability (SLA %), durability, scale, cost budget
3. **Constraint discovery** — read-heavy vs write-heavy? Real-time vs batch? Multi-region? Compliance?

> **Rule:** Never design before you understand the numbers. Ask for the expected QPS, data size, and growth rate first.

---

## 2. The Scalability Ladder (learn in this exact order)

### Level 1: Vertical scaling
Bigger machine. Simple, hits a ceiling, single point of failure.

### Level 2: Horizontal scaling + load balancer
More machines behind a load balancer. **Requires statelessness.**

### Level 3: Stateless services
Move session/user state OUT of the app tier into a shared store (Redis, DB). This is what makes horizontal scaling work. If a node dies and nothing is lost, you are stateless.

### Level 4: Database scaling
```
         ┌─────────────────────────────────────┐
         │        Database Scaling Path        │
         ├─────────────────────────────────────┤
         │ 1. Read replicas (reads scale)      │
         │ 2. Sharding / partitioning          │
         │    - by user_id hash                │
         │    - by geography                   │
         │    - by tenant                      │
         │ 3. Write-heavy: append-only + queue │
         └─────────────────────────────────────┘
```

**Key insight for a data engineer:** you already partition tables. System design applies it at service scale. The shard key choice is THE critical decision — it determines hot spots and future rebalancing.

---

## 3. CAP Theorem (and the myth around it)

- **C**onsistency — all replicas see the same data
- **A**vailability — every request gets a response
- **P**artition tolerance — system works when the network splits

Under a network partition you choose **CP or AP**. You never get all three. Partition tolerance is not optional in distributed systems — you WILL get partitions.

| Choice | Behavior | Examples |
|---|---|---|
| CP | Refuse requests you can't keep consistent | Postgres, Spanner, Zookeeper |
| AP | Answer with potentially stale data | DynamoDB, Cassandra, Redis cluster |

**The real lesson:** choose your consistency model explicitly — strong, causal, or eventual — and document the business tradeoff. Don't let it be accidental.

---

## 4. Storage Engine Fundamentals (what's under the hood)

| Engine | How it works | Best at | Used by |
|---|---|---|---|
| **B-tree** | Balanced tree, in-place update | Point + range reads | Postgres, MySQL |
| **LSM-tree** | Append-only + memtable + compaction | Fast writes, high throughput | Bigtable, Cassandra, RocksDB |
| **Log-structured** | Sequential append + replay | Write throughput, replay | Kafka, WAL, event sourcing |

### Why this matters for your decisions
- LSM = write-optimized. If your workload is write-heavy (telemetry, events), LSM engines win.
- B-tree = read-optimized. If reads dominate with strong consistency, B-tree wins.
- Compaction costs CPU — an LSM store with bad config has unpredictable read latency spikes.

---

## 5. Caching (the highest-ROI trick)

### Cache types (top to bottom)
1. **Client cache** — browser/device
2. **CDN** — static content at edge
3. **Reverse proxy** — nginx before app servers
4. **In-app cache** — local to the process
5. **Distributed cache** — Redis/Memcached shared

### Patterns
| Pattern | How | Tradeoff |
|---|---|---|
| **Cache-aside** | Read cache → miss → read DB → write cache | Simple, can be stale |
| **Write-through** | Write DB + cache together | Consistent, slower writes |
| **Write-back** | Write cache, flush to DB async | Fast, data-loss risk on crash |

### The hard part: invalidation
- TTLs (simple, bounded staleness)
- Versioned keys (invalidate by version bump)
- Event-driven invalidation via CDC (you know this from data pipelines)

### Classic failure: the hot key
One popular key (celebrity profile, trending hashtag) hammers a single node. Fixes: replicate the key, or fan out reads via the database.

---

## 6. Load Balancing & Routing

- **L4 vs L7:** L4 works on TCP (fast, no app logic). L7 works on HTTP (can route by URL/header → enables canary + A/B testing).
- **Algorithms:** round-robin, least-connections, **consistent hashing**.
- **Consistent hashing** keeps most keys in place when a node is added/removed → used by sharded caches and databases.

---

## 7. Asynchronous Processing (decouple everything slow)

```
Client ──▶ API ──▶ QUEUE ──▶ Workers ──▶ DB/Store
                    ▲
              (smooth load spikes,
               enable retries)
```

**Why queue:** producers shouldn't slow down because a consumer is slow or down. Queues smooth spikes, allow retries, and let you scale consumers independently.

### Message semantics (crucial)
- **At-least-once:** every message processed ≥1 time. Need idempotent consumers.
- **Exactly-once:** guaranteed single processing. Hard — usually implemented as at-least-once + dedup (idempotency keys, transaction IDs).

### Kafka vs task queues
- **Kafka:** log, replayable, ordered per partition. For event streams.
- **RabbitMQ / SQS:** task queues, delete-on-ack. For job processing.

---

## 8. Distributed Consistency Patterns

### Two-phase commit (2PC)
Correct but slow and blocking — if a coordinator dies, everyone waits.

### Saga pattern (the modern default)
```
Order created ──▶ Payment debited ──▶ Stock reserved
     │                 │                  │
     └───compensate───┘                  │
          refund                          │
                                         └───compensate───▶ release stock
```
Sequence of local transactions + compensating actions on failure. No locks held across services.

### Transactional outbox
Write the business change AND the outgoing event in the SAME local DB transaction. A relay reads the outbox and publishes to the queue. Avoids the dual-write problem.

### Event sourcing
State = replay of the event log. Powerful for auditability and temporal queries; expensive to rebuild state.

---

## 9. Availability & Failure Handling

### SLA numbers to memorize
| SLA | Downtime / year |
|---|---|
| 99% | 87.6 hours |
| 99.9% | 8.76 hours |
| 99.99% | 52.6 minutes |
| 99.999% | 5.26 minutes |

### Failure techniques
- **Redundancy** — no single point of failure
- **Failover** — active-passive (standby) vs active-active (both serve)
- **Retries** — exponential backoff + jitter (jitter prevents thundering herd)
- **Circuit breaker** — fail fast when downstream is broken; prevent cascading failure
- **Bulkheads** — isolate failure domains (one bad tenant can't kill the whole system)
- **Graceful degradation** — degrade features, not the whole system

### Idempotency (non-negotiable)
Retries MUST be safe. Every write endpoint needs an idempotency key. "The retry must produce the same result as the first attempt."

---

## 10. Practice Set (10-12 designs — explain out loud)

| Design | Key concepts exercised |
|---|---|
| URL shortener | hashing, read-heavy, cache, 301 redirect |
| Rate limiter | token bucket, distributed counters (Redis), sliding window |
| Chat system | websockets, fan-out, message ordering, presence |
| News feed | fan-out-on-write vs fan-out-on-read (THE classic tradeoff) |
| Key-value store | LSM vs B-tree, replication, consistency |
| Web crawler | queue, dedup (bloom filter), politeness, backpressure |
| Notification system | multi-channel, retries, backpressure |
| Ride-sharing | geospatial index, real-time matching, driver state |
| Video streaming | CDN, DASH segments, buffering, DRM |
| Payment system | exactly-once, idempotency, sagas, ledger |
| Distributed ID generator | snowflake (timestamp + worker + sequence) |
| Search | inverted index, ingestion pipeline, ranking |

**How to practice:** 25 min design + 10 min self-review per problem. Record yourself. A mock session where you struggle is worth 10 hours of reading.

---

## 11. Two-Week Study Plan

**Week 1 — Foundations:**
- Day 1-2: Read sections 1-3 (requirements, scalability, CAP)
- Day 3-4: Read sections 4-5 (storage engines, caching)
- Day 5-6: Read sections 6-8 (load balancing, queues, consistency)
- Day 7: Read section 9 (availability) + skim DDIA chapters on replication

**Week 2 — Practice:**
- Day 8-10: Design 4 problems from the practice set (2 per day)
- Day 11-13: Design 4 more
- Day 14: Mock interview (URL shortener + rate limiter) with a friend/recorded

---

## Go Deeper (pick 1-2, not all)

- Book: Designing Data-Intensive Applications - https://www.oreilly.com/library/view/designing-data-intensive-applications/9781098119058/
- Book: System Design Interview Vol 1 - https://www.amazon.com/System-Design-Interview-Second-Insiders/dp/B08CMF2CQF
- Primer: https://github.com/donnemartin/system-design-primer
- Linear course: https://github.com/karanpratapsingh/system-design
- Videos: https://www.youtube.com/@ByteByteGo
- Practice list: https://github.com/ashishps1/awesome-system-design-resources
- Diagram tool: https://excalidraw.com