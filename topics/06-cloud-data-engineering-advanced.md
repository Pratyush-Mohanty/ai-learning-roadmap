# Advanced Cloud & Data Engineering (Comprehensive)

**Estimated time: 3 weeks.** Cloud-agnostic fundamentals + modern data architecture. The layer beneath both GCP certs and AI engineering.

---

## Part A - Cloud Computing

### 1. Compute Models (choose by workload)

| Primitive | What it is | When |
|---|---|---|
| IaaS (VM) | Full control, you manage OS/patching | Need the OS, GPUs, custom software |
| Containers + K8s | Portable, immutable images; orchestrated | Containerized apps at scale |
| Serverless containers | Scale-to-zero, no infra mgmt | Containerized HTTP services, bursty traffic |
| Functions | Event-driven snippets | Short-lived event handlers |

```
Workload → Need OS/GPU? → VM
        → Containerized, steady state? → Kubernetes
        → Containerized, bursty/scale-to-zero? → Serverless containers
        → Event-driven, short-lived? → Functions
```

Tradeoff: control vs ops burden vs cost. Serverless for bursty/low traffic; containers for steady; VMs for full control.

### 2. Networking Essentials

- **VPC** — private network: subnets, regions, CIDR
- **Load balancing:** L4 (TCP) vs L7 (HTTP, URL-based routing); global vs regional
- **DNS, NAT, firewalls, IAP** (identity-aware proxy) for private access
- **Hybrid:** VPN / Direct Connect for on-prem

**Decision logic:** "private instance, no public IP, reachable from on-prem" → VPC peering / VPN / Private Service Connect.

### 3. Storage Hierarchy

```
Object storage (GCS/S3)  → data lake, backup, cold
File storage (Filestore) → shared NFS
Block storage (EBS)      → VMs
Relational               → transactional truth
NoSQL                    → hot app data
```

### 4. IAM & Security (the 4 As)

1. **Authentication** — who are you
2. **Authorization** — least privilege; service accounts with scoped roles
3. **Audit** — logs everywhere (Cloud Audit Logs)
4. **Asset protection** — secrets in Secret Manager, KMS encryption, no hardcoded keys

### 5. Cost Management

- On-demand vs committed-use discounts vs spot/preemptible
- Right-size + autoscale + storage lifecycle (hot → cold → archive)
- Tag everything; budgets + alerts. **Idle resources are the #1 waste.**

### 6. Reliability

- **SLI** (actual measured) → **SLO** (target) → **SLA** (contract). "99.9%" is a budget, not a freebie.
- Design for failure: multi-AZ/region, retries + backoff, idempotency, graceful degradation.
- **IaC:** Terraform for everything. Environments = code, reviewable, repeatable.

---

## Part B - Data Engineering

### 1. The Architecture Evolution

```
Warehouse (structured, schema-on-write)  ┐
                                          ├──▶ Lakehouse (open formats + warehouse speed)
Data lake (raw, schema-on-read)          ┘
```

**The 2026 default:** lakehouse with **Apache Iceberg** — ACID on the lake, time travel, schema evolution, multi-engine access.

- **ELT over ETL:** load raw first, transform in-warehouse (dbt/Dataform). Cheaper, flexible, keeps raw data.
- **Data mesh:** data owned by domain teams, exposed as products; governance over centralization.

### 2. Batch vs Streaming

| Dimension | Batch | Streaming |
|---|---|---|
| Freshness | Daily/hourly | Seconds-minutes |
| Cost | Cheap | More expensive |
| Reprocessing | Easy | Hard |
| When | 90% of pipelines | Dashboards, alerts, fraud, event-driven |

**Kappa over Lambda:** one streaming path; replay = reprocess. Micro-batch for simplicity.

### 3. Streaming Fundamentals (the hard 20%)

- **Windowing:** tumbling (fixed), sliding, session
- **Watermarks + late data** — the part that breaks pipelines in production
- **Exactly-once** is usually at-least-once + idempotency/dedup
- **CDC** (Change Data Capture): Debezium/connectors keep lake/warehouse fresh from OLTP

### 4. Orchestration vs Transformation

- **Orchestrate:** Airflow/Composer — dependencies, retries, scheduling
- **Transform:** dbt (SQL, versioned, tested, in-warehouse) / Dataflow / Spark
- Orchestrator coordinates; pipelines transform.

### 5. Data Quality & Governance

- Tests at every stage: freshness, volume, schema (dbt tests, Great Expectations)
- Catalog, lineage, data contracts (schema + semantics + SLA), PII masking, retention
- **Data contracts = the API for data**

### 6. The 2026 Data Platform (memorize this shape)

```
Sources (OLTP/CDC, logs, SaaS)
    │
    ▼
Ingestion (Pub/Sub, Kafka, batch)
    │
    ▼
Lakehouse (Iceberg on object storage)
    │
    ▼
Transform (Dataflow / dbt)
    │
    ▼
Serving (Warehouse, vector DB, feature store)
    │
    ▼
Consumers (analytics, ML, LLM apps)

Observability + Governance + Quality across all layers
```

This is the pattern both GCP certs test. Memorize the shape, then map services.

---

## Three-Week Study Plan

**Week 1 — Cloud fundamentals:**
- Compute models, networking, storage, IAM
- Terraform basics

**Week 2 — Data architecture:**
- Lakehouse + Iceberg concepts
- Batch vs streaming, windowing, CDC

**Week 3 — Tooling + practice:**
- dbt project, Great Expectations tests
- Map the 2026 platform onto GCP services

---

## Go Deeper

- Book: Designing Data-Intensive Applications - https://www.oreilly.com/library/view/designing-data-intensive-applications/9781098119058/
- dbt: https://github.com/dbt-labs/dbt-core
- Great Expectations: https://github.com/great-expectations/great_expectations
- Debezium (CDC): https://github.com/debezium/debezium
- Airflow: https://github.com/apache/airflow
- Iceberg: https://github.com/apache/iceberg
- Terraform: https://github.com/hashicorp/terraform