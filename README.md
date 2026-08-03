# Move planner

A single-file web app for tracking a move: a checklist grouped by track
(Prep, House, Storage, Mom's), a scrollable gantt-style timeline, a
countdown to a date you choose, a Houses tab that can sync live with
a Google Sheet, and a Move tab with the full apartment/garage/storage
packing order. Checking off a task updates its timeline bar. Progress is
saved in the browser's local storage, per device, and mirrored to Google
Sheets if you connect it (Houses and the main Checklist only — the Move
tab is local-only, see below).

## Move tab

A separate, self-contained checklist for the physical packing order —
nine phases from "Phase 0" (disinfect before anything goes in) through
the garage/shop setup, five waves sorted least-needed-first, the
storage/San Antonio runs, and a final trip that ends with the mattress.
Tap any row to check it off; each phase shows its own mini progress bar
and count, and tapping a phase title collapses it so you can focus on
what's current. "Reset all" clears just this tab for your next move.
It saves to its own local storage key (`move_planner_move_v1`) — separate
from the main Checklist tab and not synced to Google Sheets, since it's
a one-time packing sequence rather than an ongoing tracked list.

## Deploy with GitHub Pages

1. Create a new repository on GitHub (public, for the free Pages tier).
2. Upload **all the files in this folder** to the repo — `index.html`
   plus the icon files (`icon-192.png`, `icon-512.png`,
   `apple-touch-icon.png`, `manifest.webmanifest`). Drag and drop on
   github.com works fine, or `git add` / `git commit` / `git push`. They
   need to sit next to `index.html` (same folder) for the icons to load.
3. In the repo, go to Settings -> Pages.
4. Under "Build and deployment", set Source to "Deploy from a branch",
   branch to `main` (or `master`), folder to `/ (root)`, then Save.
5. GitHub gives you a URL like `https://alexbuildswood.github.io/<repo>/`
   within a minute or two. Open it on your phone and add it to your
   home screen — it'll use the checklist/moving-box icon and open in its
   own window like an app.

No build step, no server, no dependencies to install.

## Connecting the Houses tab to Google Sheets

The Houses tab reads your Google Sheet's `Houses` tab, and syncs the
Checklist tab's `Done` column both ways with the app's own checklist.
This only works once the app is hosted online (step above) — a Google
sign-in popup can't authenticate against a file opened straight from
your computer.

You only need to do this setup once. It requires a free Google Cloud
project — that sounds heavier than it is; it's about 10 minutes.

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
   and create a new project (top-left project picker -> New Project).
   Any name is fine.
2. In the search bar, search for **Google Sheets API** and click
   **Enable**.
3. Go to **APIs & Services -> OAuth consent screen**. Choose
   **External**, fill in an app name and your email, and save. When
   asked for test users, add your own Google account email. You don't
   need to submit this for verification — it's just for your own use.
4. Go to **APIs & Services -> Credentials -> Create Credentials ->
   OAuth client ID**. Application type: **Web application**. Under
   "Authorized JavaScript origins," add the GitHub Pages URL from the
   deploy step above, e.g. `https://alexbuildswood.github.io` (no trailing
   slash, no path). The app's Houses tab now shows the exact origin it's
   running from (with a Copy button) right above the Client ID field —
   use that instead of typing it by hand to avoid typos or a stray
   trailing slash, which are the most common cause of "Error 400:
   invalid_request".
5. Click Create. Copy the **Client ID** (ends in
   `.apps.googleusercontent.com`).
6. Open the app, go to the Houses tab, paste that Client ID into the
   "Google OAuth client ID" field, and click **Connect Google Sheet**.
   Sign in and approve access when prompted.
7. Make sure your Google Sheet has these tabs, named **exactly** as
   below (capitalization and spacing matter — the app looks them up by
   name):
   - `To View` — houses you haven't decided on yet. Shows in the app's
     "To View" sub-tab, fully interactive.
   - `No Decision` — houses you've passed on. Shows in the "No"
     sub-tab, and every card there is automatically greyed out and
     struck through.
   - `Yes Decision` — houses you're pursuing. Shows in the "Yes"
     sub-tab.
   - `Closed` — houses you applied for and lost, or that got rented
     before you could apply. Shows in the "Closed" sub-tab.
   - `Checklist` — with a `Done` and `Order` column; the app matches
     checklist rows by Order number.

   Each of the four house tabs can have its own set of columns — the
   app reads whatever headers are actually there on row 1 and figures
   out Address/Price/City/MLS Link/Zillow Link/Toured?/Interest/Notes/
   Live Notes/Applied/Accepted/Reason automatically by name
   (case-insensitive). Any column it doesn't recognize still shows on
   the card as a plain label/value line, so new columns you add always
   show up without needing an app update.

Across all four tabs, "Like" interest values get a yellow highlight, city
tags are automatically color-coded (same city always gets the same color),
and blank addresses no longer show as "Untitled" — they fall back to the
city name, or "Row N" if both are blank.

## Filtering (To View tab only)

The To View tab has a filter bar (Interest, City, Type) built from whatever
values are actually in your sheet — tap a chip to filter, tap it again or
hit "Clear filters" to reset. Filters combine (e.g. City=Austin AND
Interest=Like).

## Moving a house between tabs

Every To View card has **No** and **Yes** buttons. Tapping one moves that
house immediately — it's written to the No Decision or Yes Decision tab in
your sheet and removed from To View.

Yes Decision cards have two buttons: "&#8617; Move back to To View" and
**Closed**. Use Closed once you've applied and lost the house, or it got
rented before you could apply — it moves the row to the Closed tab. No
Decision and Closed cards each have their own "move back" button (Closed
cards move back to Yes Decision) in case you change your mind; nothing is
ever deleted for good, a move just relocates the row between tabs.

A move copies over whatever fields match by column name or by type
(address, city, price, interest, notes, links, etc.) between tabs, so it
works even though each tab has different columns. Columns that only exist
on the destination tab (like Applied/Accepted, or Reason on Closed) start
blank. While a move is in progress the card shows "Moving…" and is
disabled for a moment.

## Applied / Accepted (Yes Decision and Closed tabs)

If your `Yes Decision` or `Closed` tab has columns named `Applied` or
`Accepted`, the app shows them as checkboxes near the top of each card —
check them off as you apply and hear back, and it writes TRUE/FALSE
straight to those columns in your sheet.

## Reason (Closed tab only)

If your `Closed` tab has a column with "reason" in its name (e.g.
`Reason`), the app shows two tappable chips on each Closed card —
**Applied & lost** and **Rented before I could apply**. Tap one to record
why that house closed; tap it again to clear it. It writes the chosen text
straight to that column in your sheet.

Notes:
- The Spreadsheet ID field is pre-filled with your Move_Plan sheet; only
  change it if you copy this app for a different sheet.
- You'll need to click Connect again each time you open the app in a
  new browser session — the sign-in isn't kept forever, only your
  Client ID and Spreadsheet ID are remembered.
- Checking a box in the app writes back to whichever house tab that
  house lives in, and checking a box in the sheet shows up in the app
  next time you connect or hit Refresh. Same for Live Notes. Unlabeled
  or "helper" columns in your sheet are ignored.
