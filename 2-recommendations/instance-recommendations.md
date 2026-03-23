# ✅ Instance Recommendations

## Summary

Based on 1-month CPU utilization analysis, the following right-sizing recommendations were made.

*Instance names and exact costs withheld under NDA. Instance families and actions are documented for educational reference.*

---

## Group 1: General Purpose — `m6a.xlarge` Family

**Count:** 4 instances  
**CPU Range:** 11–17%  
**Recommendation:** Downsize to `m6a.large`

| Current | Recommended | vCPU Change | RAM Change | Savings |
|---------|-------------|-------------|-----------|---------|
| m6a.xlarge | m6a.large | 4 → 2 vCPU | 16GB → 8GB | ~50% per instance |

**Why:** All 4 instances showed consistent CPU below 17%. Half the resources are more than sufficient. Same instance family ensures minimal migration friction.

**Purchase Option Evaluated:**
- On-Demand → On-Demand (immediate ~50% saving)
- On-Demand → 1yr Compute Savings Plan (additional ~30% on top)

---

## Group 2: Compute Optimized — `c6a.xlarge` Family

**Count:** 3 instances  
**CPU Range:** 15–24%  
**Recommendation:** Downsize to `c6a.large`

| Current | Recommended | vCPU Change | RAM Change | Savings |
|---------|-------------|-------------|-----------|---------|
| c6a.xlarge | c6a.large | 4 → 2 vCPU | 8GB → 4GB | ~50% per instance |

**Why:** Compute-optimized instances with very low compute usage. Halving the size keeps the same instance family and architecture.

---

## Group 3: Compute Optimized — `c6a.large` (Near-Idle)

**Count:** 2 instances  
**CPU Range:** 3–4%  
**Recommendation:** Downsize to `c6g.medium`

| Current | Recommended | vCPU Change | RAM Change | Savings |
|---------|-------------|-------------|-----------|---------|
| c6a.large | c6g.medium | 2 → 1 vCPU | 4GB → 2GB | ~54% per instance |

**Why:** Near-idle instances at 3–4% CPU. Even a `medium` provides massive headroom. Note: `c6g` is Graviton (ARM) — verify workload compatibility before switching.

---

## Group 4: General Purpose Burstable — `t3a.medium` Family

**Count:** 3 instances  
**CPU Range:** 11–14%  
**Recommendation:** Downsize to `t3a.small`

| Current | Recommended | vCPU Change | RAM Change | Savings |
|---------|-------------|-------------|-----------|---------|
| t3a.medium | t3a.small | 2 → 2 vCPU | 4GB → 2GB | ~50% per instance |

**Why:** Same CPU count, half the memory. CPU usage is very low and workloads showed no memory pressure indicators.

---

## Group 5: Storage/Processing — `t3.large`

**Count:** 1 instance  
**CPU:** 46%  
**Recommendation:** Downsize to `t3a.medium`

| Current | Recommended | vCPU Change | RAM Change | Savings |
|---------|-------------|-------------|-----------|---------|
| t3.large | t3a.medium | 2 → 2 vCPU | 8GB → 4GB | ~73% |

**Why:** CPU is at 46% — just under the 50% threshold. `t3a.medium` provides same CPU with less memory. Monitor closely post-change for memory pressure.

**Risk:** Medium — validate memory usage before committing to this change.

---

## Group 6: Burstable Small — `t3.medium`

**Count:** 1 instance  
**CPU:** 28%  
**Recommendation:** Downsize to `t3a.small`

| Current | Recommended | vCPU Change | RAM Change | Savings |
|---------|-------------|-------------|-----------|---------|
| t3.medium | t3a.small | 2 → 2 vCPU | 4GB → 2GB | ~73% |

**Why:** Low-traffic workload well within `t3a.small` capacity.

---

## Group 7: Jump Hosts — `t2.micro`

**Count:** 2 instances  
**CPU Range:** 6–27%  
**Recommendation:** Downsize to `t2.nano` OR switch to Spot

| Current | Recommended | Savings |
|---------|-------------|---------|
| t2.micro | t2.nano (On-Demand) | ~50% |
| t2.micro | t2.micro (Spot) | ~64% |

**Why:** Jump hosts / bastion servers used for SSH access. Very low regular CPU usage. Even a `nano` provides sufficient capacity for this use case.

---

## ⚠️ Spot Instances — No Change Recommended

**Count:** 4 instances (`c5a.xlarge`)  
**CPU Range:** 14–37%  
**Recommendation:** Keep as-is

**Why:** These were already on Spot pricing — the optimization tool flagged them as "saveable" by suggesting On-Demand alternatives. This was incorrect — switching would have increased cost. These instances were excluded from all right-sizing actions.

---

## Summary Table

| Instance Group | Action | Estimated Savings |
|----------------|--------|------------------|
| 4× m6a.xlarge | → m6a.large | ~50% each |
| 3× c6a.xlarge | → c6a.large | ~50% each |
| 1× t3.large | → t3a.medium | ~73% |
| 2× c6a.large | → c6g.medium | ~54% each |
| 3× t3a.medium | → t3a.small | ~50% each |
| 1× t3.medium | → t3a.small | ~73% |
| 2× t2.micro | → t2.nano | ~50% each |
| 4× c5a.xlarge (Spot) | No change | Already optimized |

*Exact dollar amounts withheld under NDA.*
