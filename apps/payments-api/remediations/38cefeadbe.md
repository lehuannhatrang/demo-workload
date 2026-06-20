# Remediation proposal

- Issue key: `38cefeadbe`
- Risk: **medium**
- Source: `argocd`
- Target: `apps/payments-api` in `lehuannhatrang/demo-workload`

## Summary

Proposed remediation for payments-api high memory

## Root cause hypothesis

The single most likely root cause is a memory leak in the payments-api application due to unbounded memory accumulation, likely from improper handling of requests, connections, or cached data within the application process. Given that the pod is consistently consuming ~85% of its 64Mi memory limit (approximately 54.4 MiB), which is high for such a constrained environment, and considering the small memory footprint typical of modern microservices, the application is likely retaining memory that is not being garbage collected—possibly due to stuck goroutines, unclosed database connections, or unbounded slices/maps growing over time. This pattern suggests a software defect rather than a legitimate increase in load or data throughput.

## Validation

all checks passed

- PASS `hypothesis_present` — RCA produced a hypothesis
- PASS `context_present` — context available
