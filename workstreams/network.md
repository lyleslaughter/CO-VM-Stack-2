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

## Decision: uplink architecture — direct to existing core (2026-08-12)

**Decided 2026-08-12: the stack uplinks directly to the existing CO core
switching** — QSFP → quad-SFP breakout fiber from each S5212F-ON to the
core, and iDRAC/OOB management on the existing CO copper network. The
three Meraki switches that came over from AEP (SFP fiber pair + copper
switch) are **not** used for this stack; their disposition (spares vs.
redeploy elsewhere) is TBD.

**Rationale:** east-west isolation (iSCSI / vMotion / VM-VM) is unaffected
— that traffic never leaves the S5212F-ON pair, which remains this stack's
own switching domain. A Meraki aggregation layer would sit only in the
north-south path while adding two hops, two failure points, UPS draw, and
recurring per-device Meraki licenses (Meraki switches stop forwarding when
licensing lapses). Direct-to-core is simpler to operate and to reason
about through the pending OS10 upgrade and any future Proxmox rebuild.

**Pre-reqs to verify during implementation** (tracked in Next steps):
spare SFP ports + optic/breakout compatibility on the core, each ToR gets
its own uplink path (redundancy preserved), core ports trunk this stack's
VLANs, and the CO management copper network has spare ports for iDRAC/OOB
(if it turns out not to, the single Meraki copper switch is the fallback).

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

- [x] Decide uplink architecture — direct to existing core, no Meraki layer (2026-08-12, see Decision above)
- [ ] Verify core-side pre-reqs: spare SFP ports, optic/breakout compatibility, per-ToR redundant paths, VLAN trunking
- [ ] Verify spare copper ports on the CO management network for iDRAC/OOB (fallback: single Meraki copper switch)
- [ ] Decide addressing: keep `10.12.x` routed vs. re-IP (record decision + rationale here)
- [ ] Identify uplink ports on the CO side and patch the S5212F-ON uplinks (QSFP → quad-SFP breakout per ToR)
- [ ] Configure/verify VLANs for management, iSCSI, and vMotion
- [ ] Verify iDRAC/management reachability for all three R650s, the PowerStore, and both switches
- [ ] Update DNS records if re-IP'd
- [ ] Record the final addressing and uplink map in this file (sensitive detail → `network.local.md`)

## Asana

[Stand up datacenter equipment at CO](https://app.asana.com/1/106264684584116/project/1210666642706963/task/1213908955875142) — GID `1213908955875142`, due 2026-08-31
