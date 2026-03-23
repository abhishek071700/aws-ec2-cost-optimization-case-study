# 📋 EC2 Cost Analysis Template

Use this template to perform a structured EC2 cost analysis for any AWS account.

---

## Step 1: Environment Overview

Fill in before starting analysis:

| Field | Value |
|-------|-------|
| AWS Account (anonymized) | |
| Region(s) | |
| Analysis Date | |
| Analysis Period | |
| Total EC2 Instances | |
| Total Monthly EC2 Spend | |

---

## Step 2: Fleet Inventory

Document all running instances:

| # | Instance Type | vCPU | RAM | Purchase Model | Status |
|---|--------------|------|-----|----------------|--------|
| 1 | | | | On-Demand / Spot | Running / Stopped |
| 2 | | | | | |
| ... | | | | | |

---

## Step 3: CPU Utilization Collection

For each instance, collect 30-day CloudWatch data:

| Instance | Type | Avg CPU | Max CPU | Classification |
|----------|------|---------|---------|----------------|
| | | | | Under-utilized / Balanced / High |

**Classification Guide:**
- 🔴 Under-utilized: Max CPU < 50%
- 🟡 Balanced: CPU 50–75%
- 🟢 High: CPU > 75%

---

## Step 4: Workload Type Assessment

For each under-utilized instance, assess workload:

| Instance | Workload Type | Memory Sensitive? | IOPS Sensitive? | Safe to Downsize? |
|----------|--------------|-------------------|-----------------|-------------------|
| | Web server | No | No | ✅ Yes |
| | SQL database | Yes | Yes | ⚠️ Check memory |
| | Dev/test | No | No | ✅ Yes |
| | Batch processing | No | No | ✅ Yes |

---

## Step 5: Right-Sizing Recommendations

For each approved downsize:

| Current Instance | Recommended | vCPU Change | RAM Change | Est. Saving | Risk |
|-----------------|-------------|-------------|-----------|-------------|------|
| | | | | | Low / Med / High |

---

## Step 6: Purchasing Plan Evaluation

| Instance Group | Current Model | Recommended Model | Est. Saving |
|----------------|--------------|-------------------|-------------|
| Production servers | On-Demand | Compute Savings Plan 1yr | ~30% |
| Dev/test | On-Demand | Spot | ~60–80% |
| Variable workloads | On-Demand | On-Demand (keep) | — |

---

## Step 7: Risk Assessment

| Recommended Change | Risk Level | Mitigation |
|--------------------|-----------|------------|
| | Low / Med / High | AMI backup, monitor 48hr |

---

## Step 8: Implementation Order

List changes in order of: lowest risk first, highest savings first.

1. 
2. 
3. 
...

---

## Step 9: Results Tracking

| Change | Date Applied | Pre-Change CPU | Post-Change CPU | Status |
|--------|-------------|----------------|-----------------|--------|
| | | | | ✅ Stable / ⚠️ Reverted |

---

## Step 10: Total Savings Summary

| Category | Monthly Saving | Annual Saving |
|----------|---------------|---------------|
| Right-sizing | | |
| Savings Plan | | |
| **Total** | | |

---

## Notes

Use this space to document anything unusual, workload-specific concerns, or observations that don't fit the tables above.
