Prompt

Set up the Supabase data model for FinLit with these entities:

users
- id (uuid, pk)
- display_name (text)
- learning_goal (text, nullable)
- created_at (timestamp)

lessons
- id (uuid, pk)
- unit_id (uuid, fk -> units)
- title (text)
- order_index (int)
- xp_reward (int)
- points_reward (int)

units
- id (uuid, pk)
- title (text)
- order_index (int)
- account_type (text) — which account this unit's completion unlocks
  (e.g. "savings", "credit_card"), nullable for units that don't unlock anything

user_progress
- id (uuid, pk)
- user_id (uuid, fk -> users)
- lesson_id (uuid, fk -> lessons)
- completed_at (timestamp, nullable) — null means in-progress/not started
- score (int, nullable)

user_accounts
- id (uuid, pk)
- user_id (uuid, fk -> users)
- account_type (text) — "checking" | "savings" | "credit_card"
- unlocked_at (timestamp, nullable) — null means still locked

user_streaks
- user_id (uuid, pk, fk -> users)
- current_streak (int)
- longest_streak (int)
- last_completed_date (date) — used to calculate increment vs. reset

Relationships: one user has many user_progress rows (one per lesson),
one user has many user_accounts rows (one per account type), one user
has exactly one user_streaks row. Lessons belong to units; units may
gate an account_type unlock once all their lessons are completed.

Notes

Real vs. mocked data: users, lessons, units, user_progress, user_accounts are confirmed live and reflected correctly in the UI walkthrough. user_streaks exists and shows a real Day-1 value, but the increment/reset math against last_completed_date has not been tested across an actual day boundary — this is the single highest-risk table in the schema.
Known data issue: learning_goal on users is likely not writing correctly — Profile showed "Not set" despite the field being part of onboarding. Worth checking whether the onboarding write is failing silently or writing to the wrong field/table entirely.
Edge cases to verify against the real schema:
What happens to user_progress if a user answers every question wrong — does score get written as 0, or does the row fail to insert at all?
Does last_completed_date use UTC or the user's local timezone? This directly determines whether the streak resets correctly for users outside UTC.
Is there a unique constraint on (user_id, lesson_id) in user_progress to prevent duplicate completions from double-counting points?
Does unlocked_at get set exactly once, or could a race condition (e.g. rapid lesson completions) double-fire the unlock logic?
No external APIs — this is the complete data surface; nothing here is sourced from or synced with a real financial institution.
