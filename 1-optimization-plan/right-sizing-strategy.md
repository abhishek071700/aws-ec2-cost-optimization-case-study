# 📐 Right-Sizing Strategy

## What is Right-Sizing?

Right-sizing means matching the EC2 instance type to the actual resource needs of the workload — not too big, not too small.

In this project, the fleet was consistently **over-provisioned** — instances were larger than the workloads needed, resulting in paying for CPU and memory that was rarely or never used.

---

## Decision Framework

For each under-utilized instance, three questions were asked:

### 1. Is the CPU the right signal here?
- For web/compute workloads → Yes, CPU is the primary signal
- For database/SQL workloads → No, check memory and IOPS too
- For burstable instances (`t` family) → Check burst credit behavior as well

### 2. What is the minimum safe size?
- Identify peak CPU (not just average)
- Target: post-downsize CPU should stay under 70% at peak
- Maintain a safety buffer of at least 30% headroom

### 3. Same family or different family?
- **Same family, smaller size** → Lowest risk (e.g., `m6a.xlarge` → `m6a.large`)
- **Different family, same size** → Medium risk (e.g., `c6a.xlarge` → `c6g.xlarge`)
- **Different family, smaller size** → Higher risk, more testing needed

---

## Right-Sizing Decisions Made

### `m6a.xlarge` → `m6a.large`
- Reduction: 4 vCPU / 16GB → 2 vCPU / 8GB
- Rationale: CPU consistently at 11–17%, half the resources more than sufficient
- Risk: Low — same instance family, predictable workload

### `c6a.xlarge` → `c6a.large`
- Reduction: 4 vCPU / 8GB → 2 vCPU / 4GB
- Rationale: Compute instances at 15–24% — barely using compute capacity
- Risk: Low — same family, clean downsize path

### `c6a.large` → `c6g.medium`
- Reduction: 2 vCPU / 4GB → 1 vCPU / 2GB
- Rationale: Near-idle at 3–4% CPU — even a `medium` provides significant headroom
- Risk: Low — non-critical workload

### `t3.large` → `t3a.medium`
- Reduction: 2 vCPU / 8GB → 2 vCPU / 4GB
- Rationale: PDF processing workload at 46% CPU — `t3a.medium` provides enough capacity
- Risk: Medium — monitor for memory pressure post-change

### `t3a.medium` → `t3a.small`
- Reduction: 2 vCPU / 4GB → 2 vCPU / 2GB
- Rationale: All three at 11–14% CPU — `t3a.small` provides same CPU with less memory
- Risk: Low — small instances, easy to revert

### `t3.medium` → `t3a.small`
- Reduction: 2 vCPU / 4GB → 2 vCPU / 2GB
- Rationale: Low traffic workload at 28% CPU
- Risk: Low

### `t2.micro` → `t2.nano`
- Reduction: 1 vCPU / 1GB → 1 vCPU / 0.5GB
- Rationale: Jump hosts with very low regular usage (6–27% CPU)
- Risk: Low — dev/test infrastructure

---

## What Was NOT Right-Sized

| Instance | Reason |
|----------|--------|
| `m6a.4xlarge` (SQL) | Memory-intensive workload — CPU metric alone is misleading |
| `t3a.xlarge` (Windows) | 98.8% CPU — already needs full capacity |
| `t2.large` (SQL dev) | 100% CPU — consider upgrade instead |
| `t3a.small`, `t3a.large` (high CPU) | Fully utilized — no action needed |
| `c5a.xlarge` ×4 (Spot) | Already on Spot pricing — alternatives cost more |

---

## Instance Family Naming — What I Learned

Understanding EC2 naming was key to making good decisions:

```
m  6  a  .  x  large
│  │  │     │  └── Size: nano/micro/small/medium/large/xlarge/2xlarge...
│  │  │     └───── Separator
│  │  └─────────── Processor: (blank)=Intel, a=AMD, g=Graviton(ARM)
│  └───────────── Generation: higher = newer & cheaper
└──────────────── Family: t=burstable, m=general, c=compute, r=memory
```

👉 Key takeaway: `m6a` is cheaper than `m5a` for the same workload, and AMD (`a`) instances typically cost less than Intel equivalents.
