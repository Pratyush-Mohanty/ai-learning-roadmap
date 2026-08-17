# Advanced Cloud & Data Engineering Concepts

Cloud-agnostic fundamentals + modern data architecture. This is the "how systems actually run" layer beneath both GCP cert paths and AI engineering.

## Part A — Cloud Computing Concepts (advanced)

### 1. Compute models (choose by workload)
- **IaaS (VM):** full control, you manage OS/patching. Compute Engine, EC2.
- **Containers:** portable, immutable images. Docker + orchestration by **Kubernetes (GKE/EKS/AKS)** — the industry standard; learn pod/service/deployment/autoscaling, not just Docker.
- **Serverless:** scale-to-zero, no infra management. Cloud Run (containers, best of both), Cloud Functions (event-driven snippets).
- **Tradeoff:** control vs ops burden vs cost efficiency. Serverless shines for bursty/low traffic; containers for steady state; VMs when you need the OS.

### 2. Networking essentials
- **VPC** — your private network: subnets, regions, CIDR.
- **Load balancing:** L4 vs L7; global (multi-region) vs regional.
- **DNS, NAT, firewalls, IAP** (identity-aware proxy) for private access.
- **Service mesh** (Istio): east-west traffic, mTLS, canary — advanced GKE.

### 3. Storage hierarchy
Object storage (GCS/S3) → file (Filestore/EFS) → block (Persistent Disk/EBS) → relational → NoSQL. Know when each: **object = data lake/backup, block = VMs, NoSQL = hot app data, relational = transactional truth.**

### 4. IAM & Security
- **Least privilege** — service accounts with scoped roles, not shared keys.
- **Four As:** Authentication, Authorization, Audit, and **Keys/Secret management** (Secret Manager/KMS).
- **Network security:** private networking, VPC-SC (data exfiltration prevention), org policies.

### 5. Cost management
- On-demand vs committed use discounts vs spot/preemptible.
- **Right-sizing** + autoscaling + storage lifecycle (hot→cold→archive).
- Tag everything; use budgets/alerts. Cloud is pay-as-you-go — **idle resources are the #1 waste.**

### 6. Reliability
- **Design for failure:** multi-AZ/region, retries + backoff, idempotency, graceful degradation.
- **SLI/SLO/SLA:** measure actuals (SLI) → targets (SLO) → contracts (SLA). "99.9% availability" is a *budget*, not a freebie.
- **IaC:** Terraform for everything. Environments = code, reviewable, repeatable. (See https://github.com/hashicorp/terraform)

## Part B — Data Engineering (advanced)

### 1. The architecture evolution
- **Warehouse** (structured, schema-on-write; BigQuery, Snowflake) → **Data lake** (raw, schema-on-read; cheap, ungoverned) → **Lakehouse** (open table formats on object storage + warehouse query performance; the 2024-2026 default).
- **Open table formats:** **Apache Iceberg** (the winner) + Delta + Hudi. ACID on the lake, time travel, schema evolution, multi-engine access.
- **ELT over ETL:** load raw first, transform in-warehouse (dbt/Dataform). Cheaper, more flexible, keeps raw data.
- **Data mesh (org pattern):** data owned by domain teams, exposed as products. Governance > centralized pipelines.

### 2. Batch vs Streaming — know the real tradeoffs
- **Batch:** predictable, cheaper, easier to reprocess/backfill. 90% of pipelines.
- **Streaming:** sub-second to minutes freshness. Only for: real-time dashboards, fraud/ops alerts, event-driven apps.
- **Lambda (2 parallel paths)** vs **Kappa (one streaming path)**: Kappa preferred today; replay = reprocess.
- **Micro-batch:** streaming semantics at low latency with batch simplicity (Dataflow default).

### 3. Streaming fundamentals
- **Events vs batches; windowing:** tumbling (fixed), sliding, session; **watermarks + late data** handling (the hard part).
- **Exactly-once** vs at-least-once + idempotency (dedupe keys). Most "exactly-once" systems are at-least-once with dedup.
- **CDC (Change Data Capture):** Debezium/Dataflow connectors keep lake/warehouse fresh from OLTP; enable reverse-ETL.
- **Kafka:** log-based, ordered per partition, replayable — the backbone for event-driven.

### 4. Orchestration & transformation
- **Orchestration:** Airflow/Cloud Composer (workflow as code, retries, dependencies) — vs DAG-in-pipeline (Beam). Orchestrator coordinates, pipelines transform.
- **Transformation:** dbt (SQL, in-warehouse, versioned, tested — the analytics engineering standard; https://github.com/dbt-labs/dbt-core) · Dataform (GCP-native) · Spark/Beam (heavy compute ETL).

### 5. Data quality & governance
- **Quality:** fresh/volume/schema tests at every stage (dbt tests, Great Expectations https://github.com/great-expectations/great_expectations), validation gates before publish.
- **Governance:** catalog (Dataplex/Data Catalog), lineage, data contracts, masking for PII, retention policies.
- **Data contracts:** schema + semantics + SLA between producer and consumer — the "API for data."

### 6. The Data Platform of 2026 (for your reference architecture)
```
Sources (OLTP/CDC, logs, SaaS) → Ingestion (Pub/Sub, Kafka, batch) 
→ Lakehouse (Iceberg on GCS + BigLake) → Transform (Dataflow/dbt) 
→ Serving (BigQuery, vector DB, feature store) 
→ Consumers (analytics, ML, LLM apps)
→ Observability + Governance + Quality across everything
```
This is the pattern both GCP certs test you on — memorize the *shape*, then map services.

## Go Deeper
- **Books:** Designing Data-Intensive Applications → https://www.oreilly.com/library/view/designing-data-intensive-applications/9781098119058/ · The Data Warehouse Toolkit (Kimball) for dimensional modeling
- **dbt:** https://github.com/dbt-labs/dbt-core
- **Great Expectations:** https://github.com/great-expectations/great_expectations
- **Debezium (CDC):** https://github.com/debezium/debezium
- **Airflow:** https://github.com/apache/airflow
- **Iceberg:** https://github.com/apache/iceberg
- **Kubernetes:** https://github.com/kubernetes/kubernetes · Terraform: https://github.com/hashicorp/terraform