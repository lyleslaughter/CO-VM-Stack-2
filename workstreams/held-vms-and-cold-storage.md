# Workstream: Held VMs and cold-storage boxes

## Goal

The dangerous leftover VMs from the AEP decom stay provably inert through the
CO bring-up, their disk-destroy decision gets made and executed, and the two
cold-storage boxes are parked with a documented retention state.

## ⚠ Standing rule

The VMs below were powered off **un-demoted** — the graceful tear-down never
ran and the `aep.local` forest ended by power-off. Each still contains live
forest state. **Never power one on with a network attached.** If one must be
inspected, disconnect/remove all vNICs first and confirm in the VM settings
before boot.

## Held VMs

| VM | What it is | State | Disposition |
|---|---|---|---|
| TBD (aep.local DC 1) | aep.local domain controller (was 10.12.6.101–103 range) | Powered off, un-demoted | Disk-destroy decision pending |
| TBD (aep.local DC 2) | aep.local domain controller | Powered off, un-demoted | Disk-destroy decision pending |
| TBD (aep.local DC 3) | aep.local domain controller | Powered off, un-demoted | Disk-destroy decision pending |
| AEP-002-CA-01 | AEP certificate authority | Powered off, un-demoted | Disk-destroy decision pending |

(Exact DC VM names: fill from the AEP repo inventory — `../../AEP/`.)

## Quarantine checklist (gate for bring-up step 5)

- [ ] All four VMs located in inventory after hosts come up
- [ ] All four confirmed powered off
- [ ] All four confirmed excluded from autostart
- [ ] vNICs disconnected (or noted per-VM why not) as belt-and-suspenders
- [ ] State recorded here with date

## Disk-destroy decision

Pending. The trust-break observation window closed 2026-06-30 with the site
vacate following 2026-07-07; the remaining question is whether any retention
need justifies keeping the disks (the 2026-06-22 Nakivo DC image backups on
aiu-002-transporter-01 already cover point-in-time recovery).

- [ ] Confirm retention requirement (or lack of one) for the DC/CA disks
- [ ] Decide: destroy disks vs. archive VMs off-cluster
- [ ] Execute and record here + Asana comment

## Cold-storage boxes

| Box | Purpose | Retention state |
|---|---|---|
| AEPCECAM01 | Milestone camera footage retention from the AEP site | End date TBD — do not wipe |
| aiu-002-transporter-01 | Nakivo appliance — holds the 2026-06-22 aep.local DC image backups | Keep until disk-destroy decision executed — do not wipe |

- [ ] Decide physical placement (racked here vs. shelved) — coordinate with [rack-and-cable.md](./rack-and-cable.md)
- [ ] Confirm AEPCECAM01 footage retention end date with stakeholders
- [ ] Record power state expectations (powered on for access, or off until needed?)

## Asana

[Stand up datacenter equipment at CO](https://app.asana.com/1/106264684584116/project/1210666642706963/task/1213908955875142) — GID `1213908955875142`, due 2026-08-31
