# Move planner

A single-file web app for tracking a move: one Checklist tab with the
entire move end to end, and a Countdown tab for a date you choose.
Everything is saved in the browser's local storage, per device —
nothing is sent anywhere.

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
