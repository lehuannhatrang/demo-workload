# Remediation proposal

- Issue key: `38cefeadbe`
- Risk: **medium**
- Source: `argocd`
- Target: `apps/payments-api` in `lehuannhatrang/demo-workload`

## Summary

Proposed remediation for payments-api high memory

## Root cause hypothesis

The single most likely root cause is a memory leak in the payments-api application due to unbounded accumulation of in-memory data (e.g., caches, buffers, or pending requests) within the container, exacerbated by the low memory limit of 64Mi which is insufficient for the application’s actual workload.

Evidence:
- The alert indicates the pod is consuming ~85% of its 64Mi memory limit, which is approximately 54.4Mi, already nearing the threshold for OOMKilling.
- 64Mi is an unusually low memory limit for a production API service, especially one handling payments, suggesting the limit may have been set too conservatively or without accounting for baseline application overhead and request load.
- No autoscaling or recent deployment is mentioned, implying this is a steady-state resource exhaustion issue rather than a spike.
- Without memory headroom, even normal operation (e.g., GC cycles not running in time, transient spikes in request volume, or connection pooling) can push the app to OOM.

Conclusion: The root cause is likely insufficient memory allocation combined with poor memory management in the app, leading to sustained high memory use and risk of OOMKill. Immediate fix: increase memory limit to at least 128Mi and profile the app for leaks. Long-term: add memory monitoring and set up proper HPA/VPA.

## Validation

all checks passed

- PASS `hypothesis_present` — RCA produced a hypothesis
- PASS `context_present` — context available
