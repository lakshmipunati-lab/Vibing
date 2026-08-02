Prompt

Set up the FinLit app structure and routing:

Pages
- /learn (Home) — the default/index route, bank dashboard with balance,
  accounts, and activity feed
- /lesson/:lessonId — individual lesson flow, only reachable from a
  Pending or Completed row in the Home activity feed
- /store — reward redemption
- /coupons — redeemed rewards, with an empty state when none exist
- /streak — streak detail, calendar, and weekly leaderboard link
- /profile — user stats and settings

Navigation
- Persistent bottom tab bar on every top-level page (Home, Store,
  Coupons, Streak, Profile) — always visible, active tab highlighted
- Lesson flow is NOT part of the bottom nav — it's a full-screen modal-
  style flow entered from Home and exited back to Home on completion
  or cancel, keeping the nav bar out of the way during a lesson

Guardrails
- /lesson/:lessonId should redirect back to /learn if the requested
  lesson is still Locked (i.e. its prerequisite isn't completed) —
  never let a user reach a locked lesson directly via URL
- New/unauthenticated users landing on any route other than onboarding
  should be redirected into the onboarding flow first

Notes

Routing decisions: Home (/learn) is deliberately the index route, not a separate /onboarding landing page after first run — this keeps the bank metaphor front-and-center every time the app opens, rather than routing back through a marketing-style splash.
What connects to what: every top-level nav page reads from the same underlying Supabase state (balance, streak, XP) as Home, so none of them should be treated as isolated screens — a change to points in Store should be reflected on Home without a manual refresh.
Unverified guardrail: whether the "redirect if locked lesson" rule is actually enforced, or whether typing a locked lesson's URL directly would let a user bypass the sequential-unlock logic — worth testing directly, since this is a resilience gap the original brief specifically calls out (handling unpredictable user input, not just the happy path).
Unverified guardrail: whether an unauthenticated or fresh session correctly redirects into onboarding, or whether it's possible to land on /learn with a broken/empty state before onboarding has run.
