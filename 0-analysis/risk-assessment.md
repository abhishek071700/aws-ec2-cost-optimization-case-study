# ⚠️ Risk Assessment

## Overview

Not every under-utilized instance is safe to downsize. Before making any recommendation, each instance was assessed for risk based on workload type, criticality, and usage patterns.

---

## Risk Classification

### 🟢 Low Risk — Safe to Right-Size

These instances showed consistent low CPU, non-critical workloads, and easy rollback options.

| Instance Group | Risk Level | Reason |
|----------------|-----------|--------|
| `m6a.xlarge` (multiple) | Low | Generic compute, low CPU, same-family downsize |
| `c6a.xlarge` (multiple) | Low | Compute-optimized but barely using compute |
| `c6a.large` (near-idle) | Low | Very low CPU, safe to move to smaller type |
| `t3a.medium` (multiple) | Low | Small workloads, easy to test and revert |
| `t3.medium` | Low | Low traffic, non-critical workload |
| `t2.micro` jump hosts | Low | Dev/test infrastructure, not production-critical |

---

### 🟡 Medium Risk — Change with Monitoring

These instances need careful monitoring post-change.

| Instance Group | Risk Level | Reason |
|----------------|-----------|--------|
| `t3.large` (PDF workload) | Medium | CPU at 46% — buffer is thin, monitor closely |
| `t2.micro` (Redis host) | Medium | In-memory workload — memory reduction needs careful testing |

---

### 🔴 High Risk — Do NOT Change

These instances should not be touched based on current data.

| Instance | Risk Level | Reason |
|----------|-----------|--------|
| `m6a.4xlarge` (SQL) | High | CPU at 53% but SQL workloads need memory + IOPS, not just CPU |
| `t3a.xlarge` (Windows webserver) | High | 98.8% CPU — already at capacity |
| `t2.large` (SQL dev) | High | 100% CPU — needs upgrade, not downsize |
| `t3a.small` (scraper) | High | 97.3% CPU — fully utilized |
| `c5a.xlarge` (Spot ×4) | High | Already on Spot — switching to alternatives increases cost |

---

## Risk Mitigation Strategy

For all approved changes, the following steps were recommended:

1. **Backup First** — Create an AMI snapshot before any instance type change
2. **Off-Peak Changes** — Apply changes during low-traffic windows
3. **One at a Time** — Change one instance per group first, monitor for 48–72 hours
4. **Rollback Ready** — Keep AMI backup available for at least 7 days post-change
5. **Alert Setup** — Configure CloudWatch alarms for CPU > 80% post-change

---

## Key Insight

👉 The SQL workload on `m6a.4xlarge` appeared "Balanced" at 53% CPU — but for database workloads, memory pressure and IOPS are more important signals than CPU alone. This instance was correctly excluded from right-sizing recommendations.

This was one of the most important lessons from this project: **always understand the workload before acting on raw metrics.**
