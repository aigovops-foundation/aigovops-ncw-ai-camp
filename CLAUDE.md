# AiGovOps x NCW AI Camp: notes for Claude

The community AI training programme built for the NCW Tech Alliance AI Expo (11 August 2026).
Public repo, Apache-2.0.

## Layout

- `docs/`: the published site. GitHub Pages serves `main` `/docs` directly, so a merge to `main`
  that touches `docs/` is a publish. There is no build step for HTML.
- `authoring/`: the source documents (booklet, pledge, QA report, citations).
- `tests/`, `capture/`, `scripts/`, `tools/`: tests, demo capture and link checkers.

## Checks to run before a pull request

```bash
npm ci --no-audit --no-fund
npm run test:html
npm run test:css
npm run test:api
npm run test:links
python3 scripts/check_row_links.py
python3 scripts/check_video_links.py
npm run test:e2e
npm run test:a11y
```

The end-to-end and accessibility tests need Playwright browsers
(`npx playwright install --with-deps chromium webkit`).

## Things that are easy to get wrong

- The 7-point Pledge text (`docs/pledge.html`, `docs/index.html`,
  `authoring/06-pledge-and-two-futures.md`) changes only with both co-founders' agreement. Do not
  edit it, reword it or "tidy" it. Raise it instead.
- The pledge is anonymous by design. `tests/api/pledge-anonymity.test.mjs` guards that. Never add
  a field, log line or request that could identify who signed.
- Every factual claim traces to a primary source; see `authoring/15-qa-report.md`.
- No SaaS or proprietary dependency, and no CDN for CSS: `tests/css-no-cdn.test.mjs` fails on one.
  After changing Tailwind classes, run `npm run build:css` and commit the result.
- CODEOWNERS requests review from both co-founders on every change.

## Rules for an agent

- Agents propose, humans approve. Open a pull request; do not merge it, and do not push to `main`.
- Never sign, approve or rule on anything in a human's name.
- Secrets live in 1Password. Never print, paste or commit a key, token or private key.
- A check that was not run is reported as not run. Do not write "passes" from memory.
- Read `CONTRIBUTING.md` where there is one. It wins over this file where they differ.
