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

The policy was narrowed the same day: each OS family uses a **stable, established release** that has a published CIS Benchmark and scanner content (OpenSCAP or the vendor's tool), not necessarily the newest release. Tools still use the latest stable version at install time.

Selected from the [preflight research](../evidence/phase-0-foundation/2026-11-15T1140-preflight-research.md):

- **Rocky Linux 9** (current point release, recorded in Phase 1). The CIS Benchmark exists for Rocky 9, and the SCAP Security Guide ships a Rocky 9 data stream (`ssg-rl9-ds.xml`).
- **Ubuntu Server 24.04 LTS.** The CIS Benchmark exists and Canonical's Ubuntu Security Guide audits `cis_level1_server`; it needs Ubuntu Pro, which is free for personal use.

Rocky Linux 10 and Ubuntu 26.04 are not used: their scanner content was not confirmed, and the design document's targets depend on a Level 1 scan that works. If a later phase needs a newer release, a new decision record supersedes this one.

Phase 1 still confirms, on real guests, that `oscap info` lists a Rocky 9 CIS Level 1 profile and that `usg` lists `cis_level1_server` on Ubuntu 24.04, before the fleet is built.
