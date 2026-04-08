# Changelog — Pillar to Post Dispatch Tool

All notable changes to this project are documented here.  
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.7.3] — 2026-04-07

### Changed
- **Global proximity optimization** — the assignment engine now considers all jobs and all inspectors together before making any assignments, rather than processing jobs in the order they were entered. All possible inspector-job pairs are ranked by distance (closest first) and assigned greedily from that ranked list. This ensures the closest available inspector gets each job regardless of input order, preventing situations like an inspector getting a house 27 miles away when a closer inspector was available

---

## [1.7.2] — 2026-04-07

### Changed
- **Smarter split logic — only split when necessary** — the algorithm now calculates exactly how many splits are needed to cover idle inspectors and stops there. Previously it split aggressively whenever anyone was idle, causing over-splitting
- 3 Gas+Sewer jobs with 3 inspectors → each inspector gets one full house, no splits
- 3 Gas+Sewer jobs with 4 inspectors → exactly 1 job is split to cover the 4th inspector, the other 2 go as full houses
- Splits only fire when two idle inspectors are available (one for gas, one for sewer), preventing any inspector from doubling up

---

## [1.7.1] — 2026-04-07

### Fixed
- **Stephen doubling up on AM jobs** — sewer partner selection was falling back to already-assigned inspectors when no idle sewer candidate existed, causing one inspector to receive both a primary gas job and a sewer add-on. Sewer partner now strictly requires no prior primary assignment and never falls back to the broader pool, ensuring the split only happens when a truly idle sewer-certified inspector is available

---

## [1.7.0] — 2026-04-07

### Changed
- **Split assignments are now silent** — when a Gas+Sewer job is split across two inspectors, each inspector sees it as their own standalone job with no "split w/" language, no sewer add-on sections, and no cross-references. Gas inspector sees [GAS], sewer inspector sees [SEWER], clean and simple
- **Office waypoint now visible in export** — a "── Midday: [address] ──" line appears between the AM and PM windows in the exported schedule when an office is set
- **Office waypoint now visible in Assignments tab** — a gold banner shows the midday waypoint address at the top of the assignments list when an office is set
- **Split assignment bug fixed** — rewrote spread logic to track primary job count separately from total touches; an inspector who already received a sewer split now counts as having a primary job and won't be used as a sewer add-on on a second job, preventing the circular cross-reference issue

---

## [1.6.1] — 2026-04-07

### Fixed
- **Critical: app stuck on "Geocoding…" when adding jobs** — old dropdown-based `updateDrops` function was still present after the pill UI replacement, crashing with `Cannot read properties of null` on the removed `f-jreq`/`f-jexc` select elements, which prevented `renderAll` from completing and left the status bar frozen

---

## [1.6.0] — 2026-04-07

### Added
- **Settings transfer** in the Export tab — export your full inspector roster, office, and jobs as a single clipboard code; paste it on any other device or browser to restore everything instantly
- Import validates the code before applying so a bad paste won't wipe your data

---

## [1.5.0] — 2026-04-07

### Added
- **Multi-select requested inspectors** — jobs can now have multiple requested inspectors; auto-assign picks whichever has the lightest load at assignment time
- **Multi-select excluded inspectors** — jobs can now exclude multiple inspectors; selecting a person as requested automatically removes them from excluded and vice versa
- **Office / Midday Waypoint** — new section at the top of the Inspectors tab to set a shared office address
  - Office appears on the map as a distinct brown square pin 🏢
  - Drive time estimates now route inspectors through the office between AM and PM windows (home → AM jobs → office → PM jobs → home)
  - Office persists in localStorage between sessions
  - Clearable with the × button

### Changed
- Request/exclusion UI replaced from single dropdowns to pill-style multi-tap selectors (blue = requested, red = excluded)
- Backwards compatible — existing jobs with single `requestedId`/`excludedId` values are automatically migrated to the new array format on load

---

## [1.4.2] — 2026-04-07

### Changed
- Drive speed estimate updated from 25 mph to **45 mph** to better reflect suburban/rural STL region coverage

---

## [1.4.1] — 2026-04-07

### Fixed
- **Critical: Split logic failing silently when all inspectors had equal load** — `pickBest` sorts by load count first then distance, so with everyone at zero touches it was returning the same closest inspector for both the gas and sewer pool. The `bg.id !== bs.id` guard correctly caught the collision but fell back to single assignment instead of forcing a different sewer pick
- Gas pool now prefers idle inspectors first before falling back to full pool
- Sewer pool now **explicitly excludes whoever was just picked for gas**, then prefers idle from remaining candidates
- Result: Stone (4th inspector) now correctly receives a sewer split assignment when Gas+Sewer jobs are available and he is idle

---

## [1.4.0] — 2026-04-07

### Fixed
- **Critical: Stone (4th inspector) not appearing on schedule** — spread logic was calculating idle inspector count once upfront before any jobs were processed, causing it to go stale mid-loop. After the first Gas+Sewer split consumed two inspector slots, subsequent jobs no longer saw accurate idle counts and skipped further splits
- Spread decision now recalculates per-job in real time — each job checks how many inspectors currently have zero touches before deciding whether to split
- Scenario that now works correctly: 4 inspectors, 3 AM Gas+Sewer jobs (1 requested) → all 4 inspectors receive an assignment via two splits

---

## [1.3.0] — 2026-04-07

### Added
- **Estimated time display** on Jobs tab and Assignments tab
  - Each job card shows a ⏱ badge with total estimated service time
  - Each inspector header on Assignments tab shows total estimated day length including drive time
- **Drive time estimation** built into inspector totals (25 mph STL metro average, home → jobs → home)

### Changed
- Radon and Mold service times are now treated as **concurrent** (run simultaneously), not additive
  - Radon only → 10 min
  - Mold only → 15 min
  - Radon + Mold → 15 min (longest wins)
- Removed countdown timer UI (replaced by static time estimates)

---

## [1.2.0] — 2026-04-07

### Added
- **Smart job distribution** — when available inspectors outnumber jobs, the algorithm now aggressively splits Gas + Sewer jobs across two inspectors to spread work evenly rather than leaving inspectors idle
- **Service time constants** defined for all job types: Gas = 45 min, Sewer = 60 min, Radon = 10 min, Mold = 15 min

### Changed
- Auto-assign now checks remaining inspector slots vs unassigned jobs before deciding whether to split
- Requested inspector assignments still processed first before spread logic applies

---

## [1.1.0] — 2026-04-07

### Changed
- Replaced **Nominatim (OpenStreetMap)** geocoder with **Geoapify**
  - Significantly better address coverage for suburban St. Louis
  - Proximity bias set to STL metro center for more accurate local results
  - Apartment/unit number stripping with automatic retry on failed lookups
  - Cleaner address display format (e.g. "123 Main St, Ballwin")

---

## [1.0.0] — 2026-04-07

### Initial Release
- Single-page dispatch scheduling tool for Pillar to Post St. Louis region
- **Inspectors tab** — add/edit/remove inspectors with home address and service certifications (Gas, Sewer, Radon, Mold)
- **Jobs tab** — add/edit/remove jobs with address, AM/PM window, service types, requested/excluded inspector, and notes
- **Assignments tab** — auto-assign jobs by proximity and load balancing with manual override support
- **Export tab** — formatted plain-text schedule with copy to clipboard
- Interactive map (Leaflet + CARTO) showing inspector home bases, job pins, and routing lines
- Gas + Sewer split assignment logic (two inspectors, one job)
- Cert conflict detection and warning badges
- Inspector availability toggle (mark away for the day)
- Data persistence via localStorage (survives browser refresh)
- Hosted via GitHub Pages
