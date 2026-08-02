# Context

> This is the briefing layer — what's missing from most PM repos and what makes an agent's summary go from "appears to be a dashboard" to "this is a B2B retention tool for PMs at series-B SaaS companies." The agent uses this to disambiguate everything else. Skip it and the agent guesses.

## Company / Product Context

FinLit lives in the space of consumer financial-literacy apps — a category where most products succeed at teaching content but fail at getting people to come back. This project maps to Scenario 1: The Retention Engine from the course brief: a product losing users before real behavior change happens, where the data confirms churn but not the "why."

The user is an adult with little to no financial literacy — budgeting, credit, saving, basic investing — who wants to build money skills in short daily sessions (2–5 minutes). This person is the most likely to download a finance app, complete one lesson, and never return, because the app feels disconnected from their actual financial life.

Why this matters now: Duolingo-style gamification (streaks, XP, bite-sized lessons) is proven for retention in language learning, but it hasn't been tested as a *banking metaphor* specifically. The bet here isn't "does gamification work" — that's established — it's whether reframing the entire interface as a bank (balance, accounts, transactions) outperforms a generic gamified lesson list for this specific use case, where the subject matter (money) already has a natural, familiar visual language to borrow from.

## Decisions Log

- **Decision:** Frame the product around Scenario 1 (Retention Engine) rather than Scenario 2 or 4 (Usability scenarios).
  **Why:** The core problem — users abandoning before a habit forms — is fundamentally a retention problem, not an adoption or comprehension problem. Duolingo-style mechanics are themselves a retention intervention, so the scenario and the solution approach are naturally aligned.

- **Decision:** Reframe the learning path as a bank dashboard instead of a traditional winding lesson path.
  **Why:** A generic lesson path is what every Duolingo-style app already looks like — it doesn't differentiate or test anything new. A bank framing makes the app *feel* like the thing it's teaching about, and turns the retention question into something concretely testable: does familiarity beat generic gamification.

- **Decision:** Split "points" (spendable, used in Store) from "XP" (permanent, tied to Level) as two separate values rather than one combined score.
  **Why:** A single number can't behave like both a spendable balance and a permanent progress metric — spending points would have had to either fake-not-decrease XP or break the bank metaphor entirely. Splitting them preserves both the "balance" feeling and genuine progress tracking.

- **Decision:** Gate account unlocks (Savings, Credit Card) behind lesson-count thresholds rather than unlocking everything upfront or via a flat time-based schedule.
  **Why:** Mirrors how a real bank relationship deepens with engagement, and gives the product a natural expanding-scope narrative tied directly to the retention metric (lessons completed), not an arbitrary calendar gate.

- **Decision:** Use Supabase as the backend with no external API integrations for this phase.
  **Why:** The hypothesis under test is about interface/metaphor, not real financial data — adding real banking APIs would add integration risk without adding evidence toward the actual question being tested.

## What I Tried That Didn't Work

- Considered a straight "internal tool" framing (Scenario 2) early on, treating low engagement as a usability problem — dropped this because financial literacy apps aren't really analogous to an internal CRM nobody adopted; the friction isn't unfamiliarity with the tool, it's lack of a reason to return, which is a retention problem, not a workflow-fit problem.
- Explored other bank-concept directions before settling on the dashboard-plus-accounts approach — including a "bank teller/mission" narrative concept and a full spend-and-invest sandbox. Both were more ambitious but added significant build scope without directly testing the core retention hypothesis faster, so they were deferred rather than built for this phase.

## Open Questions

- Does the bank-dashboard framing actually change Day-7 retention behavior, or does it just look different while producing the same return rate as a generic lesson list? This is unmeasured — no A/B comparison has been run yet.
- Does the streak logic correctly increment and reset across real, multi-day usage (not just a single test session)? This is the single highest-priority unresolved technical question — see `architecture.md` and `handoff.md`.
- Is the "Learning Goal" field in Profile actually persisting from onboarding, or is there a silent write failure? Observed bug, not yet root-caused.
- Does the bank metaphor read as motivating, or as gimmicky/confusing to a first-time user with no context on why a learning app looks like a bank? Untested with real users outside this build process.
- Should account unlock thresholds be recalibrated once real usage data exists, or are the current thresholds (e.g. "complete 2 more lessons") arbitrary placeholders that need tuning against actual drop-off points?
