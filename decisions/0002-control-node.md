# 0002: Ansible control node in WSL2

Date: 2026-11-15
Status: Proposed

## Context

The Ansible control node (the machine that runs the playbooks and SSHs into the fleet) does not run natively on Windows. The control node also runs ansible-lint, Molecule and the OpenSCAP report tooling. RAM on the host is limited (decision 0001).

## Options

1. **WSL2** (a Linux environment on the same Windows hypervisor).
2. **A dedicated Linux control VM** on Hyper-V.

## Decision

Run the control node in WSL2, with a memory limit set in the WSL configuration.

## Why

- No extra VM to size, patch or keep running.
- Tools and the repository checkout live in one Linux environment.

Trade-offs accepted: WSL2 is itself a VM and takes RAM unless limited. WSL2 reaching a Hyper-V internal switch is not guaranteed; Phase 1 verifies it with `ansible -m ping` against every host. If the path does not work, the fallback is a control VM attached to the same network, and this record is superseded.

## Update 2026-11-15

With VirtualBox as the lab platform (decision 0005), two requirements were recorded from preliminary research:

- WSL2 stays in its default NAT networking mode. A WSL issue report states that a WSL2 instance in mirrored mode could not reach a VirtualBox guest on a host-only network, while an older write-up reports that NAT mode reaches it. Phase 1 tests this with `ping` and `ssh` from WSL2 to a guest.
- Vagrant runs inside WSL2 and drives the Windows VirtualBox. HashiCorp documents the `VAGRANT_WSL_ENABLE_WINDOWS_ACCESS` environment variable for this; community guides add the `virtualbox_WSL2` plugin and matching Vagrant versions on both sides. Phase 1 confirms a guest can be created and reached this way.
