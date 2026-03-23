# 🧠 Lessons Learned

## Overview

This project was my first hands-on experience with real AWS infrastructure cost analysis. Here are the most important things I learned — beyond what any tutorial or certification covers.

---

## Technical Lessons

### 1. CPU is the Starting Point, Not the Whole Story

CPU utilization is the most accessible metric and a great starting point for identifying over-provisioned instances. But it's not sufficient on its own.

- **Database workloads** may show moderate CPU but be heavily memory or IOPS-bound
- **Burstable instances** (`t` family) use CPU credits — a low average doesn't mean low peak
- **Network-heavy workloads** may need specific instance types regardless of CPU

👉 Always ask: *what does this workload actually need — CPU, memory, network, or storage?*

---

### 2. Tools Are a Starting Point, Not the Final Answer

The cost optimization tool I used did an excellent job of identifying under-utilized instances and suggesting alternatives. But it had one significant blind spot:

It flagged **Spot instances** as "saveable" by recommending On-Demand alternatives — without accounting for the fact that Spot pricing was already dramatically cheaper.

Acting on that recommendation would have **increased** cost by switching from Spot to On-Demand.

👉 Always validate tool output manually before making changes.

---

### 3. AWS Instance Naming Convention Unlocks Better Decisions

Before this project, instance names like `m6a.xlarge` and `c6g.large` were confusing. Now they make complete sense:

```
[Family][Generation][Processor].[Size]

m = General Purpose        c = Compute       r = Memory        t = Burstable
6 = 6th generation         (higher = newer, cheaper)
a = AMD processor          g = Graviton (ARM)    (blank) = Intel
```

Knowing that `m6a` is cheaper than `m5a` for the same workload, or that Graviton (`g`) instances often offer the best price-performance ratio, directly informed several recommendations.

---

### 4. Over-Provisioning is the Default, Not the Exception

In this fleet, **69% of instances** were under-utilized. This is not unusual.

Cloud environments tend to get over-provisioned over time because:
- Teams provision large instances "just in case"
- Workloads change but instance sizes don't follow
- No one is actively monitoring cost vs utilization
- There's no regular review process

👉 Cost optimization needs to be a continuous process, not a one-time exercise.

---

### 5. Savings Plans Are Underutilized

Many organizations run their entire fleet On-Demand and never adopt Savings Plans — even for workloads that have been running continuously for years.

A simple 1-year Compute Savings Plan can save 20–40% with zero infrastructure changes. It requires a commitment, but for stable production workloads that have been running for months, the risk is minimal.

---

### 6. Right-Sizing Has a Safety Formula

For every downsize decision, I developed a simple mental check:

```
✅ Safe to downsize if:
   - CPU consistently below 30% over 1 month
   - Workload is not memory or IOPS-bound
   - Target instance is same family (lowest migration risk)
   - AMI backup created before change
   - Monitoring alert set at 80% CPU post-change

⚠️ Be careful if:
   - CPU is between 30–50% (some headroom but less buffer)
   - Workload type is unclear
   - Memory reduction is involved

❌ Do not downsize if:
   - CPU is above 50%
   - Workload is a database or SQL server
   - Instance is running at or near 100%
```

---

## Professional Lessons

### Presenting Technical Findings to a Team

Walking through a 29-instance analysis with a team taught me how to communicate technical findings clearly:

- **Lead with impact** — total savings potential, not instance names
- **Group similar items** — don't go instance by instance, group by type
- **Flag risk clearly** — for every recommendation, note what could go wrong
- **Have a rollback answer ready** — teams need to know changes are reversible

### Asking the Right Questions

Before this project, I would have acted on the tool output directly. After completing the analysis, I learned to always ask:

1. What type of workload is this?
2. What purchase model is it currently on?
3. What happens if this instance restarts unexpectedly?
4. Has this instance ever spiked above the current average?

---

## What I Would Do Differently

1. **Collect memory metrics first.** CloudWatch doesn't track memory by default — you need the CloudWatch Agent. I would set this up before any analysis to get a more complete picture.

2. **Check network and disk I/O too.** For some workloads, these are more important than CPU.

3. **Right-size first, then buy Savings Plan.** I initially evaluated both together. The correct order is to implement right-sizing, stabilize, then commit to a Savings Plan based on the new baseline.

4. **Automate the monitoring.** Post-change monitoring was manual. CloudWatch alarms and AWS Cost Anomaly Detection should be set up from the start.
