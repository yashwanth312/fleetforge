# Preflight research: platform, CIS content and tool versions

Date: 2026-11-15 11:40 (America/Chicago, UTC-06:00)

## Host and versions

Windows 11 Home laptop (see the lab notes in the README). VirtualBox 7.2.18 is installed. Nothing in this entry was run on the laptop; every row is desk research from the sources listed, with the date above. Items marked "not confirmed" are tested in Phase 1.

## Method

Web searches of vendor documentation, release announcements and release trackers. Where two sources disagreed, the newer or more official one is used and the disagreement is noted.

## Results

| Item | Finding | Source | Status |
|---|---|---|---|
| Rocky Linux release | 10.2 is the newest release announced | [Rocky Linux 10.2 announcement](https://rockylinux.org/news/rocky-linux-10-2-ga-release) | Confirmed |
| Rocky 10 scanner content | The RHEL 10 SCAP Security Guide lists CIS Level 1 Server and Level 2 Server profiles marked DRAFT. Whether Rocky's own package ships a Rocky 10 data stream with the same profile is not documented in the pages found | [RHEL 10 supported profiles](https://docs.redhat.com/ko/documentation/red_hat_enterprise_linux/10/html/security_hardening/scap-security-guide-profiles-supported-in-rhel-10) | Not confirmed |
| Ubuntu 24.04 scanner content | Canonical's Ubuntu Security Guide provides CIS Level 1 and 2 profiles; audit with `usg audit cis_level1_server`. Needs an Ubuntu Pro subscription | [Canonical announcement](https://www.ubuntu.com/blog/hardening-automation-for-cis-benchmarks-now-available-for-ubuntu-24-04-lts) | Confirmed |
| Ubuntu 26.04 scanner content | The Canonical pages found describe CIS profiles in general terms and name no release newer than 24.04 | [USG documentation](https://documentation.ubuntu.com/security/compliance/usg/cis-benchmarks/) | Not confirmed |
| Packer | 1.15.4, released 2026-06-03 (one tracker listed 1.15.3 as latest; the newer entry is used) | [Packer 1.15 status](https://endoflife.ai/hashicorp-packer/1.15) | Confirmed from trackers |
| Vagrant | 2.4.9, released 2025-08-21, is the newest stable release listed; the repository publishes 2.4.10 development builds | [Vagrant release list](https://releases.sh/hashicorp/vagrant-releases.md) | Check at install |
| Vagrant from WSL2 | HashiCorp documents `VAGRANT_WSL_ENABLE_WINDOWS_ACCESS=1` to use the Windows VirtualBox from WSL; community guides add the `virtualbox_WSL2` plugin and require matching Vagrant versions on both sides | [Vagrant WSL documentation](https://developer.hashicorp.com/vagrant/docs/other/wsl) | Not confirmed on this host |

## What changed on the way

The first plan used Hyper-V virtual machines (decision 0001). The lab platform was changed to VirtualBox (decision 0005) on request, before any VM was built.

## Prediction check

Prediction before the research: CIS content exists for the newest release of both OS families. Result: the CIS Benchmarks exist for both, but scanner content is confirmed only for Ubuntu 24.04, and only as a draft profile for RHEL 10. The Ubuntu 26.04 and Rocky 10 cases remain open.

## Caveats

Desk research only; no command was run on the laptop. Phase 1 starts with on-host checks: that a Rocky Linux 10 guest boots under VirtualBox with Windows' hypervisor active, that `oscap info` lists a CIS Level 1 profile on the installed guests, and that WSL2 reaches a host-only guest.
