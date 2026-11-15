# 0004: Compliance check as a patch-run gate

Date: 2026-11-15
Status: Proposed

## Context

The design halts a patch run when a batch fails its health check. Health checks answer whether the service still works. OpenSCAP answers whether the host is still compliant. The design measures compliance before and after, but does not say whether a compliance regression should also halt a run. A patch can overwrite a hardened setting without breaking any service, and a scan takes minutes per host, so scanning every batch slows the rollout.

## Options

1. **Health check only gates the run.** Scans run before and after the whole run and are reported (the design document's behavior).
2. **Scan the canary and halt on any regression.** A regression is a rule that passed before the patch and fails after it.
3. **Scan every batch and halt on any regression.**

## Decision

Option 2 is proposed: the canary is scanned, and the run halts if any rule regressed. Not yet accepted.

## Why

- The canary is the host where an overwritten setting is cheapest to catch.
- The comparison is per rule, not a score, because a score hides which setting changed.
- Option 3 costs a scan per batch for little extra signal once the canary is clean.

Trade-off accepted: a regression that appears only on later batches (for example because of a different OS family) is reported by the final scan but does not halt the run.
