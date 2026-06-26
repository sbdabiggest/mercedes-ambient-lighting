# LUMEN — Session Handoff

Landing page for **LUMEN**, a Mercedes ambient-lighting install business (Emerald Coast, FL).
Pick up here in a fresh session.

## Working branch
`claude/lumen-frontend-design-jclxia` — all work is committed and pushed here.
Repo: `sbdabiggest/mercedes-ambient-lighting`. There is **no open PR** (don't create one unless asked).

## What this project is
A single, self-contained static site: **`index.html`** (HTML + inline `<style>` + inline `<script>`,
no framework, no external JS deps). Images live in `images/`. It also opens fine via `file://`.

- `package.json` adds optional Vite tooling only: `npm run dev` (hot reload, :5173),
  `npm run build` (→ `dist/`, hashes images), `npm run preview`. Vite is NOT required to view the page.
- A `CONFIG` object near the bottom of `index.html` (`<script>`) centralizes brand, phone, city, service area.

## Design direction (important — honor this)
The user asked: **"Make it look like Apple designed this website without making it look like an Apple website."**
Interpretation we committed to: Apple's *principles* (restraint, precision, deference, generous space,
immaculate type, interaction-driven motion) applied to LUMEN's **dark, glowing automotive identity** —
NOT a white Apple clone. The product *is* colored light, so color is a deliberate accent, never slathered.

### What the restraint pass already did (commit `94fe822`)
- Removed the perpetual rainbow shimmer + gradient from chrome (promo bar, logo, buttons, eyebrows).
  Color now lives only where it means something: hero highlight word, featured tier, hero orbs, color demo.
- Calm dark promo bar (gradient dot only), refined uppercase eyebrows w/ hairline tick, **no emoji** in chrome.
- Generous section rhythm via `--section` clamp; balanced headings (`text-wrap:balance`), tighter display tracking.
- Hero ambient **floating orbs** (depth); frosted header that elevates a hairline border on scroll (`.bar.scrolled`).
- Scroll-reveal choreography (IntersectionObserver) + card/gallery hover lift — all gated behind
  `prefers-reduced-motion` and degraded gracefully (no JS / no IO → content always visible).
- **New interactive 64-color demo** (`#colors` section): tap a swatch → cabin preview relights in real time
  with the color name. JS builds 64 hsl swatches; `--pick` CSS var on `.demo` drives strips/vents/dash glow.
  Note: demo overrides use `.demo-scene.after .strip|.vent|.dash` selectors to win specificity over the
  base `.scene.after .s1/.s2/.s3` rules — keep that if you touch the scene CSS.

## Key gotcha: previewing from this cloud container
The dev server runs **inside an ephemeral remote container with no port forwarding to the user's browser**,
so `localhost:5173` never opens for them. Don't keep restarting servers expecting them to connect.
Reliable ways to show the user the result:
1. **Render screenshots** with the pre-installed Chromium and send via SendUserFile (works every time):
   `node` + `require('/opt/node22/lib/node_modules/playwright')`, `chromium.launch()`,
   `goto('file:///home/user/mercedes-ambient-lighting/index.html')`. Scroll sections into view before
   shooting (reveal animations start at opacity:0). See prior session's script pattern.
2. **Live URL** needs the user's own auth — either: connect the GitHub repo in Vercel (Add New → Project →
   import repo → set branch; static, no build settings) so pushes auto-deploy; or they run it locally.
   Vercel CLI is present but unauthenticated here; the deploy MCP tool only returns CLI instructions.

## Suggested next steps (not yet done)
- [ ] Decide hosting: set up Vercel↔GitHub integration, or merge branch → `main` for production. (Needs user.)
- [ ] Real photos: drop `images/after.jpg`, `before.jpg`, `gallery-1..6.jpg` (real night shots).
      Placeholders (CSS "scene") auto-show when a file is missing via `onerror="this.remove()"`.
- [ ] Swap the 3 benefit "proof" cards for real customer reviews (names + car) once available.
- [ ] Optional: extend the color demo to per-zone control; consider a starlight-headliner section.
- [ ] Optional: bump Vite 5 → 7 to clear 2 dev-only npm audit advisories (low priority, dev-only).
- [ ] Verify the lead form `sms:` deep link and the `tel:` links use the correct production phone in `CONFIG`.

## How to resume quickly
1. `git checkout claude/lumen-frontend-design-jclxia && git pull`
2. Edit `index.html` (everything is in this one file).
3. Screenshot to verify (see gotcha #1), then commit + push to the same branch.
