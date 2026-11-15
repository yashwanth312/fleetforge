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

Trade-offs accepted: WSL2 is itself a VM and takes RAM unless limited. WSL2 reaching a Hyper-V internal switch is not guaranteed; Phase 1 verifies it with `ansible -m ping` against every host. If the path does not work, the fallback is a control VM attached to the same switch, and this record is superseded.
