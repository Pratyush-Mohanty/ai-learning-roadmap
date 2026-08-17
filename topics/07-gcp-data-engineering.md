# GCP Data Engineering - Professional Data Engineer (Comprehensive)

**Estimated time: 4-5 weeks including exam prep.** The exam is **scenario-based**: it tests *which service under which constraint*, not memorization.

---

## 1. The GCP Data Platform in One Diagram

```
Sources (OLTP/CDC, logs, SaaS)
   │
   ├──▶ Pub/Sub (streaming) ──▶ Dataflow (Beam, stream+batch)
   │                          │
   └──▶ Cloud Storage (batch) ─┴─▶ Dataproc (Spark/Hive)
                                   │
                                   ▼
                    BigLake over Iceberg on GCS (lakehouse)
                                   │
                                   ▼
                              BigQuery (warehouse)
                                   │
                                   ▼
                     Vertex AI / Gemini in BigQuery

Cloud Composer orchestrates all pipeline steps
Dataplex governs catalog + lineage + quality
```

---

## 2. BigQuery — the Center of Gravity

### What it is
- Serverless petabyte warehouse. Columnar storage (Capacitor), separate compute/storage, **slots** for concurrency.

### Key decisions to master
| Decision | What | Rule |
|---|---|---|
| Partitioning | Split table by date/column | Prunes scanned bytes = cost |
| Clustering | Co-locate similar values | Faster, cheaper reads |
| Slots | On-demand vs capacity | Cost questions always involve this |
| Storage | Active vs long-term vs Iceberg/BigLake | 90-day discount on long-term |

- **On-demand** = pay per query. **Capacity** = reserved slots + autoscaling (predictable cost).
- **Real-time:** Storage Write API (gRPC) — the modern ingest path.
- **Speed:** materialized views, partition pruning, avoid UDF overhead.
- **BigQuery ML / Gemini in BigQuery:** train models + run LLM analytics IN SQL. Your differentiator.

---

## 3. BigLake + Iceberg = the GCP Lakehouse (2026 default)

```
Cloud Storage (Iceberg files: Parquet + metadata)
        │
   BigLake Metastore (REST catalog)
        │
   ┌────┼───────────────┐
   ▼    ▼               ▼
BigQuery  Spark/Flink   Trino
        │
   Dataplex (governance) · Vertex AI (ML)
```

### Why it matters
- **Open table format** (Apache Iceberg): ACID, schema evolution, time travel, auto-clustering
- **Multi-engine access:** BigQuery, Spark, Flink, Trino read the same table
- **Streaming via Storage Write API**, column-level security, multi-table transactions (GA)
- **Non-extract-load** — query data in place, no copies

**This is the modern lakehouse on GCP. Know this architecture cold for cert and interviews.**

---

## 4. Dataflow (Apache Beam) — Streaming & Batch

### Mental model
Same code for batch + stream. Pipeline of transforms. Managed execution (Runner v2, Streaming Engine, Shuffle Service).

### Streaming concepts tested
- **Windows:** tumbling (fixed), sliding, session
- **Watermarks** + late-data triggers
- **Side inputs**

### SDK skills
- ParDo, GroupByKey/Combine
- State/timers (for joins, dedup at scale)
- I/O connectors: Pub/Sub, Kafka, BigQuery, GCS, Iceberg

---

## 5. Pub/Sub (and when it's right)

- Push vs pull; ordering; exactly-once delivery (2024+)
- **Dead-letter topics** for poison messages
- Retries + backoff; `max_outstanding_messages` for throughput tuning

**Alternative:** Kafka (Managed) when you need partition/retention/replay semantics; Pub/Sub for managed delivery.

---

## 6. Orchestration: Composer vs Dataform

| | Cloud Composer (Airflow) | Dataform |
|---|---|---|
| What | Orchestrate external steps | SQL ELT in BigQuery |
| Use for | Cross-service workflows | In-warehouse transforms |
| Model | Python DAGs, retries, SLAs | Versioned, tested SQL, environments |

Rule: Dataform for in-warehouse transforms; Composer for cross-service orchestration.

---

## 7. The Classic Tradeoff Questions

### Dataflow vs Dataproc
| Dimension | Dataflow | Dataproc |
|---|---|---|
| Model | Beam, serverless | Spark/Hadoop, cluster |
| Ops | None to manage | You manage |
| Cost | Pay per work | Pay for cluster time |
| Best for | Streaming, greenfield, portability | Existing Spark skills, ML pre-processing |

### Bigtable vs Firestore vs Spanner

```
Need relational + global scale?      → Spanner (SQL + external consistency)
Need wide-column, millions QPS?      → Bigtable (single-key, LSM, high write throughput)
Need mobile/web app document store?  → Firestore (autoscaling, strong consistency)
Need relational + single region?     → Cloud SQL / AlloyDB
```

---

## 8. Security & Governance (exam-heavy)

- **IAM least privilege** — service accounts, workload identity
- **Column-level security + data masking + DLP** for PII
- **Dataplex:** catalog, lineage, data quality, policies
- **VPC-SC** (exfiltration control), **CMEK/CSEK** encryption

---

## 9. The Exam, Strategically

### Facts about the exam
- Scenario-based, ~50% first-attempt pass rate
- Tests *service selection under constraints*, cost optimization, secure pipelines
- Hands-on experience decides pass/fail — courses alone won't do it

### Prep path
1. Official exam guide: https://cloud.google.com/learn/certification/data-engineer
2. Coursera cert: https://www.coursera.org/professional-certificates/gcp-data-engineering
3. Skills Boost practice + labs: https://www.cloudskillsboost.google/paths/16 (the #1 thing people skip)
4. 2+ timed mock exams (Whizlabs)

### Practice technique
For each question, eliminate options by constraint: "this fails on cost / latency / consistency / ops overhead." Don't memorize service lists — memorize decision rules.

---

## 10. Your Differentiators (data engineer)

- **Iceberg + BigLake** — build the lakehouse; nobody in the room will know it better
- **Gemini in BigQuery** — LLM + warehouse in one demo
- **Correct windowing/watermarks** — show you understand the hard 20% of streaming

---

## Five-Week Study Plan

**Week 1 — BigQuery:**
- Partitioning, clustering, slots, storage classes
- Storage Write API, BigQuery ML basics

**Week 2 — Lakehouse:**
- Iceberg concepts, BigLake, multi-engine access
- Build a BigLake Iceberg table lab

**Week 3 — Pipelines:**
- Dataflow/Beam (windows, watermarks, I/O)
- Composer vs Dataform; Pub/Sub + dead-letter

**Week 4 — Storage + security:**
- Bigtable/Firestore/Spanner decision tables
- IAM, column-level security, Dataplex, VPC-SC

**Week 5 — Exam:**
- Skills Boost practice + 2 timed mocks
- Review weak areas; book the exam

---

## Go Deeper

- BigQuery docs: https://cloud.google.com/bigquery/docs
- Iceberg in BigQuery: https://cloud.google.com/bigquery/docs/biglake-iceberg-tables-in-bigquery
- Dataflow templates: https://github.com/GoogleCloudPlatform/dataflow-templates
- Professional services: https://github.com/GoogleCloudPlatform/professional-services
- DataCamp track: https://www.datacamp.com/tracks/google-cloud-data-engineer
- Architecture center: https://cloud.google.com/architecture