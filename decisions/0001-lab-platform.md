# 0001: Lab platform: Hyper-V virtual machines

Date: 2026-11-15
Status: Proposed

## Context

The fleet needs about six Linux virtual machines on one Windows 11 Home laptop with 15.3 GB of RAM and a single SSD. The workload is Linux-only, so nested virtualization (the requirement that drove the platform choice in a related lab) is not needed. Windows' own hypervisor is already active on this laptop, with virtualization-based security enabled. Measurements of this host are recorded in the related lab's evidence: [yashwanth312/restorepoint](https://github.com/yashwanth312/restorepoint).

VM addresses must be stable because the Ansible inventory depends on them. The Hyper-V Default Switch changed its subnet after a Windows restart on this host.

## Options

1. **Hyper-V virtual machines** on an internal switch with a fixed subnet and NAT.
2. **VirtualBox 7.2.** Installed already. Reported to run in a slower compatibility mode while Windows' hypervisor is active; its speed on this host was not measured.
3. **Proxmox VE nested in a Hyper-V VM.** Adds a virtualization layer and its RAM overhead that a Linux-only fleet does not need.

## Decision

Use Hyper-V virtual machines on an internal switch with a fixed private subnet (10.78.0.0/24) and NAT.

## Why

- No extra layer: VMs run on the hypervisor Windows already uses, as does WSL2.
- A fixed subnet keeps inventory addresses stable across restarts.
- RAM is the limiting resource. Without a nested layer, more of it goes to the fleet.

Trade-offs accepted: Hyper-V on Windows 11 Home is enabled through a workaround that Microsoft does not support. Vagrant's Hyper-V provider has limited networking, so VM creation is scripted instead. VirtualBox may turn out to be adequate; its speed on this host was not tested.

Status moves to Accepted when Phase 1 records that the VMs boot, keep their addresses across a host restart, and fit in memory.
