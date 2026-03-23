# 💳 Purchasing Plan Evaluation

## The Three Options

AWS offers multiple ways to pay for EC2 instances. Choosing the right model can dramatically reduce cost without changing anything about the instance itself.

---

## Comparison Overview

| Feature | On-Demand | Spot | Savings Plan |
|---------|-----------|------|-------------|
| **Cost** | Full price | 60–90% off | 20–60% off |
| **Availability** | Always | Can be interrupted | Always |
| **Commitment** | None | None | 1 or 3 years |
| **Best for** | Unpredictable workloads | Fault-tolerant / batch jobs | Steady, predictable workloads |
| **Interruption Risk** | None | Yes (2-min warning) | None |
| **Flexibility** | Full | High | Moderate |

---

## Savings Plan Types

### Compute Savings Plan
- Applies to any EC2 instance regardless of region, family, OS, or size
- Most flexible — works across instance changes
- Typically offers 20–40% savings

### EC2 Instance Savings Plan
- Applies to a specific instance family in a specific region
- Less flexible but higher savings (up to 40–60%)
- Best when you're committed to a specific instance type

---

## Recommendations Made

### For Steady, Predictable Workloads → Compute Savings Plan (1yr)
- Production servers running 24/7
- Workloads with consistent CPU patterns
- Expected savings: 30–40% vs On-Demand

### For Fault-Tolerant / Non-Critical Workloads → Spot
- Dev/test environments
- Batch processing jobs
- Web scrapers, data pipelines
- Expected savings: 60–90% vs On-Demand

### For Existing Spot Instances → Keep as Spot
- The 4× `c5a.xlarge` instances were already on Spot pricing
- Optimization tool incorrectly recommended switching to On-Demand alternatives
- Manual review confirmed: keeping Spot was the correct decision

---

## What I Learned

### On-Demand is Expensive But Necessary
Most of the fleet was running On-Demand with no Savings Plan. This is the most common cause of overspending in cloud environments — it's the default but rarely the cheapest option for stable workloads.

### Spot Is Not Just for Big Data
Spot instances are often associated with large-scale batch jobs. But they're equally valuable for:
- Jump hosts / bastion servers (can tolerate brief interruptions)
- Development environments (no 24/7 uptime needed)
- Internal tools with flexible availability requirements

### The Tool Got It Wrong
The cost optimization tool flagged Spot instances as "under-utilized" and suggested switching to On-Demand alternatives. The apparent "savings" were actually negative — switching would have cost more.

👉 This reinforced a key lesson: **always validate tool recommendations manually before acting.**

---

## Savings Plan Calculation Approach

For the overall fleet:
1. Identify baseline On-Demand spend for steady workloads
2. Determine commitment amount (hourly rate)
3. Compare 1-year vs 3-year commitment savings
4. Factor in flexibility needs before committing

👉 Even a partial Savings Plan commitment (covering 60–70% of steady workload) can unlock significant savings while maintaining flexibility for variable capacity.
