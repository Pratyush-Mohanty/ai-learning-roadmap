# GCP Data Engineering — Advanced (Professional Data Engineer)

How GCP implements the modern data platform, plus exam strategy. The exam is **scenario-based**: it tests *which service under which constraint*, not memorization.

## 1. The GCP Data Stack (map of the platform)
```
Sources → Pub/Sub (stream) / Cloud Storage (batch) / Kafka (Managed)
  → Dataflow (Beam, stream+batch) · Dataproc (Spark/Hive) · Data Fusion (GUI ETL)
  → Storage & Query: BigQuery (warehouse) · BigLake (lakehouse on GCS) · Bigtable (NoSQL)
  → Orchestration: Cloud Composer (Airflow) · Dataform (SQL transform)
  → Governance: Dataplex · Data Catalog · DLP (data loss prevention)
  → ML/AI: Vertex AI · BigQuery ML · Gemini in BigQuery
```

## 2. BigQuery — the center of gravity
- **What it is:** serverless, petabyte-scale warehouse with columnar storage (Capacitor), separate compute/storage, **slots** for query concurrency.
- **Key decisions to master:**
  - **Partitioning** (by date/column → prune scanned bytes = cost) vs **clustering** (co-locate similar values → faster, cheaper reads). Partition for cost, cluster for speed.
  - **Slot management:** on-demand (pay per query) vs **capacity** (reserved slots + autoscaling). Cost questions always involve this.
  - **Storage lifecycle:** active vs long-term (90-day discount) vs BigLake external tables vs Iceberg managed tables.
  - **Speed up:** materialized views, `SELECT * EXCEPT`, avoiding UDF overhead, BI engine (optional, legacy).
  - **Real-time:** Storage Write API (gRPC streaming) — the modern ingest path.
- **BigQuery ML / Gemini in BigQuery:** train models + run LLM analytics **in SQL**. Your differentiator as a data engineer: *"call a model inside the warehouse"* — do a Gemini-in-BigQuery project.

## 3. BigLake + Iceberg = the GCP lakehouse (2026 default)
- **BigLake** = a bridge: external tables over GCS with fine-grained security, no data copy (non-extract-load pattern).
- **Apache Iceberg managed tables in BigQuery:** open table format with ACID, **schema evolution, time travel, auto-clustering, streaming via Storage Write API, column-level security, multi-engine access** (Spark, Flink, Trino, BigQuery all read the same table). Multi-table transactions (GA).
- **The GCP Iceberg stack:** GCS (files) + BigLake Metastore (REST catalog) + BigQuery (query) + Dataplex (governance) + Vertex AI (ML). This is what a modern "lakehouse on GCP" looks like — know this architecture for both interviews and cert.

## 4. Dataflow (Apache Beam) — streaming & batch
- **Model:** pipeline of transforms; same code for batch + stream (portability).
- **Runner v2, Streaming Engine, Shuffle Service:** the managed execution layers — know what each does.
- **Streaming concepts tested:** windows (tumbling/sliding/session), **watermarks**, late-data triggers, side inputs.
- **Beam SDK skills:** ParDo, GroupByKey/Combine, state/timers (for joins/dedup at scale), I/O connectors (Pub/Sub, Kafka, BigQuery, GCS, Iceberg).

## 5. Pub/Sub (and when it's the right choice)
- Push vs pull; **exactly-once delivery** (2024+); ordering with message ordering; subscriber throughput vs `max_outstanding_messages`; **dead-letter topics** for poison messages; retries + backoff.
- **Alternatives:** Kafka (Managed) when you need partitioning/retention/replay semantics; Pub/Sub for managed event delivery without consumer state.

## 6. Orchestration: Composer vs Dataform
- **Cloud Composer (Airflow):** orchestrate external steps (Spark job → validation → dbt run → send notification). Retries, SLA checks, task dependencies. Heavy infra (GKE under the hood).
- **Dataform:** **SQL-based ELT** in BigQuery — versioned, tested, with environments (dev/staging/prod). The "dbt for GCP." Use for in-warehouse transforms; Composer for cross-service orchestration.

## 7. Dataproc (Spark) vs Dataflow — the classic exam question
| Dimension | Dataflow | Dataproc |
|---|---|---|
| Model | Beam (managed, serverless) | Spark/Hadoop (cluster) |
| Ops | No cluster to manage | You manage (or use Serverless) |
| Cost | Pay per work | Pay for cluster time |
| Best for | Streaming, GCP-native, portability | Existing Spark/Hive skills, ML training pre-processing |
Use Dataflow for greenfield streaming; Dataproc when you already run Spark or need on-prem portability.

## 8. Bigtable vs Firestore vs Spanner (the storage tradeoff question)
- **Bigtable:** wide-column NoSQL, **millisecond latency, millions of QPS, single key, high write throughput** — for Hot-write analytics/ads/telemetry. Not a relational system. LSM-based.
- **Firestore:** document, mobile/web apps, strong consistency, managed, autoscaling.
- **Spanner:** **globally distributed relational** with SQL + external consistency (CP at planetary scale). When you need relational semantics + horizontal scale across regions. Expensive.
- Rule: relational + global → Spanner. Relational + single-region → Cloud SQL/AlloyDB. Wide-column + high QPS → Bigtable. App/mobile → Firestore.

## 9. Security & Governance (exam-heavy)
- **IAM:** least privilege, **service accounts** (never user keys in code), workload identity federation.
- **Column-level security** + **data masking** + DLP for PII (DICOM/medical scenarios).
- **Dataplex:** catalog, lineage, data quality, policy across lake+warehouse.
- **VPC-SC** for data exfiltration control; **CMEK/CSEK** encryption keys.

## 10. Exam Strategy (from the ~50% first-pass rate)
- Read the **official exam guide** first: https://cloud.google.com/learn/certification/data-engineer
- Coursera *Google Cloud Data Engineering Professional Certificate*: https://www.coursera.org/professional-certificates/gcp-data-engineering
- **Google Cloud Skills Boost** practice + labs: https://www.cloudskillsboost.google/paths/16 (the #1 thing candidates skip)
- Do **2+ timed mock exams** (Whizlabs). It's scenario-based — practice *eliminating* wrong options under a reason: "this fails on cost/latency/consistency."
- Get hands-on — candidates who only did courses fail the applied scenarios.

## 11. Your Differentiators (data engineer)
- **Iceberg + BigLake** — build the lakehouse; nobody in the room will know it better
- **Gemini in BigQuery** — LLM + warehouse in one demo
- **Streaming with correct windowing/watermarks** — show you understand the hard 20%

## Go Deeper
- **Official docs:** BigQuery — https://cloud.google.com/bigquery/docs · Iceberg in BigQuery — https://cloud.google.com/bigquery/docs/biglake-iceberg-tables-in-bigquery
- **Repos:** dataflow-templates → https://github.com/GoogleCloudPlatform/dataflow-templates · professional-services → https://github.com/GoogleCloudPlatform/professional-services
- **Course:** DataCamp Google Cloud Data Engineer track → https://www.datacamp.com/tracks/google-cloud-data-engineer
- **Architecture refs:** Google Cloud architecture center — https://cloud.google.com/architecture