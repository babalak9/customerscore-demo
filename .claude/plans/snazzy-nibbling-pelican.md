# Segments as Live Monitors + Playbook Hubs

## Context
Segments currently feel like a setup tax — lightweight cards that exist only to be attached to playbooks. The user wants segments to become the primary object: a live monitor showing customer health, MRR at risk, weekly trend, and the playbooks running on them. Clicking a segment opens a detail view with a customer list and attached playbooks. The "Create playbook" action moves inside the segment, not on the card.

**File:** `/Users/matejkaluzik/Claude code/Onboarding flow/index.html`

---

## What changes

### 1 — Seed data: add new fields to each segment

Current fields: `id, name, tag, customers, description, color, createdAt`

Add:
- `mrr` — MRR at risk / in segment (string, e.g. `"£8,400"`)
- `trend` — weekly customer delta (number, e.g. `+4`, `-1`, `0`)
- `playbooks` — array of playbook names active on this segment (e.g. `['Churn Intervention']`)

Updated seed data:
```js
{ id:1, name:'At-Risk Pro Users',      tag:'High Risk',  customers:23, mrr:'£8,400',  trend:+4,  playbooks:['Churn Intervention','Win-back Email'],   ... }
{ id:2, name:'Expansion Ready',        tag:'Expansion',  customers:8,  mrr:'£2,200',  trend:+2,  playbooks:['Expansion Signal'],                       ... }
{ id:3, name:'New Users — Onboarding', tag:'Onboarding', customers:12, mrr:'£900',    trend:+3,  playbooks:[],                                          ... }
{ id:4, name:'High-Value Stable',      tag:'Stable',     customers:47, mrr:'£18,600', trend:-1,  playbooks:['Churn Intervention'],                     ... }
```

---

### 2 — Enhanced segment cards (`renderSegments()`)

Replace current footer ("Create playbook →" link) with richer data row. Make entire card clickable → `openSegment(s.id)`.

New card layout:
```
[TAG BADGE]                          [23 customers]
At-Risk Pro Users

Low login activity + cancellation…

──────────────────────────────────────
£8,400 MRR    ↑ 4 this week    2 playbooks active
```

- MRR, trend (↑/↓ with color), playbooks count — all in the footer row
- Trend: green if negative (fewer at-risk), red if positive for risk segments; handled simply by showing arrow + number
- Remove "Create playbook →" and "✕ delete" from card footer (delete moves to detail view)
- `onclick="openSegment(${s.id})"` on the card div

---

### 3 — New `v-segment-detail` view

Insert before `<!-- AGENT-1-SEGMENTS-END -->`. Uses `db-shell` layout (sidebar + main), same pattern as `v-segments`.

**Topbar:**
- `← Back` link → `goView('v-segments')`
- Segment name as page title
- Tag badge

**Stats row (3 boxes):**
- Customers: `23`
- MRR at risk: `£8,400`
- Trend: `↑ 4 new this week` (red for risk segments, green for expansion)

**Conditions row:**
- Small pill badges showing auto-detected conditions (mocked per segment tag)
  - High Risk → `Plan = Pro` · `Last login > 14d` · `Churn risk = high`
  - Expansion → `Usage ↑ 30%` · `Seats added` · `Feature adoption = high`
  - Onboarding → `Signed up < 30d` · `First login complete`
  - Stable → `MRR > £200/mo` · `No risk signals`

**Customer list table:**
Columns: Name / Plan / Health / Last Active / MRR
- 4–5 mock rows per segment (hardcoded by segment id, realistic names)
- Health shown as a coloured dot + score (e.g. 🔴 28/100 for at-risk)

Mock customers:
```
id=1 (At-Risk):  Sarah Chen / Pro / 🔴 28 / 8 days ago / £249
                 James Kirk / Pro / 🔴 34 / 12 days ago / £249
                 Priya Nair / Pro / 🟡 51 / 5 days ago / £249
                 Tom Walsh  / Pro / 🔴 19 / 21 days ago / £249

id=2 (Expansion): Marco Rossi / Growth / 🟢 87 / Today / £99
                  Zara Ahmed  / Growth / 🟢 91 / Today / £99
                  Liu Wei     / Pro   / 🟢 94 / Yesterday / £249

id=3 (Onboarding): Alex Morgan / Starter / 🟡 62 / Today / £29
                   Sam Taylor  / Starter / 🟡 55 / Yesterday / £29
                   Dana Lee    / Growth  / 🟢 78 / Today / £99

id=4 (Stable):  Olivia Brown / Pro / 🟢 92 / Today / £249
                Raj Patel    / Pro / 🟢 88 / Yesterday / £249
                Emma Wilson  / Growth / 🟢 85 / Today / £99
                Kai Tanaka   / Pro / 🟢 96 / Today / £249
```

**Active playbooks section:**
- Title: "Active playbooks"
- List of playbook name pills (from `s.playbooks`) with "View →" links → `goView('v-playbooks')`
- If empty: "No playbooks yet"
- `[+ Add playbook]` button → `goView('v-playbooks')`

---

### 4 — JS: `openSegment(id)` function

```js
function openSegment(id) {
  const seg = db.get('segments').find(s => s.id === id);
  if (!seg) return;
  state.currentSegment = seg;

  // Populate header
  document.getElementById('seg-detail-title').textContent = seg.name;
  document.getElementById('seg-detail-tag').textContent = seg.tag;

  // Stats
  document.getElementById('seg-detail-customers').textContent = seg.customers;
  document.getElementById('seg-detail-mrr').textContent = seg.mrr;
  const trendEl = document.getElementById('seg-detail-trend');
  trendEl.textContent = (seg.trend > 0 ? '↑ ' : '↓ ') + Math.abs(seg.trend) + ' this week';
  trendEl.style.color = seg.trend > 0 ? '#ef4444' : '#10b981';

  // Conditions
  const condMap = { 'High Risk':['Plan = Pro','Last login > 14d','Churn risk = high'], 'Expansion':['Usage ↑ 30%','Seats added','Feature adoption = high'], 'Onboarding':['Signed up < 30d','First login complete'], 'Stable':['MRR > £200/mo','No risk signals','Tenure > 6mo'] };
  const conds = condMap[seg.tag] || [];
  document.getElementById('seg-detail-conditions').innerHTML = conds.map(c => `<span style="background:white;border:1px solid var(--brand-border);color:#4338ca;font-size:12px;font-weight:600;padding:4px 10px;border-radius:20px;">${c}</span>`).join('');

  // Customer table
  const customerMap = { /* mock data object keyed by id */ };
  // ... renders table rows into #seg-detail-customers-table

  // Playbooks
  const pbEl = document.getElementById('seg-detail-playbooks');
  pbEl.innerHTML = seg.playbooks.length
    ? seg.playbooks.map(p => `<span style="...">${p} <a onclick="goView('v-playbooks')" style="...">View →</a></span>`).join('')
    : '<span style="color:var(--text-3);font-size:13px;">No playbooks yet</span>';

  goView('v-segment-detail');
}
```

Also add `state.currentSegment = null` to initial state object.

---

### 5 — `goView()` patch

No change needed — `v-segment-detail` will be found automatically by `[id^="v-"]` selector.

---

## Also fix: event list scrolling bug

In `v-connect-analytics-events`, the `#analytics-events-list` div has conflicting inline styles:
```html
style="max-height:320px; overflow-y:auto; ... overflow:hidden;"
```
`overflow:hidden` overrides `overflow-y:auto`. Remove `overflow:hidden` from the inline style.

---

## After implementation: update HANDOFF.md

Replace the entire contents of `/Users/matejkaluzik/Claude code/Onboarding flow/HANDOFF.md` with the current state of the project. Key sections to update:

**File size:** ~4,200+ lines (was 2,800)

**Updated demo flow:**
```
v-signup → v-analyzing → v-stripe → v-stripe-loading
  → v-dashboard (Churn)     ← AI cards, metrics, MRR chart, engagement banner
      [Churn/Trial toggle]
  → v-dashboard-trial       ← Trial funnel, journey timeline, plan performance
  → v-segments              ← Richer cards: MRR, trend, playbook count
  → v-segment-detail        ← Customer list, conditions, active playbooks
  → v-playbooks             ← 3 playbook cards, create new flow
  → v-playbook-review / v-playbook-live
  → v-connect-analytics     ← PostHog/Mixpanel OAuth screen
  → v-connect-analytics-loading / v-connect-analytics-events / v-connect-analytics-success
```

**Updated "What's been built" table:**
| Feature | Status |
|---|---|
| Signup / Stripe connect flow | ✅ |
| Churn dashboard (AI cards, MRR chart) | ✅ |
| Trial-to-Paid dashboard (funnel, journey, plan mix) | ✅ |
| Churn/Trial toggle in topbar | ✅ |
| localStorage persistence (`db` helper) | ✅ |
| Segment cards with MRR, trend, playbook count | ✅ |
| Segment detail view (customers, conditions, playbooks) | ✅ |
| Playbooks list with create/review/live flow | ✅ |
| PostHog/Mixpanel connection flow (4-screen) | ✅ |
| Banner → syncing state after connect | ✅ |
| Analytics nav item | ⬜ placeholder |
| Settings nav item | ⬜ placeholder |

**Key architecture notes for the new session:**
- `db` helper backed by `localStorage` — `db.get/set/push/update/remove(key)`
- Seed data runs on first load if `localStorage.playbooks` is empty (checks before seeding)
- `state` object tracks: `chartsInit`, `trialChartsInit`, `analyticsProvider`, `returnTo`, `currentSegment`
- `window.chartInstances` guard — always destroy before recreating Chart.js instances
- `checkAnalyticsSyncing()` called in both `initDashboard()` and `initDashboardTrial()` — restores syncing banner from localStorage if < 10 min old
- CSS `.hidden = display:none !important` — never use inline style display:none
- To reset all data: `localStorage.clear()` in browser console then reload

**Next session priorities (the user will decide):**
- Onboarding "Review AI segments" step (after Stripe connect, before dashboard)
- Segment name auto-generated from description (currently name is typed first)
- Fix segment create panel textarea width (it's not spanning full width)
- Playbook → segment linkage in data model (currently one-directional UI only)

---

## Final step: copy plan into project + git commit

The plan file lives at `~/.claude/plans/snazzy-nibbling-pelican.md` (home dir, not in repo).
Copy it into the project so it's tracked in git:

```bash
cp ~/.claude/plans/snazzy-nibbling-pelican.md \
   "/Users/matejkaluzik/Claude code/Onboarding flow/.claude/plans/snazzy-nibbling-pelican.md"
```

Then stage and commit:
- `index.html` (all changes)
- `HANDOFF.md` (updated)
- `.claude/plans/snazzy-nibbling-pelican.md` (plan file, now inside project)

Commit message: `Add segment detail view, rich cards, fix event list scroll + update handoff`

---

## Verification
1. Go to Segments → 4 cards show MRR, trend arrow, playbooks count in footer
2. Click a card → `v-segment-detail` opens with correct segment data
3. Stats row shows customer count, MRR, trend with correct colour
4. Condition pills match the segment tag
5. Customer table shows 3–4 rows with realistic data
6. Active playbooks section shows playbook names with "View →" links
7. "← Back" navigates back to `v-segments`
8. "+ Add playbook" navigates to `v-playbooks`
9. Connect analytics flow: event list is scrollable
10. Git log shows new commit, HANDOFF.md reflects current state
