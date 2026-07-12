# Changelog

All notable changes to the Comms Plan Builder app are documented in this file, most recent first.

---

## 2026-07-12 — Fixed: Tab 4 beltpack types weren't actually scoped to Tab 1

**Fixed**
- The previous fix only scoped the specific *panel* models (KP-32, IrisX, etc.) to what was added on Tab 1 — the base beltpack types (Green-GO BPX/WBPX, Clear-Com HelixNet beltpack, Clear-Com FreeSpeak II/Edge beltpack, RTS DBP, Performer beltpack) were still always shown regardless of brand chosen. Reported: selecting only Riedel on Tab 1 still showed "Green-GO Beltpack" as an option on Tab 4.
- Every base type is now tied to its real system model(s) and only appears once that system is chosen on Tab 1:
  - DBP → RTS OMS/ODIN/ADAM
  - Bolero beltpack → Riedel Bolero
  - Performer beltpack → Riedel Performer/C44
  - Green-GO BPX/WBPX → any Green-GO model (BPX, WBPX, MCX, MCXD)
  - Clear-Com HelixNet beltpack → HelixNet or Arcadia
  - Clear-Com FreeSpeak II/Edge beltpack → FreeSpeak or Arcadia
- Generic "Panel / Keypanel (unlisted)", Analog beltpack, and Other remain universal fallbacks.
- New crew rows now default to a type that's actually valid for what's chosen, instead of always defaulting to DBP.

---

## 2026-07-12 — Tab 4 panel types now scoped to what you chose on Tab 1

**Changed**
- The "type of point" dropdown on Tab 4 no longer lists every panel model that exists in the catalogue. It now only shows the specific panels you actually added (qty > 0) on Tab 1 for the systems you've chosen.
  - Example: add Clear-Com Arcadia on Tab 1 and pick V-Series IrisX as its panel — only "V-Series IrisX Keypanel" shows up on Tab 4, not RTS keypanels, Riedel SmartPanels, or Green-GO stations you never added.
- If you assigned someone to a panel type and later remove that panel from Tab 1, their row keeps showing their original selection (so nothing is silently reset) — it just won't be offered to new rows.
- Switching to Tab 4 now always refreshes this list against the latest Tab 1 choices.

---

## 2026-07-12 — Specific panel models on Tab 4's "type of point"

**Added**
- The Roles & Partylines tab's device type dropdown now includes every specific panel/keypanel model from the Tab 1 catalogue (KP-32, KP-12, RSP-2318 variants, DCP-1116, V-Series/IrisX, HMS-4X, MCX/MCXD family, etc.) as its own selectable type, each with a party-line cap matching that panel's real key count — not just the generic "Panel / Keypanel" fallback.
- The two lists share one source of truth (`PANEL_OPTIONS` from Tab 1), so adding a panel model in one place automatically makes it selectable in the other.
- The old generic option is now labelled "Panel / Keypanel (generic / unlisted)" and stays uncapped, for panel hardware not in the catalogue.

---

## 2026-07-12 — Compatible panel/keypanel models per system (Tab 1)

**Added**
- Tab 1 system cards now show the actual compatible desk/rack panel models for systems that have a known catalogue, with a quantity per model instead of one generic "panels" number:
  - RTS OMS / ODIN / ADAM-M/Cronus: KP-32, KP-12, KP-5032, KP-4016, KP-632/EKP-632 keypanels
  - Riedel Artist / Tango: RSP-2318 SmartPanel (Basic/Plus/Pro), DCP-1116 Control Panel
  - Riedel Performer/C44: CD-2, CW-2 desk/wall stations, CR-2/CR-4 rack stations
  - Clear-Com Arcadia: V-Series Iris/IrisX keypanels, HelixNet HMS-4X (via I.V. port)
  - Clear-Com Eclipse HX: V-Series keypanels (12/16/24-key), V-Series IrisX (32-key)
  - Clear-Com HelixNet: HMS-4X Main Station
  - Clear-Com Encore: 2-channel desk/wall station
  - Green-GO MCX: MCX Rack Station, MCXEXT extension
  - Green-GO MCXD: MCXD Desktop Station, MCXDEXT extension, WPX Wall Panel X
- Systems without a known panel catalogue (Bolero, Acrobat, PunQtum, FreeSpeak, Agent-IC/LQ, all RF-only brands, Custom/Other) keep the original manual "panels" number field.
- The Tab 2 schematic view now shows the Tab-1-derived panel total as read-only for systems with a catalogue, so there's one source of truth.

---

## 2026-07-12 — FreeSpeak beltpack channel cap fix

**Fixed**
- "Clear-Com FreeSpeak beltpack" had no party line cap on Tab 4, unlike every other device type. Split it into the two real product generations, each with its correct channel limit:
  - Clear-Com FreeSpeak II beltpack: 5 channels (FSII-BP19)
  - Clear-Com FreeSpeak Edge beltpack: 9 channels (FSE-BP50)

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
