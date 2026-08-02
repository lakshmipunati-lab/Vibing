Lovable preview URL:
https://finlit-daily-quest.lovable.app/learn

Deployed prod URL (if any):
Same as above unless you've published a separate custom domain — Lovable's "Publish" step can point to a distinct prod URL if you've set one up. Confirm and swap in here if so.

Export instructions:

1. In Lovable, open the FinLit project.
2. Go to Settings → GitHub → Connect (if not already connected) and
   select or create the target repo.
3. Once connected, Lovable auto-commits changes from the visual editor
   directly to the repo's src/ — no manual export step needed for
   ongoing changes.
4. To pull the latest state into a local clone:
   git pull origin main
5. If you've made local edits in Cursor/VS Code and want them reflected
   back in Lovable, push to the connected branch — the 2-way sync picks
   up commits and replays them in the Lovable editor.
6. Conflicts: if both sides changed the same file, Lovable's sync UI
   will flag it — resolve in the Lovable editor before continuing to
   avoid silently overwriting either side's changes.

Key flow screenshots

Drop these five into screenshots/ and link them — they're the ones we actually walked through and verified:

screenshots/home-dashboard.png — Total Balance, Health Score, Accounts row, Activity feed
screenshots/store.png — Reward store with points-to-go progress
screenshots/coupons-empty.png — Empty state routing back to Store
screenshots/streak.png — Day streak, calendar, weekly leaderboard link
screenshots/profile.png — Stats, badges, settings (including the "Learning Goal: Not set" bug worth flagging in a comment)
