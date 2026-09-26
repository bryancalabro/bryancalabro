# AGENTS.md

This repo is `bryancalabro/bryancalabro`, the special repo whose `README.md` renders on the
GitHub **profile page**. GitHub renders only `README.md` there; every other file (including this
one) is ignored by the profile renderer. The repo is public, so files here are browsable, but they
do not appear on the profile.

## General guidelines

- Never use an em dash. Use a comma, semicolon, parentheses, period, or a plain hyphen.
- Never add an agent name as co-author on commits.
- Never edit CHANGELOG.md or other auto-generated files by hand.
- When making technical decisions, do not overweight development cost. Prefer quality, simplicity, robustness, scalability, and long-term maintainability.
- For bug fixes, reproduce the issue the way a user would before changing code.
- In UI work, treat visual defects as defects. If something looks off, fix it even if it is next to the task, not the task itself.
- Fix lint failures, test failures, and flaky tests when you see them, including ones you did not cause.
- Do not use Chromium, Playwright, chromium-cli, or `npx playwright` to drive or screenshot the app. Verify UI by reading the rendered output or asking for a browser check.
- Verify facts against primary sources: code, data files, official text. Do not treat transcripts, meetings, or recollection as fact. Those are sources of intent.
- On every ship commit / production PR, bump `package.json` from the repo's current version per §2. Never jump to a fixed target. Keep footer `v{version}` in sync.

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

Root `AGENTS.md` is the only real rule file. Root `CLAUDE.md` is wiring only: `@AGENTS.md`. Do not mirror a second full copy.

### Git

Open PRs. Do not push `main` without approval. Do not delete Vercel projects or domains.

## Generated blocks

The recent apps table and the twelve Mermaid diagrams are generated from calabrodesign, which holds
their source: the app inventory (`specs/app-inventory/`) and `docs/systems/` (one `.mmd` file per
diagram, plus `systems.json` for titles, summaries, and research links). The same diagrams render on
the Systems tab at bryancalabro.com. Edit them there, then from calabrodesign run:

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
