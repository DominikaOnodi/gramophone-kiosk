# CLAUDE.md — Gramophone Works Kiosk

> Read this at the start of every session. Update the Progress Log after each significant change.

---

## Project Overview

**Client:** The Gramophone Works, Ladbroke Grove, West London (W10 5BU)  
**Managed by:** TSP (building management company)  
**Purpose:** Fullscreen reception kiosk display — auto-rotates through 4 screen types on a loop  
**Type:** Static HTML/CSS/JS — no framework, no build step, no server required

---

## The Building

- 6-floor heritage creative building in West London
- 64,000 sq ft overlooking the Grand Union Canal
- BREEAM and EPC A certified; carbon-neutral timber structure
- NLA Environmental Prize 2021

### Tenants
| Floor   | Tenant                           |
|---------|----------------------------------|
| Ground  | Café, Reception, Kindred Workshop Rooms |
| 1–3     | Kindred Studios (charity, 300+ artists: jewellery, pottery, painting, etc.) |
| 4       | Emilia Wickstead                 |
| 5       | Perfect Moment                   |

---

## How the Kiosk Works

### Screen rotation (defined in `content.js` → `screens` array)
1. **Directory** — floor-by-floor tenant list with TSP logo (shown longest: 22s)
2. **Story** — one building fact at a time, large italic serif, cycles through `facts[]`
3. **Gallery (index 0)** — building exterior photo (13s)
4. **Gallery (index 1)** — second building info photo, placeholder until image added (13s)
5. **Events** — Kindred Studios recurring sessions (13s)

### Key behaviour
- Transitions: 800ms CSS opacity crossfade
- Auto-reload: page reloads every 30 minutes to pick up `content.js` changes
- Fullscreen: triggered on first click (browser security requirement)
- No server needed: content loaded via `<script src="content.js">`, not fetch()

---

## Files

```
gramophone-kiosk/
├── index.html       — 4-screen shell (no content hardcoded here)
├── styles.css       — all visual design
├── app.js           — rotation logic, renderers, transitions
├── content.js       — ALL content lives here (edit to update kiosk)
├── content.json     — inactive reference copy (content.js is the live one)
└── images/
    ├── tsp-logo.png          ✓ added
    ├── building-exterior.jpg ✓ added
    ├── building-info.jpg     ← ADD: second building image (currently placeholder)
    ├── kindred-01.jpg        ← waiting for photos
    ├── kindred-02.jpg        ← waiting for photos
    ├── emilia-01.jpg         ← waiting for photos
    └── perfect-moment-01.jpg ← waiting for photos
```

### How to update content
Edit `content.js` only. Never touch the other files unless changing layout/logic.

### How to activate a new tenant photo
1. Drop image in `images/` (e.g. `kindred-01.jpg`)
2. In `content.js`, find the matching gallery entry and confirm the path is correct
3. In `content.js → screens`, add: `{ type: "gallery", galleryIndex: 2, duration: 13000 }` (where 2 = the array index of that gallery entry)
4. Refresh the browser

---

## Design Direction

- **Aesthetic:** editorial/gallery — Kinfolk magazine meets minimal art gallery
- **Background:** warm cream `#F6F2EC`
- **Text:** near-black `#1C1815`
- **Serif font:** Fraunces (Google Fonts) — headings, floor numbers, facts, event titles
- **Sans font:** Inter (Google Fonts) — eyebrows, tenant names, dates, captions
- **Rules:** flat, no shadows, no rounded cards, no gradients
- **Type sizes:** large for distance viewing (floor numbers ~80–96px, facts ~52px)
- **TSP logo:** bottom-right of directory screen only, quiet and unobtrusive

---

## Progress Log

### Session 1 — 21 June 2026
- Built full kiosk from scratch: `index.html`, `styles.css`, `app.js`, `content.json`
- 4 screen types: Directory, Building Story, Gallery, Events
- Data-driven rotation via `screens[]` array in content — adding new tenant slides = JSON edit only
- Directory screen shown for 22s, others 13s
- 800ms crossfade transition between screens
- TSP logo uses `<img>` with text fallback; `mix-blend-mode: multiply` strips white PNG background
- TSP logo + building exterior image added to `images/`
- Fixed content loading: switched from `fetch(content.json)` (breaks on file://) to `<script src="content.js">` (works everywhere including double-click)
- Page auto-reloads every 30 min to pick up content changes
- Events: Kindred Studios recurring sessions from kindredstudios.co.uk/calendar
- `CLAUDE.md` created

### In Progress
- Second building info image (`images/building-info.jpg`) — user will add soon

### Next Up
- Add `images/building-info.jpg` — second building slide (slot is already in the rotation)
- Tenant photos: Kindred Studios, Emilia Wickstead, Perfect Moment — activate slides when photos arrive
- Kindred events — update when specific dated events are published on their calendar
- Consider custom domain / tablet mounting setup for permanent display

---

## How to Resume

Say: *"Please read CLAUDE.md and summarise the current state of the kiosk project, then let's continue."*
