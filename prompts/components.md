Prompt

Build the Home dashboard component for FinLit. It needs:

1. A Total Balance card — shows point balance, "X available · Y in savings",
   and a circular Financial Health Score gauge (0-850 range, label changes
   with score: "Building" / "Good" / "Excellent")
2. An Accounts row — horizontally scrollable cards for Checking, Savings,
   Credit Card. Checking starts unlocked with a progress bar. Savings and
   Credit Card start locked with a lock icon and "Complete N more lessons
   to open" — N is dynamic, calculated from remaining lessons until that
   account's threshold.
3. An Activity feed — each lesson renders as one of three states:
   - Completed: checkmark icon, lesson name, "+[pts]" in success color
   - Pending (next available lesson): highlighted row, "Tap to deposit" CTA
   - Locked: lock icon, muted text, "Complete previous lesson to deposit"
   Feed order always matches lesson sequence — never let a later lesson
   show as Pending while an earlier one is still Locked.

Handle empty state: a brand-new user with zero completed lessons should
still see the full layout (Balance card at 0, all locked/pending states
correct) — never a blank or broken screen.

Notes

Component intent — the Home dashboard is the anchor screen; every other screen (Lesson, Store, Streak, Profile) reads from or writes back to the same balance/streak state it displays, so it needs to re-render correctly on return from any of them, not just on first load.
States to handle:
Empty — zero-progress new user (confirmed working — welcome bonus makes true zero-state rare, but worth testing with $0 balance explicitly)
Loading — Supabase fetch delay on initial load; unverified whether there's a skeleton/spinner state or a flash of empty content
Error — Supabase write failure on lesson completion (e.g. network drop mid-deposit); unverified — worth explicitly testing whether a failed write leaves the UI showing "Completed" while the database still shows "Pending," since that mismatch would break trust in the balance number
Accessibility considerations: lock icons and progress bars need text alternatives, not just icon-only state (confirmed text labels exist, e.g. "Complete 2 more lessons to open" — good default). Color alone shouldn't carry meaning for Completed vs. Pending vs. Locked rows — worth confirming there's an icon or label difference beyond just color, not just relying on the teal/gray/lock distinction being visible to colorblind users.
