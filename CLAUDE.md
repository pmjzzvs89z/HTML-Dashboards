# HTML-Dashboards

A static gallery of standalone HTML dashboards, deployed to https://html-dashboards.vercel.app

## Structure
- `index.html` — landing page: a grid of cards, one per dashboard, each linking to a file in `dashboards/`.
- `dashboards/*.html` — self-contained dashboards (one HTML file each).

## Deploy
Vercel is connected to the GitHub repo (`pmjzzvs89z/HTML-Dashboards`). Any push to `main`
auto-deploys — there is no Vercel CLI or build step, static files are served as-is.
The live site updates ~30–60s after a push.

## Publishing a new dashboard — the "publish" workflow
When the user adds a file to `dashboards/` and says **"publish"** (optionally naming the file):

1. **Rebuild the card grid** in `index.html` by scanning `dashboards/*.html`, so the index always
   matches the folder. Rebuild the whole grid rather than appending — this also catches any files
   added manually outside this flow.
2. Keep cards in **alphabetical order by filename**, using the existing markup:
   ```html
   <a href="dashboards/FILE.html" class="card">
     <h2>TITLE</h2>
   </a>
   ```
3. **Card title:** derive from the filename — drop `.html`, split on `-`, Title-Case each word
   (e.g. `bond-half-life` → "Bond Half-Life"). Uppercase known acronyms (NMR, etc.).
   Use a different title only if the user specifies one.
4. **Commit and push** to `main`:
   ```bash
   git add -A && git commit -m "Add TITLE dashboard" && git push origin main
   ```
5. Tell the user the live URL will update in ~30–60s.
