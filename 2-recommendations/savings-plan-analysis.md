# 💰 Savings Plan Analysis

## Overview

Beyond right-sizing, AWS Savings Plans offer a significant cost reduction simply by committing to a consistent spend level — without needing to change instance types.

For this fleet, adopting a Compute Savings Plan was identified as the fastest path to ~18% savings across all 29 instances.

---

## How Savings Plans Work

```
You commit to: spend $X/hour for 1 or 3 years
AWS gives you: discounted rates on all compute usage up to that amount
Anything above: billed at normal On-Demand rates
```

The key insight: **you don't commit to specific instances** — you commit to a spend level. This makes Savings Plans flexible even as your fleet changes.

---

## Savings Plan Types Evaluated

### Option 1: Compute Savings Plan (Recommended)
- Applies to any EC2 instance, Lambda, and Fargate
- Works across regions, instance families, OS types
- No lock-in to specific instance type
- Savings: ~20–40% off On-Demand

### Option 2: EC2 Instance Savings Plan
- Applies to a specific instance family in a specific region
- Higher savings (up to 40–60%) but less flexible
- Best if fleet composition is unlikely to change
- Savings: ~30–60% off On-Demand

### Option 3: 1-Year vs 3-Year Commitment

| Commitment | Flexibility | Savings Range |
|------------|-------------|---------------|
| 1-Year | High | 20–40% |
| 3-Year | Low | 40–60% |

**Recommendation:** Start with a 1-year Compute Savings Plan. After validating right-sizing changes, reassess for a 3-year commitment on the stable core workloads.

---

## Application to This Fleet

The ~18% overall savings figure was calculated assuming a Compute Savings Plan covering the steady-state On-Demand workloads in the fleet.

Key workloads recommended for Savings Plan coverage:
- Production servers running 24/7 (web servers, application servers)
- Database instances with consistent uptime requirements
- Any instance that has been running continuously for 3+ months

Workloads NOT recommended for Savings Plan:
- Development/test instances (consider Spot instead)
- Instances scheduled for right-sizing soon (wait until after change)
- Any instance with uncertain future (might be terminated)

---

## What I Learned About Savings Plans

1. **Commit to steady-state only.** Don't include dev/test or variable workloads in your commitment calculation.

2. **Right-size first, then commit.** If you buy a Savings Plan before right-sizing, you'll be committing to a higher spend than necessary.

3. **Partial commitment is fine.** You don't need to cover 100% of your fleet. Covering 60–70% of steady workloads with a Savings Plan while keeping flexibility for the rest is a solid strategy.

4. **Use AWS Cost Explorer.** The Savings Plans recommendation tool in Cost Explorer shows exactly how much to commit based on your usage history.

---

## Recommended Next Step

```
1. Complete all right-sizing changes first
2. Wait 2–4 weeks to observe new baseline cost
3. Use AWS Cost Explorer → Savings Plans → Recommendations
4. Purchase 1-year Compute Savings Plan based on new baseline
5. Review again in 6 months
```
