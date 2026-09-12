# CLAUDE.md - CaptainCook App (frontend)

Auto-read by Claude Code at session start. Keep it current.

## Start here: HANDOFF.md

**Before changing anything, read the project handoff:**
`C:\Users\cjgra\Dropbox\My AI\CG Apps\Recipes\HANDOFF.md`

It carries current state, the map of both repos, how to run and deploy, and the section
that matters most - **decisions with the road not taken**, so a deliberate choice does not
get "fixed" by a session that arrives cold. The folder is named `Recipes` (the old
codename); the app is **CaptainCook (Cooking)**.

**It deliberately lives in Dropbox, not in either repo.** CaptainCook spans two repos plus a
docs folder, and one authoritative file beats two copies that drift apart. This pointer
exists so a session starting in the repo still finds it.
Standard: `CG Apps\Forever Apps\DOCUMENTATION-STANDARD.md`.

**Doc currency (starter spec section 5):** keep this file + HANDOFF in step with the code in
the SAME session you change code. Don't hardcode the version here (point to
`APP_VERSION`/`BUILD` + the backend `/health`); write a DATED entry in
`C:\Users\cjgra\Dropbox\My AI\CG Apps\Recipes\Recipes Log\` for every work session.

## Hard gates - do not weaken
1. **Temperatures come ONLY from the curated USDA FSIS table** (`doneness_temps`). The model
   never generates a number; AI answers cite the table.
2. **An unreviewed technique cannot be published, and a draft must LOOK like a draft.** The
   client only ever reads `/api/techniques`, which reads an approved-only database view. Any
   screen that shows a draft uses `.draft-banner` - never the finished-guidance styling.
3. **No community / publishing / sharing features** ported from MenuCaptain without raising
   recipe-import copyright with Chris first (undecided as of 2026-09-12).

## Code readability (Chris's directive 2026-08-10) - part of "done"
Readable by an AUDITOR, a REGULATOR and a DEVELOPER JOINING COLD. Comment the WHY and the
RULE (with its source and date), never the WHAT; record the road not taken; WARNING
comments on footguns; names are the documentation; dead code is deleted. Not a comment per
line. Full standard: `CG Apps\Forever Apps\CODE-READABILITY-STANDARD.md`.

## Pre-push audit gate (never bypass)
`git push` runs `C:\Users\cjgra\portfolio-audit\portfolio_audit.py` through
`.git\hooks\pre-push`. `--no-verify` is NOT acceptable - if the gate is wrong, fix the
checker.

## What this is
Single-file HTML PWA (React via CDN + Babel standalone, no build step). Extracted
2026-09-12 from the FitnessCaptain shell (itself from HomeCaptain). Backend is a SEPARATE
repo (`captaincook-backend`, FastAPI on Railway + Supabase).

## Coordinates
- Repo: `cgramlich/captaincook-app` - NOT CREATED ON GITHUB YET (Chris's account step).
  GitHub username is `cgramlich` (no "j").
- Deploy (once it exists): push to `main` -> GitHub Pages from `main`/root, served from the
  SUBPATH `/captaincook-app/` - keep every asset path relative.
- Version: `APP_VERSION` (label) + `BUILD` ("YYYY-MM-DD.N", what the updater compares) in
  `index.html`, and `VERSION` in `sw.js` in lockstep. Bump on EVERY deploy.
- Backend config: `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `API_BASE` in `index.html` are
  `REPLACE-ME` placeholders until Chris's Supabase + Railway steps land.

## Verify before delivering
- `npm run check` (Babel-in-Node compile gate) before every deploy.
- The compile gate cannot see a missing `ICONS.<key>` - that throws at runtime. Check keys.
- Signed-in screens can be exercised without a backend: inject a stub auth client + fetch
  before the babel block in a COPY of index.html (build the copy from a file, never a shell
  heredoc) and walk it in the browser.
- One change set per deploy.

## How Chris works
- Plain-English feedback; read the code and make edits directly.
- Feature work gets a SHORT proposal + sign-off first. Open questions one at a time, each
  with options and a marked recommendation.
- Debug logs-first: ask for console output / network response before theorizing.
- Commands handed to Chris: ONE per code block, wait for output. Windows 11; keep log output
  ASCII-safe.
