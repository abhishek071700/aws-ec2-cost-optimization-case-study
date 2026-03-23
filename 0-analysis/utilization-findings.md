# 📊 Utilization Findings

## CPU Analysis — 1 Month Period

All instances were analyzed over a 1-month window using CloudWatch metrics. Each instance was classified based on its maximum CPU utilization during this period.

---

## Utilization Classification

### 🔴 Under-utilized Instances (CPU < 50%) — 20 Instances

These instances were consistently running well below their allocated capacity.

| Instance Family | Count | CPU Range | Key Observation |
|----------------|-------|-----------|-----------------|
| `m6a.xlarge` | 4 | 11–17% | Paying for 4 vCPU/16GB, using very little |
| `c6a.xlarge` | 3 | 15–24% | Compute instances barely using any compute |
| `c5a.xlarge` (Spot) | 4 | 14–37% | Already on Spot pricing — different action needed |
| `c6a.large` | 2 | 3–4% | Near-idle, extremely low utilization |
| `t3a.medium` | 3 | 11–14% | Small instances still over-provisioned |
| `t3.large` | 1 | 46% | Close to threshold but still under-utilized |
| `t2.micro` | 2 | 6–27% | Jump hosts with very low regular usage |
| `t3.medium` | 1 | 28% | Low-traffic workload on mid-tier instance |

---

### 🟡 Balanced Instances (CPU 50–75%) — 3 Instances

These instances were performing adequately and were not flagged for right-sizing.

| Instance Family | CPU | Decision |
|----------------|-----|----------|
| `t2.small` | 73.9% | No change — within healthy range |
| `c6a.2xlarge` | 56.8% | No change — workload justified |
| `m6a.4xlarge` | 53.2% | No change — SQL workload, memory/IOPS matter more than CPU |

---

### 🟢 High Utilization Instances (CPU > 75%) — 6 Instances

These instances were healthy and required no optimization.

| Instance Family | CPU | Decision |
|----------------|-----|----------|
| `t3a.xlarge` | 98.8% | Do NOT downsize — at capacity |
| `t2.small` | 89.1% | No change needed |
| `t3a.small` | 97.3% | No change needed |
| `t3a.large` | 81.2% | No change needed |
| `t2.large` | 100% | Consider upgrading, not downsizing |

---

## Important Finding: Spot Instance False Positives

4× `c5a.xlarge` Spot instances were flagged by the optimization tool as "under-utilized" with apparent savings.

However, upon manual review:
- These were already on **Spot pricing** (significantly discounted)
- All recommended alternatives were **On-Demand** instances
- Switching would have **increased** cost, not reduced it

👉 This was a critical example of why tool output must always be validated manually.

---

## Data Considerations

- Analysis period: 1 full month
- Metric used: Maximum CPU utilization (CloudWatch)
- Consistent patterns across the month — not just short-term spikes
- Memory and IOPS data were considered separately for database workloads

👉 See [risk-assessment.md](./risk-assessment.md) for risk evaluation per instance group.
