# CO-VM-Stack-2 — Project Design

**Date:** 2026-08-05
**Status:** Approved 2026-08-05
**Driving Asana task:** `1213908955875142` — "Stand up datacenter equipment at CO" (start 2026-07-01, due 2026-08-31, P1-High)

## Purpose

Track the physical stand-up of the relocated AEP VMware stack at the CO datacenter, from staged-on-the-floor to "stack healthy and reachable as VMware." The AEP site was vacated 2026-07-07; all equipment was powered off, moved to CO, and is staged. The destination rack is stripped of old gear, with PDUs for two separate UPS feeds in place. The 28 photos taken during the 2026-07-07 teardown are the cabling reference for re-racking.

## Scope

**In scope (this repo owns):**

- Rack layout and physical racking of the moved AEP gear
- Cabling (keyed to the teardown photos and existing port labels)
- Power: mapping equipment across the two UPS/PDU feeds (A/B)
- Network integration: switch uplinks into the CO network, VLAN/IP changes needed for the stack to come up at CO
- Power-on sequence and bring-up validation (switches → PowerStore → ESXi hosts → vCenter)
- Held-VM safety handling: the 3 un-demoted `aep.local` DC VMs and `AEP-002-CA-01` must **never** power on with a network attached; disk-destroy decision tracking
- Cold-storage boxes that rode along: `AEPCECAM01` (Milestone camera footage retention) and `aiu-002-transporter-01` (Nakivo — holds the 2026-06-22 DC image backups)

**Out of scope (lives elsewhere):**

- Deferred component upgrades (Dell switch OS10, server firmware, vCenter, ESXi) — `../VMWare_upgrades`
- Proxmox-vs-upgrade-in-place decision and execution — `../VMWare_upgrades`
- AD/domain history and decom record — `../AEP`

**Done means:** the stack is racked, cabled, powered A/B, on the CO network, and healthy as VMware — hosts, vCenter, and PowerStore reachable and validated; held VMs and cold-storage boxes dispositioned or explicitly parked with a documented state.

## Repo structure

```
README.md            — live dashboard: status, next milestone, blockers, workstream index
CLAUDE.md            — guidance for Claude Code: scope boundaries, conventions, Asana GID
docs/superpowers/specs/  — this design doc and future specs
workstreams/
  rack-and-cable.md            — rack layout, PDU/UPS A-B power map, cabling plan
  network.md                   — switch uplinks, VLAN/IP changes
  bring-up.md                  — power-on order + validation checklist
  held-vms-and-cold-storage.md — DC/CA VM quarantine + disk-destroy decision; camera + Nakivo boxes
photos/aep-teardown-2026-07-07/  — 28 reference photos (renamed from "AEP Pictures/"), committed as-is (42 MB total)
```

Each workstream file follows the AEP-repo shape: **Goal / Current state / Open questions / Next steps (`- [ ]` checklist) / Asana link**.

## Conventions

Same as sibling repos:

- ISO `YYYY-MM-DD` dates
- `*.local.md` gitignored — credentials, IPs, or anything sensitive goes there
- Don't invent infrastructure details — unknown hostnames/IPs/port maps are written `TBD` for the user to fill
- Progress comments posted to Asana task `1213908955875142` at milestones; when closing, comment before completing
- Cross-repo links instead of duplicated content (hardware/upgrade detail → `../VMWare_upgrades`, domain history → `../AEP`)

## Parent-repo integration

- Add a `CO-VM-Stack-2` entry to INFRA's `projects.yaml` (section: active, `asana_parent_gids: ["1213908955875142"]`)
- Add `CO-VM-Stack-2/` to the INFRA parent `.gitignore` (independent repo, not swallowed by the parent)
- One-line cross-references added to `../VMWare_upgrades/CLAUDE.md` and `../AEP/CLAUDE.md` noting the CO physical stand-up lives here

## Git

Independent repo (like the other ten INFRA subprojects), branch `main`, existing remote `https://github.com/lyleslaughter/CO-VM-Stack-2.git`. Initial commit contains this spec, README, CLAUDE.md, workstreams, and photos; pushed to origin.

## Testing / verification

Documentation repo — no code. Verification is operational: the bring-up checklist in `workstreams/bring-up.md` defines the validation steps for "stack healthy," and the README dashboard must reflect actual state after each working session.
