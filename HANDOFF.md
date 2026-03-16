# CustomerScore Demo — Project Handoff

## What this is
A single-file clickable app prototype for recording a CustomerScore product vision video.
Built as a pure HTML/CSS/JS SPA — no build step, no dependencies beyond CDN links.

**File:** `index.html` (~2,800 lines)

---

## How to run
Open `index.html` directly in a browser — or use any static file server:
```bash
cd "/Users/matejkaluzik/Claude code/Onboarding flow"
python3 -m http.server 8765
# then open http://localhost:8765
```

---

## Demo flow (click path for video recording)
```
v-signup → [Create account]
  → v-analyzing (auto, 3s)
  → v-stripe → [Connect Stripe]
  → v-stripe-loading (auto, 4.8s)
  → v-dashboard        ← 4 AI insight cards + metrics + MRR chart
  → v-segments         ← AI-suggested segment cards + inline builder
  → v-playbooks        ← 3 playbook cards  →  [+ New Playbook] modal
  → v-playbook-review  ← email draft, revise toggle, approve
  → v-playbook-live    ← stats, goal bar, Chart.js chart, table
```

**Quick skip:** Click "Skip to dashboard demo" on the signup screen.

---

## Navigation system
All views are `<div id="v-*" class="hidden">`. The `goView(id)` function (in `<script>`)
hides all `[id^="v-"]` elements and shows the target one.

---

## Sidebar nav (all 5 views)
| Label | Action |
|---|---|
| Dashboard | `goView('v-dashboard'); initDashboard()` |
| Segments | `goView('v-segments')` |
| Agents | `goView('v-playbooks')` |
| Analytics | does nothing (placeholder) |
| Settings | does nothing (placeholder) |

---

## Key design tokens (CSS variables in `:root`)
```css
--brand: #4f46e5       /* indigo */
--bg: #f9fafb
--border: #e5e7eb
--text-1: #111827
--text-2: #6b7280
--r: 8px
--shadow-sm: 0 1px 3px rgba(0,0,0,0.08)
```

---

## Files to transfer
| Path | What |
|---|---|
| `Claude code/Onboarding flow/index.html` | The entire app |
| `.claude/plans/snazzy-nibbling-pelican.md` | Original build plan |

---

## How to continue in a new Claude Code session

1. Copy the project folder to the new machine / account
2. Open the folder in Claude Code: `claude "/path/to/Onboarding flow"`
3. Paste this summary at the start of the session so Claude has full context

**Key things Claude needs to know to continue:**
- Single-file SPA, no framework, no git repo
- Always read the file before editing (agents write sequentially, not in parallel)
- CSS class `.hidden` = `display: none !important` — use this, not `style="display:none"`
- Chart.js 4.4.0 loaded via CDN — guard with `window.chartInstances` before creating
- `goView('[id^="v-"]')` selector covers all views inside and outside `#app`

---

## What's been built vs placeholder
| Feature | Status |
|---|---|
| Signup / analyzing / Stripe connect flow | ✅ fully built |
| Dashboard (AI cards, metrics, MRR chart) | ✅ fully built |
| Customer Segments view | ✅ fully built |
| Agents / Playbooks list | ✅ fully built |
| New Playbook modal | ✅ fully built |
| Playbook review (email draft + revise) | ✅ fully built |
| Playbook live report | ✅ fully built |
| Analytics nav item | ⬜ placeholder (no view yet) |
| Settings nav item | ⬜ placeholder (no view yet) |
