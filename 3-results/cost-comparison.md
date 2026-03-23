# 📉 Cost Comparison

## Before vs After

*Exact cost figures withheld under NDA. Percentages and ratios are shared for educational reference.*

---

## Overall Fleet Summary

| Metric | Value |
|--------|-------|
| Total Instances | 29 |
| Under-utilized (before) | 20 (~69%) |
| Cost Reduction Identified | ~18% |
| Primary Method | Right-sizing + Compute Savings Plan |

---

## Savings by Category

### Right-Sizing Savings (Instance Downsizing)

| Change Made | Instances | Savings Per Instance |
|-------------|-----------|---------------------|
| xlarge → large (m6a family) | 4 | ~50% |
| xlarge → large (c6a family) | 3 | ~50% |
| large → medium (t3 family) | 2 | ~50–73% |
| medium → small (t3a family) | 3 | ~50% |
| large → medium (c6a → c6g) | 2 | ~54% |
| micro → nano (t2 family) | 2 | ~50% |

### Savings Plan Savings

- Applying a 1-year Compute Savings Plan to the steady-state fleet
- Expected savings: ~18% across the full On-Demand spend
- No instance changes required — pure pricing model optimization

---

## Utilization After Right-Sizing (Projected)

For all right-sized instances, projected post-change CPU utilization:

```
Before right-sizing:
  m6a.xlarge group:   11–17% CPU  (4 vCPU / 16GB — massively over-provisioned)

After right-sizing to m6a.large:
  m6a.large group:    22–34% CPU  (2 vCPU / 8GB — healthy utilization with buffer)
```

```
Before right-sizing:
  c6a.xlarge group:   15–24% CPU  (4 vCPU / 8GB)

After right-sizing to c6a.large:
  c6a.large group:    30–48% CPU  (2 vCPU / 4GB — healthy range)
```

---

## Cost Reduction Visualization

```
Before Optimization:
████████████████████████████████████████  100%  ($X/month)

After Right-Sizing Only:
████████████████████████████████░░░░░░░░   ~85%  (estimated)

After Right-Sizing + Savings Plan:
████████████████████████████████░░░░░░░░   ~82%  ($X × 0.818/month)
                                         ↑
                                    ~18% reduction
```

---

## ROI Analysis

| Input | Estimate |
|-------|----------|
| Analysis time | ~1 week |
| Implementation time | ~1–2 days (phased) |
| Monthly savings | ~18% of fleet cost |
| Annual savings | ~18% × 12 months |
| Implementation risk | Low (phased rollout with AMI backups) |

👉 For most cloud environments, a right-sizing exercise pays for itself within the first month of implementation.

---

## Important Note

The ~18% figure represents savings achievable with a Savings Plan applied to the **current** (pre-right-sizing) fleet.

If right-sizing changes are implemented first and then a Savings Plan is applied to the reduced baseline, the absolute monthly saving in dollars would be lower — but the percentage optimization from each lever would compound.

**Best practice:** Right-size first → observe new baseline → then purchase Savings Plan.
