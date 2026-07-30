# Move planner

A single-file web app for tracking a move: a checklist grouped by track
(Prep, House, Storage, Mom's), a scrollable gantt-style timeline, a
countdown to a date you choose, and a Houses tab that can sync live with
a Google Sheet. Checking off a task updates its timeline bar. Progress is
saved in the browser's local storage, per device, and mirrored to Google
Sheets if you connect it.

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
   - `Checklist` — with a `Done` and `Order` column; the app matches
     checklist rows by Order number.

   Each of the three house tabs can have its own set of columns — the
   app reads whatever headers are actually there on row 1 and figures
   out Address/Price/City/MLS Link/Zillow Link/Toured?/Interest/Notes/
   Live Notes automatically by name (case-insensitive). Any column it
   doesn't recognize still shows on the card as a plain label/value
   line, so new columns you add always show up without needing an app
   update.

Across all three tabs, "Like" interest values get a yellow highlight, city
tags are automatically color-coded (same city always gets the same color),
and blank addresses no longer show as "Untitled" — they fall back to the
city name, or "Row N" if both are blank.

## Filtering and the map (To View tab only)

The To View tab has a filter bar (Interest, City, Type) built from whatever
values are actually in your sheet — tap a chip to filter, tap it again or
hit "Clear filters" to reset. Filters combine (e.g. City=Austin AND
Interest=Like).

Below the filters is a live map showing a pin for every To View property
with an address or city filled in. It uses OpenStreetMap and the free
Nominatim geocoding service — no API key or extra setup needed, unlike the
Google Sheets connection. Tap a pin's popup to jump straight to that
listing's card. Geocoding is rate-limited to about one request per second
and cached in your browser, so re-opening the app or filtering doesn't
re-geocode addresses it's already looked up.

Notes:
- The Spreadsheet ID field is pre-filled with your Move_Plan sheet; only
  change it if you copy this app for a different sheet.
- You'll need to click Connect again each time you open the app in a
  new browser session — the sign-in isn't kept forever, only your
  Client ID and Spreadsheet ID are remembered.
- Checking a box in the app writes back to whichever of the three tabs
  that house lives in, and checking a box in the sheet shows up in the
  app next time you connect or hit Refresh. Same for Live Notes.
  Unlabeled or "helper" columns in your sheet are ignored.
- Moving a house between "To View," "No Decision," and "Yes Decision"
  is done by cutting/pasting its row into the other tab in Google
  Sheets itself — the app doesn't move rows between tabs for you.
