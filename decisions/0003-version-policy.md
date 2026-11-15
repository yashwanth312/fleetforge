# 0003: Use the latest stable version of each component

Date: 2026-11-15
Status: Accepted

## Context

Operating system and tool versions change often, and the project is built and measured on specific dates. The design document names RHEL/Rocky 9 and Ubuntu 22.04. Ubuntu 22.04 reaches the end of standard support in 2027 and a newer LTS has been released. Pinning versions from memory risks stale or wrong values.

## Options

1. **Latest stable release** of each OS and tool when it is installed, looked up at that time.
2. **Versions named in the design document** (Rocky/RHEL 9, Ubuntu 22.04) with current tool versions.
3. **Pin everything** to one dated set of versions from the start.

## Decision

Use the latest stable release of every tool, and the latest release of each OS family that has CIS Level 1 content available in OpenSCAP or the vendor's compliance tooling. Record the version, the date and the source in the evidence entry for the phase that installs it.

## Why

- The repository describes a current system, not one that is about to leave support.
- Looking versions up at install time avoids unverified numbers.

Trade-offs accepted: the newest OS release may not have a CIS Level 1 profile yet. Phase 1 checks this before any VM is built. If the newest release of an OS family has no profile, the fleet uses the newest release that has one, and an `Update` section records which release and why. Newer releases may also need more RAM or newer CPU features than the lab provides; Phase 1 measures idle memory per OS before the fleet size is fixed.

## Update 2026-11-15

Preliminary research ([evidence entry](../evidence/phase-0-foundation/2026-11-15T1140-preflight-research.md)) found:

- Rocky Linux: 10.2 is the newest release confirmed. The RHEL 10 SCAP content lists a draft CIS Level 1 Server profile; Rocky-specific content is not confirmed. Rocky Linux 10 requires the x86-64-v3 CPU level, so guests must see it.
- Ubuntu: 26.04 LTS is the newest release. Automated scanner content for 26.04 was not confirmed; for 24.04 it was (Canonical's Ubuntu Security Guide profiles). The Ubuntu Security Guide requires Ubuntu Pro, which is free for personal use on up to five machines.

Phase 1 confirms scanner content on real guests before the fleet is built. If Ubuntu 26.04 has no CIS Level 1 profile in a scanner, the Ubuntu nodes use 24.04 and this record is updated again.
