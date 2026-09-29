# AGENTS.md

This repo is `bryancalabro/bryancalabro`, the special repo whose `README.md` renders on the
GitHub **profile page**. GitHub renders only `README.md` there; every other file (including this
one) is ignored by the profile renderer. The repo is public, so files here are browsable, but they
do not appear on the profile.

## General guidelines

- Never use an em dash. Use a comma, semicolon, parentheses, period, or a plain hyphen.
- Never edit CHANGELOG.md or other auto-generated files by hand.
- When making technical decisions, do not overweight development cost. Prefer quality, simplicity, robustness, scalability, and long-term maintainability.
- For bug fixes, reproduce the issue the way a user would before changing code.
- In UI work, treat visual defects as defects. If something looks off, fix it even if it is next to the task, not the task itself.
- Fix lint failures, test failures, and flaky tests when you see them, including ones you did not cause.
- Verify UI in a real browser with Playwright (headless Chromium): load the page, exercise the change, fail on console errors, and take a screenshot when the look matters. A check worth repeating becomes a spec in `e2e/` (`npm run test:e2e`) that runs before every PR. Browser checks add to typecheck, lint, test, and build; they never replace them. In a Claude Code cloud session Chromium is pre-installed; never download browsers there.
- Verify facts against primary sources: code, data files, official text. Do not treat transcripts, meetings, or recollection as fact. Those are sources of intent.
- On every ship commit / production PR, bump `package.json` from the repo's current version per the Version bumps rule below. Never jump to a fixed target. Keep footer `v{version}` in sync.

## House rules

Apply across first-party Calabro app repos. Skip upstream and third-party.

### Footer (every app)

Replace any `· Calabro ·`, bare URL, Beta, or other footer chrome with exactly:

`© {currentYear} Bryan J. Calabro - v{packageVersion}`

- `{currentYear}` comes from the user's clock at render (`new Date().getFullYear()` or equivalent). Do not hardcode a year.
- `{packageVersion}` is `package.json` `version`, shown with a leading `v` (example `v0.1.0`).
- The entire footer line (copyright symbol, year, name, hyphen, and version) is one link to `https://bryancalabro.com`. Do not link only the name.
- That link has no hover styling.
- Footer and `package.json` version stay in sync.

### Version bumps (every ship / production PR)

Every ship commit that releases must bump `package.json` from that repo's current version (one increment on the rightmost digit). Never jump to a fixed target like `0.1.2`. Prefer one increment per ship commit; prefer one commit per footer PR. A second bump in the same PR is OK only if a material amend needs a second ship commit.

Scheme: three-part `MAJOR.MINOR.PATCH`. The rightmost component increments `1` through `9`, then rolls over, increments the next component left, and resets the right to `0`. Same rule for every major.

Examples:
- If current is `0.0.1`, bump to `0.0.2`
- `0.0.9` to `0.1.0`
- `0.1.1` to `0.1.2` only when it was already `0.1.1`
- `0.9.9` to `1.0.0`
- `1.0.9` to `1.1.0`, then on to `1.9.9` to `2.0.0`

Write versions as `v0.2.0`, never `v.0.2.0`. Footer must show the new version after the bump.

If the repo has no `package.json`, on first ship create a minimal `package.json` with `"version": "0.0.1"` (plus normal name/private fields as needed). Footer shows `v0.0.1`. Never invent a footer version that is not written to `package.json`. Next ships bump from there.

### Agent rule files

Root `AGENTS.md` is the only real rule file. Root `CLAUDE.md` is wiring only: `@AGENTS.md`. Ship both on every greenfield scaffold (the one-line pointer covers Claude Code paths that still need `CLAUDE.md` after native `AGENTS.md` support). Do not mirror a second full copy.

### Git

Open PRs. Do not push `main` without approval. Do not delete Vercel projects or domains.

Greenfield exception: when the remote has no branches yet, push the scaffold commit as `main` first, so `main` becomes the default branch; everything after it goes through a PR (see `/new-app` step 3). On a repo `/new-app` created or found empty, `/new-app` merges that first build PR itself once it is ship-ready (the step 6 gates pass locally, `/ship` has run, and the PR is out of draft), by rebase, at the commit the gates passed on. A repo that has ever merged a PR gets no exception: every later PR waits for Bryan. There is no CI: the gates run locally before every PR, because Actions minutes are one monthly budget across every private repo, so a workflow runs on dispatch or a schedule, never on push or PR (`house-audit` `ci`).

When `/new-app` already shipped the product, the first `/phase` is verify-and-stamp against main, not a rebuild.

### Catalog

The app inventory in calabrodesign is the canonical catalog of every app: name, host, meta description, surface, status, design family, and build prompt. It is `specs/app-inventory.md` (the index) plus one record per app under `specs/app-inventory/`. Read this app's record before renaming it, changing its public URL, or rewriting its meta description, and after a launch or public-facing change stamp that record with `/inventory sync` (a local run of the factory for one record) so completion is stamped from evidence, never by hand. Do not invent a product name, host, or description that is not in the catalog. Portfolio cards on bryancalabro.com match the inventory, not the other way around. Catalog: https://github.com/bryancalabro/calabrodesign/blob/main/specs/app-inventory.md (use `/inventory row`).

### Data, UI, and routing

- Refresh scripts keep the stored value when a fetch fails; never overwrite good data with a fallback.
- Data that rarely changes is bundled with a `SOURCE.md`; a source is the primary publisher's free, documented API, credited on screen as its terms require.
- Live data comes only from the app's own origin: a stateless `api/` function allow-lists its parameters, keeps any free key in the Vercel env, validates and trims the response, and caches at the edge with `stale-while-revalidate` and `stale-if-error`; an upstream failure returns an error, never a cached fallback.
- Model output is generated at build time from fixed options and shipped as data; a shipped app never calls a model and its Vercel project holds no model key (hatch is the standing exception).
- Normalise third-party prose to the house writing rules at ingest, not at render.
- Accent colours carry rules, fills, and icons, never small text; each theme's `--focus` token measures 3:1 or better against its backgrounds.
- A family is a layout, not a look. Each app's look is designed for it at build time: Mobbin research into real current apps in its own category sets the bar, then a random roll of one register lens and one finishing-detail lens, research into the app's own subject, a palette sampled from real references landing on a modern register (a neutral base plus one or two confident accents), open faces that load, and the 390px layout designed first. No palette lists, no hue formulas, and three retired registers, each at most a small accent: skeuomorphic surfaces (leaded glass, terrazzo, riso grain, carbon-copy ghosting, wax crackle, twine, postmarks), the 1970s-80s palette (burnt orange, olive, mustard, rust), and novelty display faces set as UI or body type (Monoton, Silkscreen, stencil faces). It is recorded in the app's catalog record and checked against every other app (`inventory-looks.mjs --candidate`); the built fleet never gains a twin (`looks-twins.json` only shrinks, `npm run verify` fails on a new pair), and `/ship` fails when an app drifts from a designed record (`design-audit --record --strict-record`).
- A new catalog record is never the same app as one already there: `inventory-add.mjs` refuses a near-duplicate unless `--similar-ok` says why, `npm run verify` fails on one nobody answered for, and a full concept audit reads every record every 100 new ones (`/inventory` 8b).
- Display titles break anywhere inside a `min-w-0` wrapper; sum fixed rows against 288px of content at a 320px viewport before claiming no horizontal scroll.
- Search ranks name matches ahead of text-only matches before truncating.
- Next.js routes with a closed `generateStaticParams` set export `dynamicParams = false`.
- Blueprint acceptance checks that need a browser (a viewport, the keyboard, focus, a swipe, the console) are Playwright checks the gate runs; tag a check browser-only only when automation cannot judge it (how sound or motion feels, a real device).
- Every figure, example ID, vector, or date an acceptance check quotes is computed by a script or taken from an independent oracle (a library, a reference fixture, a corpus), never typed from memory; a check that encodes an outside rule names its source.
- The typecheck reads code: a solution-style root `tsconfig.json` needs a `typecheck` script running `tsc -b`.
- Offline precache lists come from the build (a Vite plugin, or a later build step over the finished `dist/` when the page is prerendered) or are linted against the files on disk (static). A build-emitted worker matches with `ignoreVary: true` so cors module scripts and fonts still hit the cache when the host answers `Vary: Origin`.
- Locked and disabled states that carry text change shape or token, never opacity.
- An element hidden with the `hidden` attribute whose class sets `display` also gets `.class[hidden] { display: none }` (`house-audit` `hidden-display`).
- Unicode escapes stay escapes: after writing one, grep the file for the literal character (`house-audit` `invisible-chars`).
- Copy people read carries no AI tells: no dash used as a clause break, no chatbot phrasing; write and check it with `/humanize` (`house-audit` `ai-tells`).
- Page titles read `Name | Value prop`: the name keeps its own capitalization and the value prop is sentence case with its first word capitalized (`Gambit | Full chess against an alpha-beta engine`); the same string fills `<title>`, `og:title`, and `twitter:title` (`house-audit` `meta-head`). Every other page is `Page | Name`, the page title in sentence case (`The constraints | Engineering`).

## Generated blocks

The recent apps table and the twelve Mermaid diagrams are generated from calabrodesign, which holds
their source: the app inventory (`specs/app-inventory/`) and `docs/systems/` (one `.mmd` file per
diagram, plus `systems.json` for titles, summaries, and research links). Edit them there, then from
calabrodesign run:

```bash
node scripts/profile-readme.mjs ../bryancalabro/README.md
```

It rewrites only what sits between the `<!-- recent-apps:start/end -->` and `<!-- systems:start/end -->`
markers. Never hand-edit inside them; the next run overwrites it.

## Mermaid diagrams: label rules

`README.md` contains 12 Mermaid flowcharts. GitHub's Mermaid renderer clips node labels that are
too wide instead of wrapping them, so labels must be authored defensively. Two rules, both required:

### 1. Use quoted labels with `<br/>`, never `\n`

```
GOOD:  ROUTER["Task Router<br/>Which Agent · What Order"]
BAD:   ROUTER[Task Router\nWhich Agent · What Order]
```

`\n` is undocumented legacy syntax. GitHub sizes the node box from the label *before* expanding
`\n` into a line break, so any line after the first overflows the box and gets cut off.

### 2. Keep every line to **26 characters or fewer**

This is the one that actually matters, and it applies per line, not per label. GitHub caps node
width and clips the overflow; `<br/>` alone does not save you.

Measured on the rendered profile page:

| Line | Chars | Result |
|------|-------|--------|
| `Manual · Late · Reactive` | 24 | fits |
| `Late · Over Budget · Partial` | 28 | clipped |
| `From Specs · Whiteboard · Docs` | 30 | clipped |
| `Written After · Incomplete · Stale` | 34 | badly clipped |

26 leaves a small safety margin under the observed ~28 ceiling. Count characters, not words.

**Applies to:** every line of every node label, primary and secondary.

**Does not apply to:** subgraph titles (`subgraph X["① Stages 0–10: closed-loop methodology"]`). These size their own
container rather than clipping, so long section headers are fine and should not be shortened.
Edge labels (`-->|approved|`), `classDef`, and `class` lines are also unaffected.

### Writing short secondary lines

The secondary line is a compressed tagline, not a sentence. To get under 26:

- Drop the third item: `Decompose · Sequence · Decide` → `Decompose · Sequence`
- Use the noun or the verb, not both: `Plans · Delegates · Self-Corrects` → `Plan · Delegate · Correct`
- Abbreviate known terms: `Requirements · Architecture · Code gen · QA` → `Reqs · Arch · Code · QA`
- Cut leading filler: `Feed back → Prompts · Index` → `→ Prompts · Index`

Keep the `·` separator and sentence case, the house rule for every label: capitalize the first word of
each line and each `·` item, and keep acronyms, product names, and code (`/new-app`, `learnings.json`)
as they are spelled.

### Verifying before commit

Local Mermaid (`@mermaid-js/mermaid-cli`) word-wraps labels and renders *both* the broken and
correct forms fine, so a clean local render does **not** prove the profile page is clean. Use it
only to confirm diagrams still parse. The character count is the real check; audit it directly:

```bash
# flag any label line over 26 chars inside mermaid blocks
node -e '
const s=require("fs").readFileSync("README.md","utf8").split("\n");
let m=false,c=0;
s.forEach((l,i)=>{
  if(/^```mermaid\s*$/.test(l)){m=true;c++;return}
  if(m&&/^```\s*$/.test(l)){m=false;return}
  if(!m)return;
  const q=l.match(/"([^"]*)"/); if(!q)return;
  if(/^\s*subgraph/.test(l))return;              // titles exempt
  q[1].split("<br/>").forEach(t=>{if(t.length>26)console.log(`c${c} L${i+1} (${t.length}) ${t}`)});
});'
```

Then eyeball the rendered page on github.com/bryancalabro after pushing; that is the only
authoritative check.
