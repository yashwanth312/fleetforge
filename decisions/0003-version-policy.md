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
