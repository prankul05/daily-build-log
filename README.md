# Deploying the tracker to GitHub Pages

`index.html` in this folder is the exact tracker artifact, renamed for GitHub Pages (which requires an `index.html` at the root of what it serves). No build step, no dependencies, no GitLab-specific config — it's the same self-contained file.

## Steps

1. Create a new GitHub repo (public is simplest — private repos need a GitHub Pro/Team/Enterprise plan for Pages; public repos get Pages free on any plan).
2. Push this folder's `index.html` to the repo root (or to a `/docs` folder — just be consistent with what you pick in step 4).
3. Go to the repo's **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**. Pick your branch (usually `main`) and the folder (`/ (root)` or `/docs`, matching step 2). Save.
5. Wait a minute or two, then GitHub shows the live URL (something like `https://<username>.github.io/<repo-name>/`). Bookmark it.

No GitHub Actions workflow is needed for a plain static file like this — the branch-based "Deploy from a branch" method serves it directly.

## The one real caveat: localStorage is per-browser, per-device, per-origin

Once this is hosted, all your checkbox/ADR/Skills-log progress is still stored in **your browser's localStorage for that exact URL** — not in the repo, not on GitHub's servers, not synced anywhere. Concretely:

- Opening the same URL in a different browser (or a different device, or an incognito/private window) starts with **zero progress** — it's a separate localStorage bucket.
- Clearing your browser's site data/cache for that URL wipes progress.
- Progress **will** persist across normal reloads and closing/reopening the same browser on the same device — that part works exactly like it did before hosting.

If you want progress to follow you across devices, that needs a small backend (or a sync-to-GitHub-via-API button) — a real upgrade, not something to bolt on preemptively. For a solo 30-day sprint on one primary machine, the current localStorage-only version is genuinely fine — just pick one browser/device as your daily driver.

## Updating the tracker later

Since there's no build step, updating the site is just: edit `index.html`, commit, push. GitHub Pages redeploys automatically within a minute or so. Note that editing the file doesn't touch anyone's already-stored localStorage — your in-progress checkmarks survive a redeploy as long as the task IDs in the data (the `d:` day numbers and task array order) don't change. If you reorder or delete tasks later, their old checked state may not line up with the new list.
