# Workstream: Rack and cable the AEP stack at CO

## Goal

All moved AEP equipment physically racked in the prepared CO rack, dual-fed
across the two UPS/PDU feeds, and cabled per the teardown photos — ready for
the power-on sequence in [bring-up.md](./bring-up.md).

## Current state (2026-08-11)

- **Racked 2026-08-11**: switches, PowerStore, and hosts are in the rack and
  cable management is installed. Power + data cabling planned for 2026-08-12.
- Equipment was powered off, moved to the CO datacenter 2026-07-07, staged
  (Asana comment 2026-07-08).
- Destination rack was stripped of old gear before racking.
- PDUs for two separate UPSs are installed in the rack.
- Cabling reference: [../photos/aep-teardown-2026-07-07/](../photos/aep-teardown-2026-07-07/)
  — morning shots show the labeled cabling as-built at AEP (port labels like
  `ESXI-01 SLOT2-P1`); afternoon shots show the emptied racks.

## Hardware to rack

Full per-device detail (service tags, firmware, IPs) lives in
[`../../VMWare_upgrades/inventory/aep-site.md`](../../VMWare_upgrades/inventory/aep-site.md)
— do not duplicate it here.

| Device | Qty | Notes |
|---|---|---|
| Dell S5212F-ON ToR switch | 2 | |
| Dell PowerStore 500T | 1 | Already on 4.3.1.0 (upgraded 2026-04-23) |
| Dell PowerEdge R650 (ESXi host) | 3 | |
| AEPCECAM01 (Milestone camera retention) | 1 | Cold storage — shelved, not racked; see [held-vms-and-cold-storage.md](./held-vms-and-cold-storage.md) |
| aiu-002-transporter-01 (Nakivo) | 1 | Cold storage — shelved, not racked; holds 2026-06-22 DC image backups |

## Open questions

- UPS-A / UPS-B outlet mapping per PSU: TBD

## Next steps

- [x] Draft rack elevation (placement decided at racking 2026-08-11 — as-built elevation still to be recorded below)
- [x] Rack the two S5212F-ON switches (2026-08-11)
- [x] Rack the PowerStore 500T (2026-08-11)
- [x] Rack the three R650 hosts (2026-08-11)
- [x] Decide placement for AEPCECAM01 and aiu-002-transporter-01 — shelved in the CO datacenter, not racked, unpowered, un-networked (2026-08-11)
- [ ] Power-cable every device dual-feed: PSU1 → UPS-A PDU, PSU2 → UPS-B PDU
- [ ] Cable data per the teardown photos and existing port labels (do not power on — power-on order is owned by [bring-up.md](./bring-up.md))
- [ ] Verify every cable label against the photos before first power-on
- [ ] Record the as-built elevation and power map in this file

## Asana

[Stand up datacenter equipment at CO](https://app.asana.com/1/106264684584116/project/1210666642706963/task/1213908955875142) — GID `1213908955875142`, due 2026-08-31
