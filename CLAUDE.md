# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Markdown-tracked project to **stand up the relocated AEP VMware stack at the
CO datacenter** — rack, cable, power (dual UPS/PDU feeds), network
integration, and controlled power-on, ending at "stack healthy and reachable
as VMware." An **independent git repo** under the INFRA umbrella (see
`../CLAUDE.md` for cross-cutting AIU context); remote
`https://github.com/lyleslaughter/CO-VM-Stack-2.git`.

There is **no code** — only documentation, checklists, and reference photos.
Driving Asana task: `1213908955875142` ("Stand up datacenter equipment at
CO", due 2026-08-31).

## ⚠ Standing safety rule

The 3 un-demoted `aep.local` DC VMs and `AEP-002-CA-01` (powered off at the
2026-07-07 site vacate, graceful demote never ran) still contain live forest
state. They must **never** be powered on with a network attached. Any
work touching VM power state goes through the quarantine checklist in
`workstreams/held-vms-and-cold-storage.md` first.

## Scope boundaries

| In scope here | Lives elsewhere |
|---|---|
| Rack layout, cabling, A/B power map | Component upgrades (switch OS10, firmware, vCenter, ESXi) → `../VMWare_upgrades` |
| CO network integration (uplinks, VLANs, addressing) | Proxmox-vs-upgrade decision + execution → `../VMWare_upgrades` |
| Power-on sequence + bring-up validation | Hardware inventory detail (tags, versions, licenses) → `../VMWare_upgrades/inventory/aep-site.md` |
| Held-VM quarantine + disk-destroy decision | AEP domain decom history → `../AEP` |
| Cold-storage boxes (AEPCECAM01, aiu-002-transporter-01) | |

Link to sibling repos rather than copying their content.

## Structure

- `README.md` — live dashboard (status, next milestone, blockers). Keep it
  current after every working session.
- `workstreams/*.md` — one file per workstream. The default shape is
  **Goal**, **Current state**, **Open questions**, **Next steps**
  (`- [ ]` checklist), **Asana** link — used where it fits. Runbook-style
  files (`bring-up.md`) and standing-rules files
  (`held-vms-and-cold-storage.md`) adapt that shape to their content, but
  always keep a **Goal** and an **Asana** link.
- `photos/aep-teardown-2026-07-07/` — the re-cabling reference. Morning
  shots = labeled cabling as-built at AEP; afternoon = emptied racks.
- `docs/superpowers/` — specs and plans.

## Conventions

- Dates: ISO `YYYY-MM-DD`
- `*.local.md` is gitignored — credentials, sensitive IPs, or port maps that
  shouldn't hit GitHub go there
- Don't invent infrastructure details — unknown hostnames/IPs/VLANs are
  written `TBD` for the user to fill
- Post progress comments to Asana task `1213908955875142` at milestones
  (`mcp__claude_ai_Asana__add_comment`); when the project closes, comment a
  summary **before** marking complete (INFRA-wide rule)
- Update progress by editing the workstream checklists; commit with short
  messages

## Key references

- Sibling repos: `../VMWare_upgrades/` (hardware/upgrades/Proxmox),
  `../AEP/` (domain decom record)
- Outline (`docs.aiu3.net`) via `mcp__outline__*` tools — standard WebFetch
  fails on Outline pages; read-only unless asked
