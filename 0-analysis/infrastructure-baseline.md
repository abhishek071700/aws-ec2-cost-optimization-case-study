# 🏗️ Infrastructure Baseline

## Environment Overview

| Detail | Info |
|--------|------|
| Cloud Provider | Amazon Web Services (AWS) |
| Service | EC2 (Elastic Compute Cloud) |
| Region | ap-south-1 (Mumbai) |
| Analysis Period | 1 Month |
| Total Instances | 29 |
| Pricing Models | On-Demand (majority) + Spot (minority) |

*Exact cost figures withheld under NDA.*

---

## Instance Fleet Breakdown

### By Utilization Status

| Status | Count | % of Fleet |
|--------|-------|-----------|
| 🔴 Under-utilized (CPU < 50%) | 20 | 69% |
| 🟡 Balanced (CPU 50–75%) | 3 | 10% |
| 🟢 High (CPU > 75%) | 6 | 21% |

### By Purchase Type

| Purchase Model | Count |
|----------------|-------|
| On-Demand | 25 |
| Spot | 4 |

### Instance Family Distribution

| Family | Purpose | Count |
|--------|---------|-------|
| `t2` / `t3` / `t3a` | Burstable — dev, web, small workloads | 13 |
| `m6a` | General purpose — databases, app servers | 5 |
| `c6a` / `c5a` | Compute-optimized — processing workloads | 11 |

---

## Key Observations

- Majority of the fleet (69%) was running below 50% CPU
- Several instances in the `m6a` and `c6a` families were significantly over-provisioned
- Spot instances were already on discounted pricing — a detail the optimization tool initially missed
- A small number of instances were running at or near 100% CPU and required no changes

---

## What I Did

1. Logged into AWS Management Console
2. Navigated to EC2 → Instances → viewed all running instances
3. Reviewed instance types, purchase options, and basic CloudWatch metrics
4. Exported data to a cost optimization tool for deeper analysis
5. Used this baseline to prioritize which instances to investigate further

👉 See [utilization-findings.md](./utilization-findings.md) for detailed CPU analysis.
