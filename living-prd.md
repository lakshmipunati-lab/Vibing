# Living PRD

> Generated in Lovable via the M4 PRD-extraction prompt, then committed here. Updated through M5 and M6. This is the product spec — keep it honest, especially Section 5.

## 1. Product Overview

FinLit is a Duolingo-style financial literacy app reframed as a bank dashboard. Instead of a generic winding lesson path, users see a real bank-style home screen — total balance, a Financial Health Score, and multiple accounts (Checking, Savings, Credit Card) that unlock as lessons are completed. Every lesson appears as a transaction to be deposited, not a node to be checked off, testing whether a familiar banking interface improves the habit-formation that most financial literacy apps fail to create.

## 2. Problem & Hypothesis

- **Problem:** Financial literacy apps lose most users before any real habit or knowledge gain forms — people download, complete one lesson, and never return. This mirrors Scenario 1 (The Retention Engine): the data confirms churn, but not the "why," and no intervention has been tested yet.
- **Hypothesis being tested:** A bank-dashboard framing (balance, unlockable accounts, transaction-style lessons) will improve Day-7 retention and streak continuation compared to a standard gamified lesson-path format.
- **Riskiest assumption (from M2):** Users will perceive the bank framing as motivating and familiar rather than confusing or gimmicky. If the metaphor doesn't land — if users can't tell what's a lesson vs. a real financial feature, or find it novelty-driven rather than genuinely stickier — the core premise collapses, because the bet isn't "gamification works" (proven), it's "banking-as-metaphor outperforms generic gamification" (untested).

## 3. User Flows & Screen Map

Full detail and diagram in [`user-flow-canvas.md`](user-flow-canvas.md). Screenshots in [`prototypes/screenshots/`](prototypes/screenshots/).

- **Onboarding → Home:** goal selection → welcome bonus (+50 pts) → lands on Home dashboard
- **Home (bank dashboard):** Total Balance card (points, health score) → Accounts row (Checking unlocked, Savings/Credit Card locked with progress-gated unlock messages) → Activity feed (pending/locked/completed lessons as transactions)
- **Activity feed → Lesson:** tap a "Pending" transaction → complete lesson → returns to Home with balance updated, feed entry flips to "Completed: [lesson] +[pts]," next lesson flips from Locked to Pending
- **Account unlock event:** once lesson-count threshold is met, a locked account (e.g. Savings) flips to unlocked and joins the deposit flow
- **Bottom nav (persistent):** Home / Store (redeem points for real-world rewards) / Coupons (redeemed rewards, empty state routes back to Store) / Streak (streak count, calendar, weekly leaderboard) / Profile (XP, lessons done, badges, settings)

## 4. Success Metrics

- **Primary metric:** Day-7 retention rate — percentage of users who return and complete at least one lesson 7 days after signup.
- **Guardrail metrics:** Day-1 → Day-2 return rate, average streak length before break, percentage of users who unlock a second account (Savings), lesson completion rate within the first session (should stay high — if the bank framing adds friction rather than motivation, this would be the first place it shows).
- **Target:** No baseline yet — this build hasn't run against real multi-day user data. First target should be beating the informal industry pattern referenced in the brief (most finance apps see steep drop-off by Day-3), with a stretch goal of a formal A/B comparison against a plain-lesson-list control if time allows.

## 5. Technical Reality (real vs. mocked — be honest)

| Feature | Status (Real / Mocked) | Notes |
|---------|------------------------|-------|
| Lesson completion → balance/XP update | Real | Confirmed live via Supabase across full walkthrough |
| Sequential lesson locking | Real | Enforced in the Activity feed; unverified whether it's also enforced at the URL/route level |
| Account unlock thresholds | Real | Dynamic, tied to completed-lesson count, not hardcoded |
| Streak increment (Day 1) | Real | Confirmed on first session |
| Streak reset on missed day | Unverified | Logic exists but has not been tested against a real day boundary — highest-priority open item |
| Points vs. XP split | Real | Two distinct values confirmed — points are spendable (Store), XP is permanent (Level) |
| Store reward browsing | Real | "X to go" progress shown per reward, tied to real balance |
| Store redemption (deduction) | Unverified | Unclear if completing a "purchase" actually deducts from the Supabase balance |
| Coupons empty state | Real | Correctly routes back to Store rather than showing a dead screen |
| Learning Goal persistence | Broken/Mocked | Shows "Not set" in Profile despite being part of onboarding — likely a silent write failure |
| Push notifications | Mocked | In-app toggle exists; delivery mechanism explicitly not built yet |
| Weekly leaderboard | Unverified | Link exists on Streak page, not yet walked through |
| External financial APIs | Not integrated | By design — all balances/accounts/transactions are simulated, no real banking data source |

## 6. Assumptions & Risks

- **Assumptions:**
  - Users will understand the bank metaphor without additional explanation beyond the onboarding flow
  - A gamified financial-education product can ethically use bank-like language ("balance," "deposit," "credit score") without misleading users into thinking real money or credit is involved
  - Lesson-count-based unlock thresholds are a reasonable proxy for engagement depth (not yet tuned against real drop-off data)

- **Risks:**
  - The core retention hypothesis is entirely unproven — everything confirmed so far is build-correctness, not user-behavior evidence
  - Streak logic, the single mechanic the hypothesis depends on most, is unverified past Day 1
  - If users find the bank framing confusing rather than motivating, the product could underperform a plain lesson-list app despite looking more polished
  - No real user testing has occurred yet — all findings to date come from a single builder's walkthrough, not independent users

## 7. Scope

- **In:** Bank-style home dashboard, points/balance system, XP/Level system, Financial Health Score, account unlock logic gated by lesson count, transaction-feed lesson list, streak counter and calendar, Store (reward browsing), Coupons (with empty state), Profile (stats and settings), Supabase backend
- **Out:** Real financial data integration, actual payments, full curriculum beyond the first unit, social/leaderboard features beyond the teased link, formal push notification delivery, A/B test infrastructure
- **Phased / later:** Multi-day streak validation and hardening (immediate next step), real user testing against a retention baseline, expanded curriculum across additional units and account types, push notifications, formal leaderboard/social features, potential real financial-data integrations if the metaphor proves out and the product moves toward real account-linking

## 8. Engineering Recommendation

Build and verify the streak increment/reset logic against real Supabase timestamps across actual multiple days first, before any further feature work. This is backend logic tied to timezone handling and date-boundary math, not UI, and it's the one mechanic the entire product hypothesis depends on that cannot be validated by clicking through the app once in a single sitting. Every other gap in Section 5 (Learning Goal persistence, Store redemption deduction, locked-lesson URL guardrails) is a straightforward fix; the streak logic is the one piece of technical debt that, if wrong, invalidates the retention metric this whole prototype exists to test.
