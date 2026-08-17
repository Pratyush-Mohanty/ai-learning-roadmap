# GCP Cloud Engineering - Professional Cloud Architect (Comprehensive)

**Estimated time: 4-5 weeks including exam prep.** The architect exam is about **decisions** — compute, networking, storage, and migration — under constraints of cost, reliability, and security.

---

## 1. Compute — The Decision Tree

```
Workload needs full OS / GPU / legacy software?  ──▶ Compute Engine (VM)
Containerized, need orchestration + control?    ──▶ GKE
Containerized, HTTP, scale-to-zero, no ops?     ──▶ Cloud Run
Event-driven, short-lived, small jobs?          ──▶ Cloud Functions
Need to build a large ML model (TPUs)?          ──▶ Vertex AI / TPU
```

### GKE in one screen
- **Control plane** is Google-managed
- **Autoscaling:** HPA (replicas) + cluster autoscaler (nodes) + VPA (resources)
- **Cost:** Spot nodes (cheap, preemptible), committed-use discounts
- **Right-sizing wins cost battles**: 40% of cloud spend is wasted on over-provisioned instances. Right-size + autoscale before adding anything.

### Serverless, the practical choice
- **Cloud Run** — scale-to-zero containers, request-based billing. Most common recommendation for standard HTTP services.
- **Cloud Functions** — event handlers (GCS triggers, Pub/Sub, Firestore)
- **App Engine** — classic PaaS; fewer new deployments in 2026

---

## 2. Networking — VPC Fundamentals

```
VPC ── subnets (per region) ── instances / GKE / Cloud Run
  │
  ├──▶ Global HTTPS LB (L7) ──▶ backend services / serverless
  ├──▶ TCP/UDP LB (L4) for stateful protocols
  ├──▶ Cloud NAT (outbound from private instances)
  ├──▶ IAP (no public IP — identity-based access to SSH/RDP)
  ├──▶ VPC peering / Shared VPC (org-level)
  └──▶ Cloud VPN / Interconnect (hybrid: on-prem <-> GCP)
```

### Key decisions to master
| Decision | When |
|---|---|
| Global vs regional LB | Global = one anycast IP, near users; regional = lower latency controls |
| Cloud NAT | Private instances need outbound internet (updates) |
| IAP | Private access to SSH/console without public IPs |
| Shared VPC | Centralized network control across teams |
| VPC peering vs Interconnect | Peering = GCP-to-GCP; Interconnect = on-prem via Google's network |

---

## 3. Storage — Match the Service to the Data

```
High performance single-node (databases)  ──▶ Persistent Disk (SSD/HDD)
Shared file access (NFS)                  ──▶ Filestore
Object storage, huge, versioned           ──▶ Cloud Storage (GCS)
Archive                                    ──▶ Archive storage class
In-memory cache                            ──▶ Memorystore (Redis)
Analytics warehouse                        ──▶ BigQuery
Transactional relational                  ──▶ Cloud SQL / Spanner
```

### GCS lifecycle rule (cost control)
Hot (Standard) → 30 days → Nearline → 90 days → Coldline → 365 days → Archive.
Automatic lifecycle policies move data down the tiers — set them once.

---

## 4. Databases — Relational & Scale

```
Single region, standard relational?  ──▶ Cloud SQL (Postgres/MySQL/SQL Server)
Read-heavy at scale?                 ──▶ AlloyDB (Postgres-compatible, 4x Cloud SQL reads)
Global scale, SQL, strong consistency? ──▶ Spanner
Document, mobile/web app data?       ──▶ Firestore
Wide-column, massive write throughput? ──▶ Bigtable
```

**Spanner vs Cloud SQL** — the two big decision points:
- Cloud SQL: single region, HA within region, ~10k reads/s ceilings
- Spanner: global, strong consistency across regions, unlimited scale; costs more

---

## 5. Big Data & ML (architect-level overview)

- **BigQuery** — the warehouse (deep-dived in topic 07)
- **Pub/Sub** — event ingestion
- **Dataflow / Dataproc** — processing
- **Dataplex** — governance
- **Vertex AI** — ML platform (models, Pipelines, Agents, GenAI)
- **BigLake + Iceberg** — the lakehouse default

---

## 6. Reliability & Disaster Recovery

### The 4 disaster-recovery strategies (know the ordering)
1. **Backup/Restore** — cheapest, slowest RTO (hours)
2. **Pilot light** — minimal standby; scale up on failover (RTO: minutes-hours)
3. **Warm standby** — running minimal full-stack (RTO: minutes)
4. **Multi-site active-active** — both regions serving (RTO: seconds)

### Design principles
- Design for failure: retries + exponential backoff, idempotency, graceful degradation
- **SLI → SLO → SLA** mindset (you manage this with SRE-style practice)
- Region pairs / multi-region where SLAs demand

### GKE resilience
- Multi-zone clusters, pod disruption budgets, node pools per zone
- **Cheaper** to over-provision compute before an event than to enable autoscaling during it (tested concept).

---

## 7. Security — The 4 Pillars (architect-level)

1. **Identity:** Google Cloud IAM, least privilege, service accounts
2. **Network:** VPC-SC (data exfiltration control), Private Google Access, firewalls
3. **Data:** CMEK/KMS, Cloud DLP (PII), secret management
4. **Audit:** Cloud Audit Logs, Organization policies

> IAM + service accounts + VPC-SC + audit logs appear in nearly every scenario question. Learn them cold.

---

## 8. Terraform (IaC) — Make It Habit

- **State** management, modules, `plan`/`apply` workflow
- **Drive resource naming/labels** from IaC (cost + operations)
- **Environments as code** — dev/stage/prod from the same modules
- IaC isn't just a cert topic — it's how teams actually manage GCP in 2026

---

## 9. Exam Strategy

### Facts
- Scenario-based, tests **architectural decision-making**, not memorization
- Frequently repeated themes: compute choice, storage selection, DR strategy, cost, IAM/security, migration

### Prep path
1. Official guide: https://cloud.google.com/learn/certification/cloud-architect
2. Coursera cert: https://www.coursera.org/professional-certificates/gcp-cloud-architect
3. Skills Boost labs: https://www.cloudskillsboost.google/paths/11
4. 2+ timed mock exams (Whizlabs) — review by wrong-answer category

### Practice technique
Eliminate by constraint. Ask per option: does it satisfy cost? latency? ops overhead? consistency? For migration questions: backup/restore → pilot light → warm standby → active-active, in order.

---

## 10. Sample Scenario Walkthrough

**Problem:** 5TB object store, hot 30 days, cold forever; 200k reads/day; must cut costs.

**Answer path:**
1. GCS Standard for hot tier → lifecycle rule to Archive after 30 days
2. Read pattern → Cloud CDN for the hot objects
3. Versioning + retention policy (compliance)
4. Result: hot reads fast, cold data at ~$0.0012/GB-month (Archive)

That's the whole exam in one example: *constraint (cost) → service selection (GCS) → lifecycle → verify.*

---

## Five-Week Study Plan

**Week 1 — Compute + serverless:**
- VM/GKE/Cloud Run/Functions decision tree; autoscaling + right-sizing

**Week 2 — Networking + storage:**
- VPC, LBs, NAT, IAP; GCS classes + lifecycle

**Week 3 — Databases + data/ML:**
- Cloud SQL vs Spanner; BigQuery/Vertex AI overview

**Week 4 — Reliability + security:**
- DR strategies (4 levels), SLO mindset, IAM/VPC-SC/audit; Terraform

**Week 5 — Exam:**
- Skills Boost labs + 2 timed mocks; review by category; book exam

---

## Go Deeper

- GCP architecture center: https://cloud.google.com/architecture
- Cloud Architecture Center videos: https://cloud.google.com/architecture/architecture-center-videos
- CloudRun blueprints: https://github.com/GoogleCloudPlatform/cloud-run-blueprints
- Professional services: https://github.com/GoogleCloudPlatform/professional-services
- Terraform GCP provider: https://github.com/hashicorp/terraform-provider-google
- SRE book: https://sre.google/sre-book/table-of-contents/