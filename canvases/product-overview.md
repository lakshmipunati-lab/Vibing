# FinLit — Product Canvas

## The pitch
A Duolingo-style financial literacy app reframed as a bank dashboard. Lessons appear as transactions to deposit, not nodes on a path — completing them unlocks real-feeling accounts (Checking → Savings → Credit Card).

## The user
Adults with little to no financial literacy who want to build money skills in short daily sessions (2–5 min) — the group most likely to download a finance app, do one lesson, and never return.

## The problem
Financial literacy apps lose most users before any habit or knowledge gain forms. This mirrors Scenario 1: The Retention Engine — a product bleeding users in the first 90 days, where the data confirms churn but not the "why."

## The bet
A bank-dashboard framing — balance, health score, unlockable accounts, transaction feed — will improve Day-7 retention and streak continuation compared to a standard gamified lesson list. The open question: does familiarity with a bank UI actually change return behavior, or is it just a cosmetic skin on a proven mechanic?

## Core loop
Onboarding → goal selection → welcome bonus → Home dashboard → tap pending lesson → complete lesson → balance/streak update → next lesson unlocks → (at threshold) new account unlocks

## Status right now
| | |
|---|---|
| **Live URL** | https://finlit-daily-quest.lovable.app/learn |
| **Backend** | Supabase — no external APIs |
| **Confirmed working** | Balance, XP/level, health score, sequential lesson unlocking, account unlock thresholds, transaction feed, Store redemption UI, Coupons empty state, Day-1 streak |
| **Unverified** | Multi-day streak reset/increment, push notifications, weekly leaderboard, Learning Goal persistence |
| **#1 risk** | Streak logic has only been tested on Day 1 — the entire retention hypothesis depends on this working correctly across real day boundaries |
