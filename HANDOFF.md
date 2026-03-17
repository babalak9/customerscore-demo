# CustomerScore Demo — Project Handoff

## What this is
A single-file clickable app prototype for the CustomerScore product vision.
Built as a pure HTML/CSS/JS SPA — no build step, no dependencies beyond CDN links.

**File:** `index.html` (~4,200 lines)

---

## How to run
Open `index.html` directly in a browser — or use any static file server:
```bash
cd "/Users/matejkaluzik/Claude code/Onboarding flow"
python3 -m http.server 8765
# then open http://localhost:8765
```

To reset all demo data: open browser console and run `localStorage.clear()` then reload.

---

## Demo flow (click path)
```
v-signup → [Create account]
  → v-analyzing (auto, 3s)
  → v-stripe → [Connect Stripe]
  → v-stripe-loading (auto, 4.8s)
  → v-dashboard (Churn)         ← AI cards, metrics, MRR chart, engagement banner
      [Churn / Trial toggle]
  → v-dashboard-trial           ← Trial funnel, journey timeline, plan mix
  → v-segments                  ← Rich cards: MRR, trend, playbook count
  → v-segment-detail            ← Customer list, conditions, active playbooks [PLANNED]
  → v-playbooks                 ← 3 playbook cards → New Playbook modal
  → v-playbook-review           ← Email draft, revise toggle, approve
  → v-playbook-live             ← Stats, goal bar, chart, table
  → [Banner] → connect modal
  → v-connect-analytics         ← PostHog/Mixpanel OAuth screen
  → v-connect-analytics-loading ← 4-step progress animation
  → v-connect-analytics-events  ← Event picker, AI pre-selected
  → v-connect-analytics-success ← Done screen
```

**Quick skip:** Click "Skip to dashboard demo" on the signup screen.

---

## Navigation system
All views are `<div id="v-*" class="hidden">`. The `goView(id)` function hides all `[id^="v-"]` elements and shows the target one. Auto-calls `renderPlaybooks()` / `renderSegments()` for those views.

---

## Sidebar nav
| Label | Action |
|---|---|
| Dashboard | `goView('v-dashboard'); initDashboard()` |
| Segments | `goView('v-segments')` |
| Agents | `goView('v-playbooks')` |
| Analytics | placeholder |
| Settings | placeholder |

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

## Architecture notes

- **`db` helper** backed by `localStorage` — `db.get/set/push/update/remove(key)`
- **Seed data** runs once on first load (checks `localStorage.playbooks` before seeding)
- **`state` object** tracks: `chartsInit`, `trialChartsInit`, `analyticsProvider`, `returnTo`, `currentSegment`
- **`window.chartInstances`** guard — always destroy before recreating Chart.js instances
- **`checkAnalyticsSyncing()`** called in both `initDashboard()` and `initDashboardTrial()` — restores syncing banner from localStorage if < 10 min old
- **CSS `.hidden`** = `display:none !important` — never use `style="display:none"`
- **Chart.js 4.4.0** loaded via CDN

---

## What's been built
| Feature | Status |
|---|---|
| Signup / Stripe connect flow | ✅ |
| Churn dashboard (AI cards, MRR chart, engagement banner) | ✅ |
| Trial-to-Paid dashboard (funnel, journey timeline, plan mix) | ✅ |
| Churn/Trial toggle in topbar | ✅ |
| localStorage persistence (`db` helper + seed data) | ✅ |
| Segment cards (MRR, trend, playbook count — rich cards) | ✅ planned |
| Segment detail view (customer list, conditions, playbooks) | ✅ planned |
| Playbooks list with create/review/live flow | ✅ |
| PostHog/Mixpanel connection flow (4-screen) | ✅ |
| Banner → syncing state with progress bar after connect | ✅ |
| Analytics nav item | ⬜ placeholder |
| Settings nav item | ⬜ placeholder |

---

## Next session: implement segment detail view

The plan is in `.claude/plans/snazzy-nibbling-pelican.md`. Summary:

1. **Update seed data** — add `mrr`, `trend`, `playbooks` fields to each segment
2. **Rewrite `renderSegments()`** — richer card footer (MRR · trend · playbook count), card onclick → `openSegment(id)`
3. **New `v-segment-detail` view** — stats row, condition pills, customer table, active playbooks section
4. **`openSegment(id)` JS function** — populates and navigates to detail view
5. **Update HANDOFF.md** after implementation
6. **Git commit + push**

See `.claude/plans/snazzy-nibbling-pelican.md` for full spec including mock customer data.
