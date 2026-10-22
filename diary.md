# Diary

## 2026-10-22

Read through the repo first. It is a single `index.html` (vanilla JS, inline CSS, no build step). It is a mobile-first push/pull workout log that stores everything in `localStorage` under `pushpull-log-v1`.

### Added to `index.html`
- **Add exercise to a workout**
  - A grouped dropdown above "Finish session": Chest / Shoulders / Arms / Back / Misc, with the movements listed under each heading.
  - Added exercises apply to the current workout only. They have a "Remove from this workout" link and are cleared when the session is finished.
  - About 20 new movements: barbell bench, dips, push-ups, skullcrusher, close-grip bench, barbell OHP, barbell and T-bar rows, pull-up, incline curl, wrist and reverse curl, shrug, leg curl, RDL, and abs (cable crunch, hanging leg raise, ab wheel).
  - New muscle groups: forearms, traps, abs. They also appear in the per-slot swap dropdowns.
- **Weight tab**
  - A third nav button next to History.
  - Log date and kg, one entry per day (same date replaces).
  - SVG line chart of the last 30 entries, list with change vs previous entry, and Edit/Delete per entry.
  - Weigh-ins are included in Export/Import backup.
- **Edit past workouts**
  - An Edit button on each History entry opens an edit view.
  - Edits: date, kg/reps, add/remove sets, remove exercises, add exercises via the grouped dropdown.
  - Saving re-sorts history by date and resets prefilled draft weights.

### State changes
- New fields `extras` and `weights` in the saved state. Old data loads fine because `load()` merges over `defaultState()`.

### Checks
- `node --check` on the extracted script passed.
- Stubbed-DOM smoke test covered the add, finish, edit and weight paths and passed.
- Ran a local server at http://127.0.0.1:8000 and Erik confirmed it looked good.
- Not checked on a real phone by me.

### Git
- Commit `8de5ba0` "Add exercise picker, body weight tracking, and workout editing", pushed to `origin/main`.

### Notes
- The auto-mode classifier blocked binding the dev server to all interfaces, so it ran on localhost only. Phone testing over wifi would need a manual `python3 -m http.server 8000` or a permission rule.
- Data lives in the browser, per origin. Keeping the same URL keeps history. Changing URL or host, deleting the home-screen icon, or clearing Safari data wipes it. Use Export backup before moving anything.

### Later the same day
- **Weight tab: 7-day rolling average.** Mean of all weigh-ins in the 7 calendar days ending on each entry's date. Shown as a thick line over the raw weight line, a "7-day average" summary above the chart, and per row in the list.
- **Analyze tab** (fourth nav button). Pick any logged exercise (grouped dropdown) and a metric: Strength, Top weight, Volume, Total reps. Line chart over sessions, stats (Latest, Best, Change since first with %, Sessions), and a session list.
  - **Strength** is the default. It is an estimated 1RM (Epley: weight × (1 + reps / 30)) from the best set, so one number blends weight and reps.
  - **Overall** (first dropdown option): per-exercise est. 1RM as % of its first session, averaged per workout into a strength index (100 = start). Only exercises with weight logged in 2+ sessions count. Per-exercise % change list below.
- Header now wraps to two rows on narrow screens because of the fourth nav button.
- Tested with stubbed-DOM scripts only, not in a real browser.
