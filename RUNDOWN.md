# GrafterUK site — rundown
_Last updated: 2026-08-24_

## What it is
The public marketing site for GrafterUK Ltd at **grafteruk.co.uk** — the parent brand.
Positioned as marketing plus AI implementation for UK small businesses and trades. It's
the credibility page a prospect lands on, not a product; it exists to make the pitch and
carry the company/compliance details.

## How it works
About as simple as it gets: **two hand-written static HTML pages served by a Cloudflare
Worker's static-assets binding.** No build step, no framework, no JS dependencies, no
backend.

- `index.html` — the whole site.
- `privacy.html` — privacy policy, carrying ICO registration **ZC207387**.
- `wrangler.jsonc` — worker name `grafteruk`, `assets.directory` set to `"."`.
- `.assetsignore` — load-bearing. Because the assets directory is the repo root, this is
  what stops `.git/` and `.wrangler/` being served as public files. That was a real leak
  fixed in commit `9a41ea6`; don't remove it.

Serving the repo root directly is the trade-off: zero build complexity, at the cost of
needing `.assetsignore` to be correct.

## What it does today
- Serves the marketing page and privacy policy at grafteruk.co.uk (verified 200 on 2026-08-24).
- Publishes the ICO registration number as required for a data controller.
- Anchors the AI-implementation pitch on the live demos (the other estate sites).
- No forms, no lead capture, no email, no analytics endpoint in this repo.

## Current state
**Live and stable — and effectively finished/dormant.**

- grafteruk.co.uk returns 200. Deployed as a Workers assets site.
- Note: `wrangler.jsonc` contains **no `routes` block**, unlike the other sites in the
  estate. The custom domain is therefore bound in the Cloudflare dashboard rather than in
  config — not determined from the repo, so don't assume a redeploy re-establishes it.
- Working tree clean, in sync with `origin/main`. Last real change was the ICO number
  (`d136535`); nothing since 2026-07-28.
- No tests — there is nothing here to test.

## What's next
Parked, and reasonably so. It's a brochure page that does its job. If it gets picked back up:

1. Decide whether it needs a contact/enquiry form. It has none — every other site in the
   estate captures leads to KV, this one doesn't capture anything.
2. Move the custom-domain route into `wrangler.jsonc` so the deploy is self-describing.
3. Fix the README (see gotchas).

## Gotchas
- **The README is not about this site.** It's a GitHub-profile-style "Hi, I'm Jordan" page
  listing personal projects. Anyone opening this repo cold will be misled about what it is.
  Worth replacing.
- `assets.directory` is `"."` — the repo root is the web root. **Anything you add to this
  repo is publicly served unless `.assetsignore` excludes it.** Never put a secret, note,
  or draft in here.
- Deploys via `npx wrangler deploy`. On this machine prefix wrangler calls with
  `export NPM_CONFIG_CACHE=/tmp/npm-cache-pp` — the default npm cache errors.
- Standing rule for the estate: deploy and push travel together. Live code should exist on
  GitHub the same day.
