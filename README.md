# Move planner

A single-file web app for tracking a move: one Checklist tab with the
entire move end to end, and a Countdown tab for a date you choose.
Everything is saved in the browser's local storage by default, per
device. Turn on Sync (below) to share checked-off progress between
your laptop and phone.

## Checklist tab

One self-contained checklist covering the whole move, start to finish —
fifteen phases from pre-move logistics (reserve the truck, book
storage, packing supplies) through setting up utilities at the new
house (electric, water, gas, trash, internet), the physical packing
order (garage/shop, five waves sorted least-needed-first, storage
runs), the paperwork side (mail forwarding, address changes,
subscriptions, shop/business address updates), and finally closing out
the old apartment. Tap any row to check it off; each phase shows its
own mini progress bar and count, and tapping a phase title collapses it
so you can focus on what's current. "Reset all" clears the whole list
for your next move. It saves to its own local storage key
(`move_planner_move_v1`).

## Syncing across devices

By default, checked-off progress lives only in that browser's local
storage, so your phone and laptop won't see each other's checkmarks.
The "Set up sync" button at the top of the Checklist tab turns on
free, no-login syncing through [kvdb.io](https://kvdb.io), a small
key-value store:

1. On your first device (say, your laptop), tap **Set up sync**. This
   creates a private storage bucket and immediately syncs your current
   checklist state to it.
2. Tap **Copy code** to copy the sync code it generated (a short
   random ID — this is effectively the "password" for your synced
   checklist, so don't publish it anywhere public).
3. On your other device (your phone), open the same page, paste that
   code into the "Paste sync code from other device" field, and tap
   **Connect**. It immediately pulls over whatever's checked so far.
4. From then on, checking a box on either device pushes the update
   automatically, and each device also polls for changes every 20
   seconds while the page is open. Tap **Refresh now** any time to
   pull the latest immediately (handy right after switching devices).
5. **Disconnect** on a device stops that device from syncing (its
   local checklist stays as-is) without affecting the shared data or
   your other device.

Notes:
- Synced data is stored with kvdb.io, a third-party service — it's
  just the checklist item IDs and their checked/unchecked state, no
  personal details beyond what's already visible in the checklist
  text itself, but it does leave your device once you turn sync on.
  Skip this step if you'd rather keep everything fully local.
- The sync code is the only thing that gates access to your synced
  data. Anyone with the code could read or write it, so treat it like
  a shared password — don't post it anywhere public.
- If you want the same checklist on a third device, just repeat step
  3 there with the same code.

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
