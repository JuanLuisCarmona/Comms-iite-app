# Changelog

All notable changes to the Comms Plan Builder app are documented in this file, most recent first.

---

## 2026-07-12 — Brand/model registry expansion

**Added**
- 5 new brands: Hollyland (Solidcom M1, Solidcom C1), Pliant Technologies (CrewCom, MicroCom XR/M), Eartec (UltraLITE/HUB), Telikou (MDS-400, Skyline SK-100), Vokkero (Guardian/Guardian Show).
- New Clear-Com models: Encore (analog partyline), Agent-IC / LQ Series (IP mobile bridge).
- New Riedel models: Tango (compact matrix), PunQtum (IP digital partyline), Acrobat (wireless).
- Green-GO split from a single lumped entry into four real models: MCX, MCXD, BPX, WBPX.

**Notes**
- The new RF-only wireless kits (Hollyland, Pliant, Eartec, Telikou, Vokkero) deliberately don't generate a Network Considerations checklist card in Tab 3 — they're point-to-point radio systems with no switches/IGMP/PTP involved, so there's nothing to check there.
- Total brand count: 10 (RTS, Riedel, Clear-Com, Green-GO, Hollyland, Pliant Technologies, Eartec, Telikou, Vokkero, Other/Generic).

---

## 2026-07-12 — Client report: per-PL columns

**Changed**
- Tab 4 "Print client report" now renders one column per party line with a ✓/✗ per crew member, instead of a single comma-separated "Party Lines" text column. PL headers rotate vertically in print to stay readable when there are a lot of them.

---

## 2026-07-12 — Service worker fix (PWA staleness bug)

**Fixed**
- `sw.js` was using a cache-first strategy, so updates to `index.html` never reached installed devices without a manual hard-refresh. Switched to network-first (falls back to cache only when offline) so future updates propagate automatically the next time the app is opened with a connection.

---

## 2026-07-12 — Device channel caps on Tab 4

**Fixed**
- Party line tick-boxes on the Roles & Partylines tab now respect each device type's real physical channel limit instead of allowing unlimited selections:
  - DBP (wired beltpack): 4 channels
  - Bolero beltpack: 6 keys
  - Clear-Com HelixNet beltpack: 2 channels
  - Green-GO BPX/WBPX: 32 channels
  - Performer beltpack: 2 channels
- Switching a crew member to a more restrictive device type automatically trims any over-limit PL selections down to fit.
- Panel/Keypanel, FreeSpeak, Analog, and Other remain uncapped since those vary too much by model to set one number.

---

## 2026-07-12 — PWA packaging

**Added**
- Converted the tool into an installable Progressive Web App: `manifest.json`, `sw.js`, and generated icons (192px/512px).
- Installable via GitHub Pages + "Add to Home Screen" (mobile) or the browser install prompt (desktop). Works fully offline once installed.

---

## 2026-07-12 — Major restructure: 4-tab vendor-agnostic rebuild

**Changed**
- Replaced the original 3-tab bespoke-calculator structure with a generic 4-tab workflow:
  1. **Choose System** — add any number of system blocks (brand → model → PL/conference/IFB/beltpack/panel counts), flagged against known published limits.
  2. **Schematic** — auto-generated SVG diagram from systems, switches, and connections (Copper/Fiber, 4-Wire, etc.) you define.
  3. **Network Considerations** — checklist cards appear automatically based on which brand families are in use (RTS/Riedel OMNEO-Dante, Clear-Com AES67, Green-GO, Universal), including a flagged warning when mixing OMNEO and AES67 systems on the same network (conflicting IGMP snooping guidance).
  4. **Roles & Partylines** — auto-generated party line directory, crew roster with device type/headset/verified status, CSV export with selectable columns, and a print-friendly client report.

---

## 2026-07-12 — Crew tab rebuilt to match real production paperwork

**Changed**
- Reworked the Crew tab after reviewing an actual production comms plan sheet: added Job Info header, a shared Master Channel/Partyline name registry, and per-base-station tables (Packs + NSA ID/analog breakout) matching real-world layout instead of a generic one-size-fits-all roster.

---

## 2026-07-12 — Initial build

**Added**
- System Planner tab with dedicated capacity calculators for RTS OMS vs ODIN, Riedel Bolero (Standalone vs Integrated Artist), and Riedel Performer/C44.
- Network Readiness Checklist tab (OMNEO/Dante network requirements — IGMP, RSTP, EEE, QoS, VLAN).
- Crew Assignments tab (first version): device type, headset, and party-line tick sheet.

---

### For collaborators

If you're picking this up to extend it: the whole app lives in `comms_plan_pwa/index.html` as a single self-contained file (vanilla HTML/CSS/JS, no build step). The brand/model registry (`BRANDS`, `MODEL_SPECS`, `BRAND_FAMILY`) and the network checklist content (`NETWORK_CHECKLISTS`) are the two places most future "add a brand" or "add a network note" contributions will land. Please add an entry to this file for anything you ship.
