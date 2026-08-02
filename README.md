# My Vibe Coding Project

> One product, built across six modules. By the end of the certification this repo IS your product — version-controlled, agent-readable, and portable. A teammate should be able to clone it, open it in any AI tool, and have the agent brief them on the product *without you in the room*.

---

## At a Glance

- **One-sentence pitch:** FinLit is a Duolingo-style financial literacy app reframed as a bank dashboard — lessons appear as transactions to be deposited, and completing them unlocks new accounts (Checking → Savings → Credit Card) — built to test whether a familiar banking metaphor improves early user retention over a generic gamified lesson list.
- **Status (Confidence Line):** `M1 ✅ · M2 ✅ · M3 ✅ · M4 ✅ · M5 ✅ · M6 ⬜`
  - What's real: onboarding, welcome bonus, balance/points system, XP/level system (tracked separately from points), Financial Health Score, account unlock logic gated by lesson count, sequential lesson locking, transaction-feed activity list, streak counter + calendar, Store (points redemption toward real-world rewards), Coupons (with working empty state), Profile stats — all backed by Supabase.
  - What's mocked or unverified: multi-day streak reset/increment behavior (only tested on Day 1), push notification delivery (in-app toggle exists, no real notifications sent), weekly leaderboard (teased, not yet verified), Learning Goal persistence in Profile settings (showing "Not set" — likely a bug), whether Store redemptions actually deduct from balance or are display-only.
  - No external APIs are connected — all financial data is simulated by design.
- **Live URLs:**
  - Lovable preview: https://finlit-daily-quest.lovable.app/learn
  - Deployed prod: _same as above, unless separately published_
  - Supabase project: _[add your Supabase project link here]_

### Where to read next
- **For PMs** → [`living-prd.md`](living-prd.md)
- **For engineers** → [`handoff.md`](handoff.md)
- **For background** → [`context.md`](context.md)
- **Quick visual briefs** → [`canvas.md`](canvas.md) · [`architecture.md`](architecture.md) · [`user-flow.md`](user-flow.md)
- **Prompt history** → [`prompts/`](prompts/) — Components, Data Model, Design System, Structure

---

## Module Progress

| Module | Phase | Status | Key Artifact |
|--------|-------|--------|--------------|
| **M1** | Activate | [x] | Scenario 1: The Retention Engine — chosen and reframed around financial-literacy churn |
| **M2** | Validate | [x] | Riskiest-assumption test: does the bank-dashboard framing outperform generic gamification on Day-7 retention? |
| **M3** | Direct | [x] | [`prompts/`](prompts/) — Living Prompt Pack (Components, Data Model, Design System, Structure) — swap in real Lovable prompt history where it differs from the reconstructed drafts |
| **M4** | Structure | [x] | [`living-prd.md`](living-prd.md) + [`handoff.md`](handoff.md) + [`context.md`](context.md) — drafted and cross-linked, pending a final review pass against the real Supabase schema |
| **M5** | Ship | [x] | Live deployment at the Lovable URL above; core loop (onboarding → lesson → balance update → unlock) confirmed working via screen walkthrough |
| **M6** | Measure | [ ] | Not started — blocked on multi-day streak testing (see `handoff.md` "Start Here" priority) |

---

## How to use this template

1. Click **"Use this template" → "Create a new repository"** (do not fork — generate your own copy).
2. Name it something like `my-vibe-coding-project` and open it in Cursor, VS Code, or GitHub's web editor.
3. Fill in `README.md`, `context.md`, and the module artifacts as you progress.
4. Keep `src/` synced from Lovable via the Lovable ↔ GitHub 2-way sync.

> **Naming rules:** kebab-case for markdown (`living-prd.md`), no spaces in filenames (AI tools URL-encode them and links break).
