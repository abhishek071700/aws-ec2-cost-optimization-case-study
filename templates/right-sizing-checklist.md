# ✅ Right-Sizing Safety Checklist

Run through this checklist before changing any EC2 instance type.

---

## Pre-Change Checklist

### 1. Analysis
- [ ] CPU utilization reviewed for minimum 30 days
- [ ] Peak CPU identified (not just average)
- [ ] Workload type confirmed (web / compute / database / dev)
- [ ] Memory requirements checked (if possible via CloudWatch Agent)
- [ ] Network I/O reviewed for network-heavy workloads
- [ ] Disk IOPS reviewed for database workloads

### 2. Recommendation Validation
- [ ] Tool recommendation cross-checked manually
- [ ] Confirmed instance is NOT a database / SQL server
- [ ] Confirmed CPU is consistently below 50% (not just average)
- [ ] Confirmed current purchase model (On-Demand? Spot? Reserved?)
- [ ] Verified alternative is not more expensive than current pricing

### 3. Backup
- [ ] AMI snapshot created
- [ ] Snapshot tagged with date and original instance type
- [ ] Snapshot retention set (minimum 7 days)

### 4. Change Window
- [ ] Low-traffic window identified
- [ ] Team / stakeholders notified
- [ ] Rollback procedure documented and ready

---

## Change Procedure

```bash
# 1. Stop the instance
#    AWS Console → EC2 → Instances → Select instance → Instance State → Stop

# 2. Change instance type
#    Actions → Instance Settings → Change Instance Type → Select new type → Apply

# 3. Start the instance
#    Instance State → Start

# 4. Verify instance is healthy
#    Check: Instance State = Running
#    Check: Status Checks = 2/2 passed
#    Check: Application responding normally
```

---

## Post-Change Monitoring (48–72 hours)

- [ ] CPU utilization within expected range (target: < 70% at peak)
- [ ] No application errors or timeouts reported
- [ ] Memory pressure normal (if monitored)
- [ ] Response times unchanged
- [ ] No alerts triggered on dependent services

---

## Rollback Procedure

If performance issues are observed:

```bash
# 1. Stop the instance
# 2. Change instance type back to original
# 3. Start the instance
# 4. Verify recovery
# 5. Document: what was the issue? Memory? CPU? Other?
```

---

## Red Flags — Stop and Review

If any of the following occur after right-sizing, pause and investigate before continuing with other instances:

- 🚨 CPU consistently above 80% after change
- 🚨 Application errors or timeouts
- 🚨 Memory-related errors in application logs
- 🚨 Disk I/O saturation
- 🚨 Network throughput degradation

---

## Documentation

After completing each change, record:

| Field | Value |
|-------|-------|
| Instance (anonymized) | |
| Original Type | |
| New Type | |
| Change Date | |
| Post-Change CPU (avg) | |
| Post-Change CPU (max) | |
| Status | Stable / Reverted |
| Notes | |
