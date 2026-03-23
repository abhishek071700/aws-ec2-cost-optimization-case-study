# 📅 Implementation Timeline

## Approach

A phased rollout was recommended to minimize risk and allow monitoring between changes.

---

## Phase 1 — Quick Wins (Week 1–2)

**Target:** Low-risk, high-savings instances

✅ **Actions:**
- Right-size the largest under-utilized compute instances (same-family downsizing)
- Convert steady On-Demand workloads to Savings Plan
- Set up CloudWatch CPU alarms on all modified instances (alert at 80%)

**Expected Impact:** ~50% of total savings realized

---

## Phase 2 — Medium Priority (Week 3–4)

**Target:** Remaining under-utilized instances

✅ **Actions:**
- Right-size remaining under-utilized instances (burstable + smaller families)
- Move non-critical/dev workloads to Spot where applicable
- Review and validate Phase 1 changes — check CloudWatch metrics post-change

**Expected Impact:** ~80% of total savings realized

---

## Phase 3 — Monitoring & Stabilization (Week 5–6)

**Target:** Validate all changes and optimize further

✅ **Actions:**
- Review CloudWatch metrics on all modified instances
- Check for any performance degradation or unexpected CPU spikes
- Revert any instances showing consistent CPU > 80% post-change
- Document final results and savings

**Expected Impact:** Full ~18% savings confirmed and stabilized

---

## Phase 4 — Automation & Continuous Optimization (Ongoing)

**Target:** Build a repeatable process

- Set up monthly cost reviews using AWS Cost Explorer
- Configure CloudWatch alarms for instances going consistently under 10% CPU
- Evaluate Graviton (`g`-family) instances as a next optimization step
- Explore auto-scaling for workloads with variable traffic patterns

---

## Safe Change Procedure (Per Instance)

```
Step 1: Create AMI backup of the instance
Step 2: Note current CloudWatch metrics (baseline)
Step 3: Stop the instance
Step 4: Change instance type (Actions → Instance Settings → Change Instance Type)
Step 5: Start the instance
Step 6: Monitor CloudWatch CPU for 48–72 hours
Step 7: If CPU consistently > 80%, revert using AMI backup
Step 8: If stable, mark as complete and move to next instance
```

---

## Rollback Strategy

If any instance shows degraded performance after right-sizing:

1. Stop the instance
2. Restore from AMI backup (or change type back manually)
3. Restart and verify normal operation
4. Document the case — some workloads have hidden memory/IOPS requirements not visible in CPU metrics

---

## Key Timeline Principle

👉 **One instance at a time per group.** Don't change all 4× `m6a.xlarge` instances simultaneously. Change one, monitor for 48 hours, then proceed to the next. This limits blast radius if something goes wrong.
