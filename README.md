# CO-VM-Stack-2 — AEP VMware Stack Stand-Up at Central Office

Stand up the relocated AEP VMware stack in the CO datacenter: rack → cable →
power → network → bring-up → **stack healthy and reachable as VMware**.

The AEP site was vacated 2026-07-07; all equipment was powered off, moved to
CO, and staged. The destination rack is stripped, with PDUs for two separate
UPS feeds in place.

**Driving Asana task:** [Stand up datacenter equipment at CO](https://app.asana.com/1/106264684584116/project/1210666642706963/task/1213908955875142) (GID `1213908955875142`) — **due 2026-08-31**

## Status

| | |
|---|---|
| **Status** | Active — equipment racked, cabling next |
| **Next milestone** | Power (dual-feed) + data cabling per teardown photos — planned 2026-08-12 |
| **Blockers** | None |
| **Last update** | 2026-08-12 |

## Workstreams

| Workstream | State |
|---|---|
| [Rack and cable](./workstreams/rack-and-cable.md) | In progress — racked 2026-08-11, cable management in; cabling planned 2026-08-12 |
| [Network integration](./workstreams/network.md) | In progress — uplinks decided 2026-08-12 (direct to core, no Meraki layer); addressing decision open |
| [Bring-up](./workstreams/bring-up.md) | Blocked on rack + network |
| [Held VMs & cold storage](./workstreams/held-vms-and-cold-storage.md) | Standing quarantine rule in force; disk-destroy decision pending |

## Reference

- **Cabling photos:** [photos/aep-teardown-2026-07-07/](./photos/aep-teardown-2026-07-07/) — 28 shots from the AEP teardown; morning = labeled cabling as-built, afternoon = emptied racks
- **Hardware inventory:** [`../VMWare_upgrades/inventory/aep-site.md`](../VMWare_upgrades/inventory/aep-site.md) (do not duplicate here)
- **Design spec:** [docs/superpowers/specs/2026-08-05-co-vm-stack-2-design.md](./docs/superpowers/specs/2026-08-05-co-vm-stack-2-design.md)

## Scope boundaries

- Deferred component upgrades (switch OS10, firmware, vCenter, ESXi) and the
  Proxmox-vs-upgrade-in-place decision: [`../VMWare_upgrades/`](../VMWare_upgrades/)
- AEP domain decom history (trust break, held-VM origin): [`../AEP/`](../AEP/)

## ⚠ Standing safety rule

The 3 un-demoted `aep.local` DC VMs and `AEP-002-CA-01` must **never** be
powered on with a network attached — see
[held-vms-and-cold-storage.md](./workstreams/held-vms-and-cold-storage.md).
