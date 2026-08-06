# Workstream: Power-on sequence and bring-up validation

## Goal

The stack comes back online as VMware in a controlled order — switches →
PowerStore → ESXi hosts → vCenter — with the held aep.local VMs provably
quarantined before any VM starts, ending in a validated healthy stack.

## ⚠ Safety gate (read before powering anything)

The 3 un-demoted `aep.local` DC VMs and `AEP-002-CA-01` still contain live
forest state. They must **never** be powered on with a network attached —
one of them booting networked comes up as a live `aep.local` DC. See
[held-vms-and-cold-storage.md](./held-vms-and-cold-storage.md). No VM powers
on until the quarantine checklist there is green.

## Current state (2026-08-05)

- Nothing powered on. Racking/cabling ([rack-and-cable.md](./rack-and-cable.md))
  and network integration ([network.md](./network.md)) are prerequisites.
- Hosts were powered off 2026-07-07 at the AEP site vacate; last known-good
  stack state predates that.

## Power-on order

- [ ] 1. **Switches** — both S5212F-ON up, uplinks to CO verified ([network.md](./network.md))
- [ ] 2. **PowerStore 500T** — power on, wait for healthy state, verify management reachability
- [ ] 3. **ESXi hosts** (one at a time) — BEFORE the first host: plan to check VM autostart config immediately at boot; **all VMs stay powered off**
- [ ] 4. **Autostart check** — on each host, confirm no VM is configured to autostart (host client → Manage → System → Autostart); disable anything found
- [ ] 5. **Quarantine verification** — held-VMs checklist in [held-vms-and-cold-storage.md](./held-vms-and-cold-storage.md) fully green
- [ ] 6. **Datastores** — iSCSI paths to PowerStore up on every host, datastores mounted, multipath healthy
- [ ] 7. **vCenter (VCSA)** — power on `AIU-002-vCenter-01`, wait for services, log in
- [ ] 8. **Management VMs** — OME and SCG powered on and reachable (they are ordinary VMs, not held)

## Validation ("done" for the whole project)

- [ ] All three hosts connected in vCenter, no alarms beyond expected
- [ ] All datastores mounted on all hosts; multipath shows expected path count
- [ ] vCenter healthy (services green, licensing intact)
- [ ] PowerStore healthy, no faults
- [ ] Held VMs still powered off, NICs disconnected, per quarantine checklist
- [ ] README dashboard updated to reflect stack-online state
- [ ] Close-out comment posted to Asana `1213908955875142`

## Open questions

- Where does the VCSA VM live (which host/datastore) — confirm before host power-on order matters? TBD
- Any VMs besides VCSA/OME/SCG expected to start for stack validation? TBD

## Asana

[Stand up datacenter equipment at CO](https://app.asana.com/1/106264684584116/project/1210666642706963/task/1213908955875142) — GID `1213908955875142`, due 2026-08-31
