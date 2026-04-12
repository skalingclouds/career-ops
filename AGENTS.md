# Career-Ops for Codex

Read `CLAUDE.md` for all project instructions, routing, and behavioral rules. They apply equally to Codex.

Key points:
- Reuse the existing modes, scripts, templates, and tracker flow — do not create parallel logic.
- Store user-specific customization in `config/profile.yml`, `modes/_profile.md`, or `article-digest.md` — never in `modes/_shared.md`.
- Never submit an application on the user's behalf.

For Codex-specific setup, see `docs/CODEX.md`.

## Cursor Cloud specific instructions

### Project overview
career-ops is a file-based (Markdown/YAML/TSV) AI job search pipeline. There is no long-running server — all functionality is in Node.js `.mjs` scripts and an optional Go TUI dashboard.

### How to lint/test/build
- **Test suite:** `node test-all.mjs` (or `node test-all.mjs --quick` to skip the Go dashboard build). See `package.json` `scripts` for individual utilities.
- **Doctor check:** `npm run doctor` — validates prerequisites (Node, Playwright, user data files).
- **Dashboard build:** `cd dashboard && go build -o career-dashboard .`

### Running the scanner (main "hello world")
Before running `node scan.mjs`, ensure these user data files exist (they are gitignored):
1. `cp config/profile.example.yml config/profile.yml`
2. `cp templates/portals.example.yml portals.yml`
3. `cp modes/_profile.template.md modes/_profile.md`
4. Create `cv.md` (copy from `examples/cv-example.md` for testing)
5. Create `data/pipeline.md` with header `# Pipeline Inbox`

The scanner fetches live data from Greenhouse/Ashby/Lever APIs — internet access is required.

### Gotchas
- `cv-sync-check.mjs` exits with code 1 when `cv.md` is missing — this is expected in CI/clean clones.
- The test suite uses `grep` internally; do not remove coreutils.
- Playwright requires system-level Chromium deps. If `npx playwright install chromium` doesn't install OS deps, run `npx playwright install-deps chromium`.
