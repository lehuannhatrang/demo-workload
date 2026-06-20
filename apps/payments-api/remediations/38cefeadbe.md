# Remediation proposal

- Issue key: `38cefeadbe`
- Risk: **medium**
- Source: `argocd`
- Target: `apps/payments-api` in `lehuannhatrang/demo-workload`

## Summary

Proposed remediation for payments-api high memory

## Root cause hypothesis

**Root Cause**: The `payments-api` pod is running the `stress` tool configured to allocate 50MB of memory (`--vm-bytes 50M`), which consumes approximately 85% of its 64Mi (≈67MB) memory limit, leaving insufficient headroom and triggering the high memory alert due to operating near the OOM threshold.

## Validation

all checks passed

- PASS `hypothesis_present` — RCA produced a hypothesis
- PASS `context_present` — context available
