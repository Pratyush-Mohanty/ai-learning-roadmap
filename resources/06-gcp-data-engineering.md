# GCP Data Engineering (Professional Data Engineer)

Core services: BigQuery, Dataflow (Apache Beam), Pub/Sub, Cloud Storage, Dataproc, Cloud Composer, Data Fusion, Bigtable vs Firestore vs Spanner, IAM.

## Prep path (in order)
1. **Official exam guide PDF** — `cloud.google.com/learn/certification/data-engineer` (read before anything else; it's scenario-based, ~50% first-pass pass rate)
2. **Coursera: Google Cloud Data Engineering Professional Certificate** — canonical conceptual coverage + Qwiklabs
3. **Google Cloud Skills Boost** (`cloudskillsboost.google`) — official practice questions + labs (the #1 thing candidates skip)
4. **2+ timed mock exams** before the real test (Whizlabs, PassITExams)

## What the exam actually tests
- **Cost optimization** — BigQuery on-demand vs capacity, storage class selection, committed use discounts
- **Service selection under constraints** — Bigtable vs Firestore vs Spanner tradeoffs (latency, consistency, cost, ops)
- **Secure pipelines** — IAM least privilege, service accounts across storage/processing/analytics
- **Design tradeoffs** — batch vs streaming, cost vs latency vs operational complexity

## GitHub (hands-on)
- `github.com/GoogleCloudPlatform/dataflow-templates` — real Beam pipeline templates
- `github.com/GoogleCloudPlatform/professional-services` — real architecture solutions

## Milestone
Build and secure a streaming pipeline: Pub/Sub ingestion → Dataflow/Beam processing → BigQuery sink, with IAM, monitoring, and cost optimization.