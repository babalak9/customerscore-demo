# CustomerScore Demo Scenario — Sneak Peek Recording Script

## Context
This is a step-by-step walkthrough script for recording a product demo of the CustomerScore prototype. The narrative follows **Kate**, a Growth Manager at a 35-person B2B SaaS company with 1,200 customers. Kate is losing ~60 customers/month silently and has no visibility into why. The demo shows how CustomerScore goes from zero to AI-driven retention + upsell in minutes — no sales call, no onboarding.

The tone should feel like a "sneak peek" — product-led, fast-moving, confident. Each screen should feel inevitable.

---

## Demo Flow — Step by Step

---

### 0. Opening Hook (before touching the app)

**Narrative (voiceover or camera):**
> "Last month, 60 of your customers quietly left. No ticket. No complaint. They just stopped paying. Do you know why?"

Pause. Then:
> "This is CustomerScore. From sign-up to your first insight — 5 minutes. No sales call. No form. Let's go."

---

### 1. Sign-Up Screen (`v-signup`)

**What to show:** The clean sign-up screen.

**What to do:** Enter a realistic business email, e.g. `kate@growthlab.io` — pause for a beat so viewers can read it — then click the primary sign-up button.

**What to say:**
> "Kate signs up with her work email. That's it. No plan selection, no onboarding wizard."

**Highlight:**
- No credit card required
- No "book a demo" CTA — just sign up and go

---

### 2. AI Analysis Loading (`v-analyzing`)

**What to show:** The 5-step progress animation.

**Steps visible:**
1. Website crawl
2. Product features extraction
3. G2 / Capterra reviews
4. ICP fit identification
5. Knowledge base building

**What to say:**
> "In the background, AI is reading Kate's website, pulling her product's key features, finding reviews on G2 and Capterra — and building a knowledge base. Kate fills in nothing."

**Highlight:** This auto-runs and auto-transitions — zero user input.

---

### 3. Stripe Connection (`v-stripe`)

**What to show:** The Stripe OAuth screen.

**What to do:** Click the "Connect with Stripe" button.

**What to say:**
> "One click. OAuth. No API keys, no tokens, no CSV uploads. CustomerScore pulls billing data directly."

**Highlight:**
- "Don't have Stripe?" → that's when a demo call kicks in
- Everything else is self-serve

---

### 4. Stripe Data Loading (`v-stripe-loading`)

**What to show:** The 5-step progress animation.

**What to say:**
> "While Kate makes her coffee, we're pulling all her customers — active and churned — their MRR, plan, renewal date, add-ons. Everything."

Let the animation run to completion (4.8s). Auto-navigates to dashboard.

---

### 5. Main Dashboard — The First Reveal (`v-dashboard`)

**This is the money moment. Slow down here.**

#### 5a. AI Revenue Intelligence Cards (top row)

Pan/zoom across the 4 cards. Read them out loud:

- **Churn Risk:** "17 customers at risk. £1,840 MRR. 12 days to act."
- **NRR Gap:** "NRR is 87% — 13 points below benchmark."
- **Upsell Opportunity:** "94 accounts ready. £89K ARR potential."
- **Involuntary Churn:** "38% of churn is just failed cards. £1,180 recoverable today."

**What to say:**
> "Kate has been running her company for years and never saw this clearly. This is 5 minutes after connecting Stripe."

#### 5b. Core Metrics (KPI row)

- MRR: £38,400 ↓ £1,300
- NRR: 87%
- Churn Rate: 3.2% ↑ 0.4%

**What to say:**
> "The numbers Kate already knew — but now they're moving in the wrong direction and she can see exactly why."

#### 5c. MRR Stacked Bar Chart

Hover over the bars. Show Sep → Feb progression.

**What to say:**
> "Six months. You can see exactly when expansion MRR peaked and when churned MRR started to overtake new revenue. This is where it breaks."

#### 5d. Cohort Retention Heatmap

Briefly scroll to it.

**What to say:**
> "Month 3 is the cliff. Customers who don't engage by month 3 almost never stay. We see this in the cohort data instantly."

#### 5e. Downgrade → Churn Pipeline Table

Point to:
- Radix Labs (Pro → Mini, High risk)
- Luna Digital (Business → Pro, Med risk)
- Crisp CX (Pro → Mini, High risk)

**What to say:**
> "These three companies downgraded last month. CustomerScore flags them as pre-churn. £930 MRR on the line. Kate can act now — before they leave."

---

### 6. Segments (`v-segments`)

**What to do:** Click "Segments" in the left sidebar.

**What to show:** The 4 AI-suggested segment cards.

Walk through each card:
- **At-Risk Pro Users** (red) — 23 customers, £8,400 MRR, churn risk 73/100
- **Expansion Ready** (green) — 8 customers, £2,200 MRR, usage signals
- **New Users — Onboarding** (indigo) — 12 customers, just signed up
- **High-Value Stable** (gray) — 47 customers, £18,600 MRR, the ones to protect

**What to say:**
> "AI auto-created these segments from billing data alone. No tagging, no spreadsheet, no ops work. Kate didn't configure anything."

**Click into "At-Risk Pro Users"** to show detail.

---

### 7. Segment Detail (`v-segment-detail`)

**What to show:**
- Left panel: customer list with status chips (Draft / Sent / Opened)
- Right panel: conditions — "Plan = Pro", "Last login > 14d", "Churn risk = high"

**What to say:**
> "Here's the At-Risk segment. The conditions are AI-derived — not manually configured. Kate can see exactly which customers are in here and what triggered their risk flag."

---

### 8. Playbooks (`v-playbooks`)

**What to do:** Click "Agents" in the sidebar.

**What to show:** The 3 playbook cards.

Walk through:
- **Win-Back Campaign** (Active) — 23 customers, 3 already saved, £2,100 MRR recovered
- **Expansion Upsell** (Review Pending) — 8 drafts awaiting Kate's approval
- **Onboarding Nudge** (Active) — new user journey running

**What to say:**
> "CustomerScore doesn't just show you problems — it runs the fix. These playbooks are automated campaigns. The AI wrote the messages. Kate just approves."

---

### 9. Playbook Review — Draft Approval (`v-playbook-review`)

**What to do:** Click "Review Drafts →" on the Expansion Upsell card.

**What to show:**
- Left: customer list (Sarah Chen, Michael Torres, Sofia Martinez)
- Right: personalized email draft, ready to send

**What to say:**
> "AI wrote a personalized email for each customer. Kate can read it, revise it if she wants — or just hit Launch."

**Click "Launch Playbook" button.**

**What to say:**
> "One click. Done. 8 personalized emails — sent at the right moment to the right people. Kate spent 30 seconds."

---

### 10. Live Playbook Performance (`v-playbook-live`)

**What to show:** The Win-Back Campaign performance report.

Highlight:
- 3 customers saved
- £2,100 MRR recovered
- Goal progress bar: 65% toward target
- Results table: Sarah Chen (email opened), Michael Torres (clicked offer), Sofia Martinez (scheduled call)

**What to say:**
> "This is the Win-Back campaign that's already been running. Real results. Real money retained. The AI is learning from what works."

---

### 11. Connect Product Analytics — The Upgrade Moment (`v-connect-analytics`)

**What to do:** Click the "Connect engagement data" banner at the top of the dashboard.

**What to show:** Provider selection — PostHog and Mixpanel.

**What to say:**
> "Billing data alone gets you this far. But add product usage data — and the picture gets sharper."

Click PostHog → loading screen → event picker.

**On the event picker:**
> "AI found 8 high-signal events. Things like: `cancelled_clicked`, `billing_page_viewed`. These are the actions that predict churn — and CustomerScore finds them automatically."

**What to say:**
> "Once Kate connects PostHog, the model can tell her not just *who* is at risk — but *why*. And exactly when to reach out."

---

### 12. Closing — The Value Statement

**Come back to dashboard. Zoom out.**

**What to say:**
> "Kate came in knowing she had a churn problem. In 10 minutes, she has the numbers, the segments, the playbooks — and the first campaign is already running."

> "No spreadsheets. No ops. No sales call. Just: connect Stripe, see your revenue, fix it."

> "This is CustomerScore. Early access is opening soon."

Show sign-up CTA or cut to brand card.

---

## Key Talking Points to Thread Through

| Theme | Line to use |
|-------|-------------|
| Speed | "5 minutes from zero to first insight" |
| Zero config | "Kate fills in nothing. AI fills in everything." |
| Immediate value | "This is 10 minutes after connecting Stripe" |
| Low-touch at scale | "1,200 customers. Kate can't manage them all. Now she doesn't have to." |
| Billing + usage = full picture | "Billing shows you who left. Usage shows you why." |
| Revenue impact | "£2,100 recovered. 3 customers saved. One click." |

---

## Screen Sequence Summary

```
v-signup → v-analyzing → v-stripe → v-stripe-loading
  → v-dashboard (churn focus, all 5 sections)
  → v-segments → v-segment-detail (At-Risk Pro Users)
  → v-playbooks → v-playbook-review → v-playbook-live
  → v-dashboard (banner) → v-connect-analytics → events picker
  → back to dashboard (closing shot)
```

**Estimated recording time:** 4–6 minutes at a confident pace.

---

## Before Recording — Data Alignment Note

The app's current mock data uses GBP (£) and a 3.2% churn rate. Kate's story references EUR and 6.2% churn rate with 1,200 customers. Two options:

1. **Keep the app as-is** — narrate loosely, don't read numbers directly from screen
2. **Update mock data** to match Kate's story (EUR, 1,200 customers, ~60 churned/month, 6.2% rate) — small code change, makes the story airtight

**Recommendation: Option 2** if time allows — consistency between narration and on-screen data makes the demo land harder.
