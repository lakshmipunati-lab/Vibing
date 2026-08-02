# Engineering Handoff Note

| Field | Value |
| --- | --- |
| **Project Name** | FinLit |
| **Confidence Line Status** | Right Side (Production Hardening) — core loop built and deployed, but streak logic and a few states are unverified past a single session |
| **Prototype URL** | https://finlit-daily-quest.lovable.app/learn |
| **Living PRD Link** | [`living-prd.md`](living-prd.md) |
| **Date** | August 1, 2026 |
| **PM / Builder** | [Insert Name] |

---

## 1. The Core Intent

**Validated Assumption:**
> "Reframing financial-literacy lessons as a bank dashboard — balance, unlockable accounts, a transaction feed — produces a working, coherent gamified loop that reads as a bank app rather than a generic lesson list." Confirmed through direct screen walkthrough: balance, health score, sequential lesson unlocking, and account-unlock thresholds all update correctly and consistently.

**Not yet validated:**
> Whether this framing actually improves Day-7 retention compared to a standard lesson-path version — that's the real product hypothesis (Scenario 1: The Retention Engine) and it requires multi-day usage data, not just a build-correctness check.

**Critical Path:**
> The lesson-completion → balance/streak update → next-unlock loop is the single most-used and most load-bearing flow in the app. Every other screen (Store, Streak, Profile) is downstream of the state this loop writes. If this breaks, the entire retention mechanic breaks with it — this is the flow to protect most carefully during any refactor.

---

## 2. Implementation Status: Real vs. "Vibed"

| Feature | Status | Engineering Notes |
| --- | --- | --- |
| Authentication | Mocked/Unconfirmed | Not directly tested in this walkthrough — confirm whether Supabase Auth is wired up or if the app is running on a single implicit session. |
| Data Storage | Real | Supabase Postgres backing balance, XP, streak, lesson progress, and account-unlock state — confirmed persisting across screens. |
| External APIs | None | No third-party integrations by design — all financial data is simulated. Nothing to harden here for this phase. |
| Streak Logic | Real, unverified at scale | Increment logic works on Day 1. Reset-on-missed-day logic exists but has not been tested against real multi-day timestamps — see Section 3. |
| Styling / UI | Production-ish | Consistent card-based design system across screens (see `prompts/design-system.md`), but accessibility (color-only status distinctions, screen-reader labels on lock/progress icons) has not been audited. |
| Learning Goal field | Broken/Mocked | Profile shows "Not set" despite being part of onboarding — likely a silent write failure or field/table mismatch. |
| Store Redemption | Unconfirmed | Unclear whether "purchasing" a reward actually deducts from the real balance in Supabase or is currently a display-only interaction. |

---

## 3. Known "Hacks" and Technical Debt

**Edge Cases Not Handled (or unconfirmed):**
> - No confirmed behavior for a user directly navigating to a locked lesson's URL (`/lesson/:lessonId`) — may bypass the sequential-unlock guardrail entirely.
> - No confirmed timezone handling for streak day-boundary calculation — a user in a non-UTC timezone could see incorrect increments or resets.
> - No confirmed behavior when a lesson is answered entirely incorrectly — unclear whether the Health Score ever decreases, or whether it's a one-directional (always-increasing) value, which would misrepresent the "real bank" metaphor it's built on.
> - "Reset Progress" button in Profile has not been tested — unconfirmed whether it cleanly clears all related Supabase rows or leaves orphaned data across tables.

**Spaghetti Logic:**
> Not directly observable from the UI alone — recommend an engineer review the actual component/state code before assuming cleanliness. Given the number of interdependent states (balance, XP, streak, unlocks, feed status) all needing to stay in sync on Home, this is a likely candidate for scattered state updates rather than a single source of truth — worth checking whether Home computes derived state consistently or duplicates logic per-component.

**Performance:**
> Not yet a concern at current content scale (1 unit, ~9 lessons observed). Will need revisiting once the curriculum expands — the Activity feed rendering every lesson as an individual row will need virtualization or pagination once units scale beyond a handful.

---

## 4. Integration Requirements (Existing Products)

Not applicable — this is a 0-to-1 prototype, not a feature addition to an existing product.

---

## 5. Success Criteria for Production

- [ ] **Correctness:** Streak increment/reset logic verified against real multi-day Supabase timestamps, including timezone edge cases
- [ ] **Data integrity:** Confirm `learning_goal` writes correctly from onboarding into the `users` table
- [ ] **Data integrity:** Confirm Store redemptions correctly deduct from the real balance, not just visually decrement
- [ ] **Security:** Guardrail confirmed — locked lessons cannot be reached directly via URL manipulation
- [ ] **Accuracy:** Account-unlock thresholds and balance/XP math match the logic documented in `living-prd.md`
- [ ] **Polish:** Loading state defined for initial Supabase fetch on Home (currently unconfirmed whether there's a skeleton or a content flash)
- [ ] **Accessibility:** Lock/Completed/Pending states confirmed distinguishable by more than color alone

---

## 6. Suggested Architecture (extracted from M4 Refactor)

```
/src
  /auth          -- confirm whether this exists yet; not verified in this prototype
  /components
    /home        -- balance card, accounts row, activity feed
    /lesson       -- lesson flow, question types, completion screen
    /store        -- reward store, redemption logic
    /streak       -- streak calendar, leaderboard
    /profile      -- stats, settings
  /services
    /supabase.ts  -- Supabase client + queries (balance, progress, streak, accounts)
  /utils
    /streak.ts    -- date/timezone logic for increment vs. reset (highest-priority file to review)
```

**Graduation path:**
> The prototype currently mixes read/write Supabase calls directly inside screen components (typical of a fast Lovable build). Recommended graduation path: extract all Supabase queries into a dedicated `/services/supabase.ts` (or split per-entity: `lessons.ts`, `streaks.ts`, `accounts.ts`) so streak logic in particular can be unit-tested in isolation from the UI before it's trusted in production — this is the piece the whole retention hypothesis depends on, and it's currently the least testable in its likely current form.
