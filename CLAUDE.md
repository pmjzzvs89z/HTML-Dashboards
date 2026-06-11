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
2. Keep cards in **case-insensitive alphabetical order by filename**, using the current card markup
   (each card has an emoji icon, a one-line description, an accent color, and a hover arrow):
   ```html
   <a href="dashboards/FILE.html" class="card" style="--c:#HEXCOLOR">
     <div class="icon">EMOJI</div>
     <div class="body">
       <h2>TITLE</h2>
       <p>SHORT DESCRIPTION</p>
     </div>
     <span class="arrow">→</span>
   </a>
   ```
   - **`--c`** — a distinct accent hex per card (used for the icon tint + hover glow). Vary the hue so
     the grid stays colorful; don't reuse a neighbour's color.
   - **EMOJI** — one emoji that fits the topic (🔗 ⌨️ 🎓 🧭 🪴 🧲 ⚛️ …).
   - **SHORT DESCRIPTION** — ~5–8 words; derive from the dashboard's `<title>`/content. Ask the user
     if the topic isn't obvious.
   - Filenames with spaces must be URL-encoded in `href` (e.g. `Hoya%20Plants.html`).
3. **Card title:** derive from the filename — drop `.html`, split on `-`, Title-Case each word
   (e.g. `bond-half-life` → "Bond Half-Life"). Uppercase known acronyms (NMR, etc.).
   Use a different title only if the user specifies one.
4. **Update the footer count** in `index.html` (`<span class="count">N</span>`) to match the number of cards.
5. **Commit and push** to `main`:
   ```bash
   git add -A && git commit -m "Add TITLE dashboard" && git push origin main
   ```
6. Tell the user the live URL will update in ~30–60s.
