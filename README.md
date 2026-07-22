# Move planner

A single-file web app for tracking a move: a checklist grouped by track
(Prep, House, Storage, Mom's) and a scrollable gantt-style timeline.
Checking off a task updates its timeline bar. Progress is saved in the
browser's local storage, per device.

## Deploy with GitHub Pages

1. Create a new repository on GitHub (public, for the free Pages tier).
2. Upload `index.html` from this folder to the repo (drag and drop on
   github.com works fine, or `git add` / `git commit` / `git push`).
3. In the repo, go to Settings -> Pages.
4. Under "Build and deployment", set Source to "Deploy from a branch",
   branch to `main` (or `master`), folder to `/ (root)`, then Save.
5. GitHub gives you a URL like `https://<username>.github.io/<repo>/`
   within a minute or two. Open it on your phone and add it to your
   home screen for an app-like icon.

No build step, no server, no dependencies to install.
