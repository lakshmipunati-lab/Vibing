Prompt

Establish the FinLit design system:

Colors
- Primary/brand: teal (#0FB5A5) — used for the FinLit Bank logo, primary
  CTAs, Checking account, active nav states, and completed-lesson checkmarks
- Background: warm off-white/cream (not pure white) — matches a
  friendly-but-credible fintech feel, not a cold corporate one
- Account-type accent colors (color-code each account so they're
  instantly distinguishable in the Accounts row):
  - Checking: teal
  - Savings: amber/gold (piggy bank icon)
  - Credit Card: coral/red
- Status colors: success/teal for completed and positive score movement,
  muted gray for locked states, warm coral for streak/urgency elements
- Text: dark charcoal for primary content, muted gray for secondary
  labels and locked-state copy

Typography
- Bold, rounded sans-serif for headings and numbers (balance, XP, streak
  count) — numbers should feel substantial, this is the "money" in the app
- Regular weight for body copy and lesson descriptions
- All lowercase/sentence case for labels — no ALL CAPS except small
  section eyebrows like "TOTAL BALANCE" or "YOUR ACCOUNTS"

Spacing & components
- Card-based layout throughout — rounded corners (12-16px radius) on
  every card: balance card, account tiles, activity feed rows
- Generous padding inside cards so numbers and icons don't feel cramped
- Bottom tab navigation, 5 items, active state uses filled icon + teal
  label vs. outline icon + gray label for inactive
- Progress bars use the same teal-on-light-track pattern everywhere
  (Checking balance progress, Store reward "X to go")

Reference: this should read as a real bank app first, gamified education
app second — closer to a fintech product's visual confidence than a kids'
learning app's playfulness.

Notes

Tokens confirmed real: 
#0FB5A5 teal is the actual theme color pulled from the live deployment's metadata — safe to treat as the source-of-truth brand token, not a guess.
Tokens inferred from screenshots, not confirmed as exact hex: the cream background, amber/gold Savings accent, and coral/red Credit Card accent are visually accurate from the screens we reviewed but weren't extracted as literal values — worth pulling exact hex codes from your Lovable project's Tailwind config or CSS variables before treating these as locked tokens.
Consistency reference for an agent: point future styling prompts at the existing Home dashboard screen as the canonical reference rather than re-describing the system each time — it's the most complete, most-tested screen and keeps new components visually consistent with it.
Accessibility note: the account-type color-coding (teal/amber/coral) is a nice-to-have distinguisher but shouldn't be the only signal — confirm each account tile also differs by icon and label text, not color alone, for colorblind users.
