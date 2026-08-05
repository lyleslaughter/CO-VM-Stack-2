# CO-VM-Stack-2 Project Setup Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Scaffold the CO-VM-Stack-2 repo (dashboard README, CLAUDE.md, four workstream files, committed photos), integrate it into the INFRA parent registry, cross-reference the two sibling repos, and log the kickoff to Asana.

**Architecture:** Documentation-only repo following the AEP-repo workstream pattern (Goal / Current state / Open questions / Next steps / Asana link per file) with README as the live dashboard. No code, no tests — verification is file-level (content present, links resolve, commits clean).

**Tech Stack:** Markdown, git (independent repo, remote `https://github.com/lyleslaughter/CO-VM-Stack-2.git`), Asana MCP (`mcp__claude_ai_Asana__add_comment`).

**Spec:** `docs/superpowers/specs/2026-08-05-co-vm-stack-2-design.md`

## Global Constraints

- Dates are ISO `YYYY-MM-DD`.
- `TBD` markers inside workstream files are **deliberate content** (spec convention: never invent hostnames/IPs/port maps — the user fills them). They are not plan placeholders.
- `*.local.md` is gitignored; sensitive values go there, never in tracked files.
- Content rule that must survive verbatim wherever held VMs are mentioned: the 3 un-demoted `aep.local` DC VMs and `AEP-002-CA-01` must **never** be powered on with a network attached.
- Every commit message ends with `Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>`.
- Tasks 1–7 commit in **this** repo. Task 8 commits in the INFRA parent repo, Task 9 in `../VMWare_upgrades` and `../AEP` — each is its own git repo; run git from inside the right directory.
- Driving Asana task GID: `1213908955875142` (due 2026-08-31).

---

### Task 1: Repo hygiene — .gitignore, photo rename, commit photos

**Files:**
- Create: `.gitignore`
- Rename: `AEP Pictures/` → `photos/aep-teardown-2026-07-07/` (28 JPGs, currently untracked)

**Interfaces:**
- Produces: the path `photos/aep-teardown-2026-07-07/` that Tasks 2, 5, and 6 link to.

- [ ] **Step 1: Write `.gitignore`**

```gitignore
# macOS metadata
.DS_Store

# Local-only notes — credentials, IPs, anything sensitive (INFRA convention)
*.local.md
```

- [ ] **Step 2: Rename the photo directory (plain `mv` — files are untracked, `git mv` won't work)**

```bash
mkdir -p photos
mv "AEP Pictures" photos/aep-teardown-2026-07-07
```

- [ ] **Step 3: Verify — 28 JPGs at the new path, old path gone, .DS_Store ignored**

Run: `command ls photos/aep-teardown-2026-07-07 | wc -l && git status --short | head -5`
Expected: `28`; git status shows `?? .gitignore` and `?? photos/` only (no `.DS_Store`, no `AEP Pictures/`).

- [ ] **Step 4: Commit**

```bash
git add .gitignore photos
git commit -m "$(cat <<'EOF'
Add gitignore and teardown reference photos

28 photos from the 2026-07-07 AEP site teardown: morning shots capture
the labeled ESXi/switch/storage cabling before disconnection (the
re-cabling reference), afternoon shots show the emptied racks.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 2: `workstreams/rack-and-cable.md`

**Files:**
- Create: `workstreams/rack-and-cable.md`

**Interfaces:**
- Consumes: `photos/aep-teardown-2026-07-07/` from Task 1.
- Produces: workstream filename that the README (Task 6) and CLAUDE.md (Task 7) index.

- [ ] **Step 1: Write the file with exactly this content**

```markdown
# Workstream: Rack and cable the AEP stack at CO

## Goal

All moved AEP equipment physically racked in the prepared CO rack, dual-fed
across the two UPS/PDU feeds, and cabled per the teardown photos — ready for
the power-on sequence in [bring-up.md](./bring-up.md).

## Current state (2026-08-05)

- Equipment powered off, moved to the CO datacenter 2026-07-07, staged and
  ready to rack (Asana comment 2026-07-08).
- Destination rack completely stripped of old gear.
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
| AEPCECAM01 (Milestone camera retention) | 1 | Cold storage — see [held-vms-and-cold-storage.md](./held-vms-and-cold-storage.md) |
| aiu-002-transporter-01 (Nakivo) | 1 | Cold storage — holds 2026-06-22 DC image backups |

## Open questions

- Rack elevation plan (which U for each device): TBD
- UPS-A / UPS-B outlet mapping per PSU: TBD
- Do the cold-storage boxes rack here or shelve elsewhere at CO? TBD

## Next steps

- [ ] Draft rack elevation (top-of-rack switches, storage, hosts, cold-storage boxes)
- [ ] Rack the two S5212F-ON switches
- [ ] Rack the PowerStore 500T
- [ ] Rack the three R650 hosts
- [ ] Decide placement for AEPCECAM01 and aiu-002-transporter-01
- [ ] Power-cable every device dual-feed: PSU1 → UPS-A PDU, PSU2 → UPS-B PDU
- [ ] Cable data per the teardown photos and existing port labels (do not power on — power-on order is owned by [bring-up.md](./bring-up.md))
- [ ] Verify every cable label against the photos before first power-on
- [ ] Record the as-built elevation and power map in this file

## Asana

[Stand up datacenter equipment at CO](https://app.asana.com/1/106264684584116/project/1210666642706963/task/1213908955875142) — GID `1213908955875142`, due 2026-08-31
```

- [ ] **Step 2: Verify links resolve**

Run: `command ls "photos/aep-teardown-2026-07-07" >/dev/null && command ls ../VMWare_upgrades/inventory/aep-site.md`
Expected: both paths exist (relative links in the file are correct from `workstreams/`).

- [ ] **Step 3: Commit**

```bash
git add workstreams/rack-and-cable.md
git commit -m "$(cat <<'EOF'
Add rack-and-cable workstream

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 3: `workstreams/network.md`

**Files:**
- Create: `workstreams/network.md`

**Interfaces:**
- Produces: workstream filename indexed by Tasks 6 and 7.

- [ ] **Step 1: Write the file with exactly this content**

```markdown
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
```

- [ ] **Step 2: Commit**

```bash
git add workstreams/network.md
git commit -m "$(cat <<'EOF'
Add network integration workstream

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 4: `workstreams/bring-up.md`

**Files:**
- Create: `workstreams/bring-up.md`

**Interfaces:**
- Consumes: quarantine rules from `workstreams/held-vms-and-cold-storage.md` (Task 5 — written next; the link is forward-referenced and both land before the README indexes them).
- Produces: the "done means" validation checklist the README (Task 6) points at.

- [ ] **Step 1: Write the file with exactly this content**

```markdown
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
```

- [ ] **Step 2: Commit**

```bash
git add workstreams/bring-up.md
git commit -m "$(cat <<'EOF'
Add bring-up workstream with power-on order and safety gate

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 5: `workstreams/held-vms-and-cold-storage.md`

**Files:**
- Create: `workstreams/held-vms-and-cold-storage.md`

**Interfaces:**
- Produces: the quarantine checklist that `bring-up.md` (Task 4) gates on.

- [ ] **Step 1: Write the file with exactly this content**

```markdown
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
```

- [ ] **Step 2: Commit**

```bash
git add workstreams/held-vms-and-cold-storage.md
git commit -m "$(cat <<'EOF'
Add held-VMs and cold-storage workstream with quarantine rules

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 6: `README.md` — live dashboard

**Files:**
- Create: `README.md`

**Interfaces:**
- Consumes: all four workstream filenames (Tasks 2–5), photo path (Task 1), spec path.

- [ ] **Step 1: Write the file with exactly this content**

```markdown
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
| **Status** | Active — racking not yet started |
| **Next milestone** | Rack elevation drafted + equipment racked |
| **Blockers** | None |
| **Last update** | 2026-08-05 |

## Workstreams

| Workstream | State |
|---|---|
| [Rack and cable](./workstreams/rack-and-cable.md) | Not started — rack prepped, gear staged |
| [Network integration](./workstreams/network.md) | Not started — addressing decision open |
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
```

- [ ] **Step 2: Verify every relative link in the README resolves**

Run: `command ls workstreams/rack-and-cable.md workstreams/network.md workstreams/bring-up.md workstreams/held-vms-and-cold-storage.md docs/superpowers/specs/2026-08-05-co-vm-stack-2-design.md ../VMWare_upgrades/inventory/aep-site.md >/dev/null && echo OK`
Expected: `OK`

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "$(cat <<'EOF'
Add README dashboard

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 7: Rewrite `CLAUDE.md` for the real project

**Files:**
- Modify: `CLAUDE.md` (full replacement of the placeholder written before the project was defined)

**Interfaces:**
- Consumes: structure from Tasks 1–6.

- [ ] **Step 1: Replace the entire file with exactly this content**

```markdown
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
- `workstreams/*.md` — one file per workstream, all following the shape:
  **Goal**, **Current state**, **Open questions**, **Next steps** (`- [ ]`
  checklist), **Asana** link.
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
```

- [ ] **Step 2: Commit**

```bash
git add CLAUDE.md
git commit -m "$(cat <<'EOF'
Rewrite CLAUDE.md for the CO stand-up project

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 8: Push, then register in the INFRA parent repo

**Files:**
- Modify: `../projects.yaml` (add entry between `automox` and `Defender`)
- Modify: `../.gitignore` (add `CO-VM-Stack-2/` between `automox/` and `Defender/`)

**Interfaces:**
- Consumes: all commits from Tasks 1–7 (push happens first so the remote isn't empty when referenced).

- [ ] **Step 1: Push this repo**

```bash
git push -u origin main
```
Expected: `main` pushed to `https://github.com/lyleslaughter/CO-VM-Stack-2.git`.

- [ ] **Step 2: Add the projects.yaml entry** — insert this block after the `automox` entry and before `Defender` in `/Users/lyle.slaughter/Documents/Projects/INFRA/projects.yaml`:

```yaml
  - dir: CO-VM-Stack-2
    section: active
    asana_parent_gids:
      - "1213908955875142"  # Stand up datacenter equipment at CO (due 2026-08-31)
    asana_parent_label: "Stand up datacenter equipment at CO"
    status_override: null
    deadline_override: null
    next_milestone_override: null
    blockers_override: null
    done_date: null
    done_outcome: null
```

- [ ] **Step 3: Add the .gitignore line** — in `/Users/lyle.slaughter/Documents/Projects/INFRA/.gitignore`, in the independent-repos block, insert `CO-VM-Stack-2/` on its own line between `automox/` and `Defender/`.

- [ ] **Step 4: Verify the parent repo doesn't see the subdir**

Run: `git -C /Users/lyle.slaughter/Documents/Projects/INFRA status --short`
Expected: shows ` M projects.yaml` and ` M .gitignore` only — no `CO-VM-Stack-2/` line.

- [ ] **Step 5: Commit in the parent repo**

```bash
git -C /Users/lyle.slaughter/Documents/Projects/INFRA add projects.yaml .gitignore
git -C /Users/lyle.slaughter/Documents/Projects/INFRA commit -m "$(cat <<'EOF'
Register CO-VM-Stack-2 subproject

New independent repo tracking the physical stand-up of the relocated
AEP VMware stack at CO (Asana 1213908955875142, due 2026-08-31).

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 9: Cross-reference the sibling repos

**Files:**
- Modify: `../VMWare_upgrades/CLAUDE.md` (after the "Project phases" numbered list in "Project Context")
- Modify: `../AEP/CLAUDE.md` (in the "Scope boundary with sibling repo" section)

**Interfaces:**
- Consumes: this repo's path name `../CO-VM-Stack-2/`.

- [ ] **Step 1: Edit `../VMWare_upgrades/CLAUDE.md`** — insert this paragraph immediately after the numbered "Project phases" list (after the line `6. Validate Proxmox cluster; optionally migrate workloads from VMware over time`):

```markdown

> **Phase 4 hand-off:** the CO-side *physical* stand-up of the moved AEP gear (rack, cable, A/B power, network integration, power-on to healthy-as-VMware) is tracked in `../CO-VM-Stack-2/`. This repo keeps the deferred AEP-gear component upgrades and the Phase 5+ Proxmox decision/execution.
```

- [ ] **Step 2: Edit `../AEP/CLAUDE.md`** — in the "Scope boundary with sibling repo" section, append this sentence to the paragraph that begins "Sibling project at `../VMWare_upgrades`":

```markdown
The CO-side physical stand-up of the moved stack (rack/cable/power/bring-up, including the held aep.local DC/CA VM quarantine during power-on) is tracked in `../CO-VM-Stack-2/`.
```

- [ ] **Step 3: Commit in each sibling repo**

```bash
git -C /Users/lyle.slaughter/Documents/Projects/INFRA/VMWare_upgrades add CLAUDE.md
git -C /Users/lyle.slaughter/Documents/Projects/INFRA/VMWare_upgrades commit -m "$(cat <<'EOF'
Cross-reference CO-VM-Stack-2 for the CO physical stand-up

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
EOF
)"
git -C /Users/lyle.slaughter/Documents/Projects/INFRA/AEP add CLAUDE.md
git -C /Users/lyle.slaughter/Documents/Projects/INFRA/AEP commit -m "$(cat <<'EOF'
Cross-reference CO-VM-Stack-2 for the CO physical stand-up

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
EOF
)"
```

Note: do NOT push the parent/sibling repos unless the user asks — only this repo's push (Task 8 Step 1) was approved in the design.

---

### Task 10: Kickoff comment on the Asana task

**Files:** none (Asana MCP call)

**Interfaces:**
- Consumes: the pushed repo URL from Task 8.

- [ ] **Step 1: Post the comment** via `mcp__claude_ai_Asana__add_comment` on task `1213908955875142` with exactly this text:

```
2026-08-05 — CO stand-up tracking repo created

The CO-side body of work on this task now has a dedicated tracking repo: https://github.com/lyleslaughter/CO-VM-Stack-2

- Scope: rack → cable → A/B power → network integration → power-on to "stack healthy as VMware," plus held-VM quarantine (3 aep.local DCs + AEP-002-CA-01, never power on networked) and the two cold-storage boxes (AEPCECAM01, aiu-002-transporter-01).
- Out of scope there (stays in VMWare_upgrades repo): the deferred switch/firmware/vCenter/ESXi upgrades and the Proxmox-vs-upgrade-in-place decision.
- Current physical state: rack stripped of old gear, PDUs for the two separate UPS feeds installed, equipment staged since 2026-07-07. The 28 teardown photos (cabling reference) are committed in the repo.

Workstreams: rack-and-cable, network, bring-up (with power-on order + safety gate), held-vms-and-cold-storage.
```

- [ ] **Step 2: Verify** — the tool result returns the created story; no further check needed.

---

## Self-review notes

- **Spec coverage:** structure (Tasks 1–7), parent integration (Task 8), sibling cross-refs (Task 9), git/push (Tasks 1–8), Asana convention (Task 10). "Done means" from the spec is encoded as bring-up.md's validation section. No gaps found.
- **Placeholders:** all `TBD`s are deliberate spec-mandated content markers for the user, per Global Constraints; every file's full content is present in the plan.
- **Consistency:** workstream filenames, photo path, and Asana GID are identical across Tasks 2–10 and match the spec.
