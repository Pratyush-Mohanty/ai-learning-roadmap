# GCP Cloud Engineering — Associate → Professional Architect

Entry-level cloud engineering, then advanced architecture. Do this before the Data Engineer cert if GCP is new to you.

## 1. Compute — pick the right primitive
| Service | What it is | When |
|---|---|---|
| Compute Engine (VMs) | IaaS | Need the OS, GPUs, custom software |
| GKE (Kubernetes) | Managed K8s | Containerized apps at scale, portability |
| Cloud Run | Serverless containers | Containerized, scale-to-zero, HTTP services |
| Cloud Functions | Serverless functions | Event-driven, short-lived snippets |
| App Engine | PaaS | Legacy GCP-native apps |

**Exam logic:** web service, containerized, wants scale-to-zero → **Cloud Run**. Event on GCS triggers short job → **Cloud Functions**. Long-running stateful or GPU → **Compute Engine/GKE**.

## 2. Storage decisions
- **Cloud Storage (GCS):** object storage. Classes: Standard → Nearline (30d) → Coldline (90d) → Archive (365d). **Lifecycle rules** auto-transition → cost questions love this. Buckets: regional/multi-regional/dual-region.
- **Persistent Disk:** block storage attached to VMs (SSD/HDD, zonal/regional).
- **Filestore:** NFS for shared file systems.
- **Cloud SQL:** managed Postgres/MySQL — single-region relational.
- **Cloud Spanner:** globally distributed relational (CP).
- **AlloyDB:** Postgres-compatible, transactional+analytical (advanced alternative to Cloud SQL).

## 3. Networking (memorize the primitives)
- **VPC, subnets, firewall rules** (default-deny + explicit allows), **routes**, Cloud NAT, Cloud DNS.
- **Load balancing:** global HTTP(S) L7 (URL-based routing) vs regional TCP/UDP L4 vs internal LB.
- **Cloud Armor:** WAF + DDoS. **IAP:** identity-aware access to internal apps. **Cloud VPN / Interconnect:** hybrid connectivity.
- Exam loves: *"private instance, no public IP, reachable from on-prem"* → VPC peering / Private Service Connect / VPN.

## 4. IAM & Security
- **Roles:** primitive (legacy) < predefined (per-service) < custom (least privilege).
- **Service accounts** + **Workload Identity** for GKE; short-lived credentials; no hardcoded keys.
- **Best practices:** org policies, VPC Service Controls, Cloud KMS/CMEK for encryption, Secret Manager.
- **Audit:** Cloud Audit Logs + Cloud Logging/Monitoring.

## 5. IaC & CI/CD (the modern expectation)
- **Terraform** for all infrastructure (state, modules, review). https://github.com/hashicorp/terraform
- **Cloud Build** for CI/CD; **Cloud Deploy** for progressive delivery (canary/rollback).
- GKE delivery: Artifact Registry, GitOps (Config Sync/Argo CD).
- Exam increasingly tests *"how do you deploy config changes safely"* → IaC + review + canary.

## 6. Cost management
- **Committed Use Discounts** (1/3-yr) for steady VMs; **spot** for batch/fault-tolerant; **autoscaling** right-sizing.
- GCS lifecycle to cold classes; BigQuery slot reservations.
- Budgets + alerts + billing export to BigQuery (for cost analytics — your data skills help).

## 7. Reliability & Migration
- **HA:** managed instance groups (MIGs) with health checks + autoscaling; multi-region deployments; Cloud DNS failover.
- **Backups:** snapshots, GCS versioning, Cloud SQL PITR.
- **Migration:** Migrate for Compute Engine (lift-and-shift) vs modernization to Cloud Run/GKE.

## 8. Path to Professional Cloud Architect (optional, after Associate)
Advanced topics: HA architectures, security design, cost/performance optimization, migration planning, ML on GCP (Vertex AI). Best combined with the concepts in [06-cloud-data-engineering-advanced.md](06-cloud-data-engineering-advanced.md) and [01-system-design.md](01-system-design.md).

## 9. Exam Strategy (hands-on heavy)
- Official Skills Boost Cloud Engineer path: https://www.cloudskillsboost.google/paths/9 (free credits)
- Coursera *Preparing for Associate Cloud Engineer*: https://www.coursera.org/learn/preparing-cloud-associate-cloud-engineer-certification-exam
- **Do the labs** — courses alone won't pass it. Then mocks (Whizlabs, ExamPro).
- Exam asks *"choose the resource/compute/storage/networking"* under a constraint → practice decision tables above, not recall.

## 10. Your Differentiators (data engineer)
- Cost analytics: build a **billing BigQuery dataset + Looker dashboard** project — shows you can operationalize GCP cost.
- IaC + data: Terraform-provision the GCP data stack from [07-gcp-data-engineering.md](07-gcp-data-engineering.md).

## Go Deeper
- **Certification pages:** Associate Cloud Engineer — https://cloud.google.com/learn/certification/cloud-engineer · Professional Cloud Architect — https://cloud.google.com/learn/certification/cloud-architect
- **Architecture center:** https://cloud.google.com/architecture
- **Labs:** Qwiklabs catalog — https://www.cloudskillsboost.google/catalog
- **Terraform GCP provider:** https://github.com/hashicorp/terraform-provider-google