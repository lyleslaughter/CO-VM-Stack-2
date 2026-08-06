# Workstream: Network integration at CO

## Goal

The relocated stack is reachable on the CO network: switch uplinks in place,
management/iSCSI/vMotion connectivity working, and an addressing decision
(keep vs. re-IP) made and executed.

## Current state (2026-08-05)

- At the AEP site the stack lived on `10.12.x` addressing (e.g. vCenter
  `AIU-002-vCenter-01.aiu3.net` / `10.12.6.11` — see
  [`../../VMWare_upgrades/inventory/aep-site.md`](../../VMWare_upgrades/inventory/aep-site.md)).
- The AIU LAN at CO is `10.20.0.0/16` with the gateway at `10.20.0.1`.
- No uplink or addressing decisions made yet.

## Open questions

- Keep the `10.12.x` subnet and route it at CO, or re-IP the stack into CO
  addressing? TBD
- Do the AEP-side VLANs (management / iSCSI / vMotion) exist at CO, or do
  they need to be created/stretched? VLAN IDs: TBD
- Which CO core/distribution switches do the S5212F-ON uplinks land on, and
  on which ports? TBD
- Any firewall rules keyed to the old AEP-site addressing that need updating?
  TBD
- DNS: which records need updating if devices are re-IP'd? TBD

## Next steps

- [ ] Decide addressing: keep `10.12.x` routed vs. re-IP (record decision + rationale here)
- [ ] Identify uplink ports on the CO side and patch the S5212F-ON uplinks
- [ ] Configure/verify VLANs for management, iSCSI, and vMotion
- [ ] Verify iDRAC/management reachability for all three R650s, the PowerStore, and both switches
- [ ] Update DNS records if re-IP'd
- [ ] Record the final addressing and uplink map in this file (sensitive detail → `network.local.md`)

## Asana

[Stand up datacenter equipment at CO](https://app.asana.com/1/106264684584116/project/1210666642706963/task/1213908955875142) — GID `1213908955875142`, due 2026-08-31
