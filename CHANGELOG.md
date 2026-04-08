# Changelog — Pillar to Post Dispatch Tool

All notable changes to this project are documented here.  
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

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
