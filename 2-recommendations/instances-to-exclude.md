# 🚫 Instances Excluded from Optimization

## Why Exclusions Matter

A good cost optimization analysis is not just about finding what to change — it's equally important to clearly document what should NOT be changed and why.

Incorrectly downsizing a production database or a high-traffic server can cause outages, data loss, or performance degradation. Every exclusion here has a specific, documented reason.

---

## Excluded Instances

### 1. SQL / Database Production Server (`m6a.4xlarge`)

**CPU:** ~53%  
**Status:** Balanced  
**Decision:** ❌ Do NOT right-size

**Reason:**  
CPU at 53% appears "Balanced" — but this is a production SQL/database workload. For database servers, CPU is not the primary bottleneck. What matters more is:
- **Memory** — databases cache data in RAM; reducing memory causes disk I/O
- **IOPS** — disk read/write performance
- **Network throughput** — for client connections and replication

Downsizing this instance without deep memory and IOPS analysis would be a significant risk to production data and query performance.

👉 **Lesson:** Always understand the workload type before acting on CPU data.

---

### 2. Windows Production Web Server (`t3a.xlarge`)

**CPU:** 98.8%  
**Status:** High  
**Decision:** ❌ Do NOT right-size (consider scaling out instead)

**Reason:**  
This instance is running at near-maximum capacity. Downsizing would immediately cause performance degradation. If anything, this instance may need to be upgraded or horizontally scaled (adding more instances behind a load balancer).

---

### 3. SQL Development Server (`t2.large`)

**CPU:** 100%  
**Status:** High  
**Decision:** ❌ Do NOT right-size — consider upgrading

**Reason:**  
Running at 100% CPU constantly is a red flag. This instance is under-powered, not over-powered. It should be investigated for upgrade, not downsize.

---

### 4. High-Utilization Small Instances (`t3a.small`, `t3a.large`, `t2.small`)

**CPU:** 81–97%  
**Status:** High  
**Decision:** ❌ No change needed

**Reason:**  
These instances are well-utilized. No right-sizing action is needed. They are performing their intended function efficiently.

---

### 5. Spot Instances — `c5a.xlarge` ×4

**CPU:** 14–37%  
**Status:** Under-utilized (per tool)  
**Decision:** ❌ Do NOT change — tool recommendation was incorrect

**Reason:**  
The optimization tool flagged these as "under-utilized" and suggested switching to On-Demand alternatives. However, these instances were already on **Spot pricing** — a significantly discounted model.

All recommended alternatives were On-Demand instances, which would have **cost more** than the current Spot price.

This was a false positive in the tool's output — the only correct action was to keep them as-is.

👉 **Key Lesson:** Always check the purchase model before acting on tool recommendations. A tool that doesn't account for existing Spot pricing can lead to cost increases disguised as "savings."

---

## Summary

| Instance | Reason for Exclusion |
|----------|---------------------|
| Production SQL server | Memory/IOPS-bound workload — CPU alone is misleading |
| Windows web server | 98.8% CPU — already at capacity |
| SQL dev server | 100% CPU — needs upgrade, not downsize |
| High-CPU small instances | Well-utilized — no action needed |
| 4× Spot instances | Already on discounted Spot pricing — tool gave false positive |
