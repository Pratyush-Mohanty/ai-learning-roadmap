# GCP Data Engineering (Professional Data Engineer)

Core services: BigQuery, Dataflow (Apache Beam), Pub/Sub, Cloud Storage, Dataproc, Cloud Composer, Data Fusion, Bigtable vs Firestore vs Spanner, IAM.

## Prep Path (in order)
1. **Official exam guide PDF** — https://cloud.google.com/learn/certification/data-engineer
   Read before anything else. The exam is scenario-based with ~50% first-attempt pass rate.
2. **Coursera: Google Cloud Data Engineering Professional Certificate** — https://www.coursera.org/professional-certificates/gcp-data-engineering
   Canonical conceptual coverage + Qwiklabs hands-on labs.
3. **Google Cloud Skills Boost** — https://www.cloudskillsboost.google/paths/16
   Official practice questions + labs. The #1 thing candidates skip.
4. **2+ timed mock exams** before the real test (Whizlabs: https://www.whizlabs.com/google-cloud-certification-exams/data-engineer/)

## What the Exam Actually Tests
- **Cost optimization** — BigQuery on-demand vs capacity pricing, storage class selection (Nearline/Coldline/Archive), committed use discounts
- **Service selection under constraints** — Bigtable vs Firestore vs Spanner tradeoffs (latency, consistency, cost, ops)
- **Secure pipelines** — IAM least privilege, service accounts across storage/processing/analytics
- **Design tradeoffs** — batch vs streaming, cost vs latency vs operational complexity

## Hands-on GitHub
- **Dataflow Templates** — https://github.com/GoogleCloudPlatform/dataflow-templates
  Real Beam pipeline templates.
- **Professional Services** — https://github.com/GoogleCloudPlatform/professional-services
  Real architecture solutions from Google's consulting teams.

## Milestone
Build and secure a streaming pipeline: Pub/Sub ingestion → Dataflow/Beam processing → BigQuery sink, with IAM, monitoring, and cost optimization.