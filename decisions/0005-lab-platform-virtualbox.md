# 0005: Lab platform: VirtualBox virtual machines

Date: 2026-11-15
Status: Accepted

## Context

The fleet needs Linux virtual machines on one Windows 11 Home laptop with 15.3 GB of RAM and a single SSD. Windows' own hypervisor is active on this laptop, so VirtualBox runs on top of it. Oracle VirtualBox 7.2.18 is installed. The workload is Linux-only, so nested virtualization is not needed.

VirtualBox 7.2 added x86-64-v3 CPU feature exposure (including AVX and AVX2) when it runs on the Windows hypervisor, according to its change log. Rocky Linux 10 requires that CPU level, and earlier 7.1 releases were reported to fail to boot it in that mode. VirtualBox host-only networks accept addresses only in 192.168.56.0/21 by default.

## Options

1. **VirtualBox with Vagrant.**
2. **Hyper-V virtual machines** ([decision 0001](0001-lab-platform.md), superseded).
3. **Proxmox VE nested in a Hyper-V VM.**

## Decision

Use VirtualBox with Vagrant. Each guest has a NAT adapter for internet access and a host-only adapter in 192.168.56.0/24 with a fixed address for the Ansible inventory.

## Why

- VirtualBox is already installed and is Vagrant's default provider.
- The Molecule Vagrant driver defaults to VirtualBox, which gives VM-based role tests. A container cannot prove SELinux settings because it shares the host kernel.
- Fixed host-only addresses keep inventory stable across restarts.

Trade-offs accepted: VirtualBox runs through Windows' hypervisor, which can reduce speed; the effect on this host was not measured. Whether Rocky Linux 10 and Ubuntu 26.04 guests boot in this mode is confirmed by Phase 1 smoke tests, not assumed. Running Vagrant from WSL2 needs extra setup ([decision 0002](0002-control-node.md)). If a guest cannot boot, the fallback is Hyper-V (decision 0001) or an earlier OS release recorded in decision 0003.
