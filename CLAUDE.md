# CLAUDE.md

This repo is `bryancalabro/bryancalabro` — the special repo whose `README.md` renders on the
GitHub **profile page**. GitHub renders only `README.md` there; every other file (including this
one) is ignored by the profile renderer. The repo is public, so files here are browsable, but they
do not appear on the profile.

## Mermaid diagrams — label rules

`README.md` contains 11 Mermaid flowcharts. GitHub's Mermaid renderer clips node labels that are
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
width and clips the overflow — `<br/>` alone does not save you.

Measured on the rendered profile page:

| Line | Chars | Result |
|------|-------|--------|
| `Manual · Late · Reactive` | 24 | fits |
| `Late · Over Budget · Partial` | 28 | clipped |
| `From Specs · Whiteboard · Docs` | 30 | clipped |
| `Written After · Incomplete · Stale` | 34 | badly clipped |

26 leaves a small safety margin under the observed ~28 ceiling. Count characters, not words.

**Applies to:** every line of every node label, primary and secondary.

**Does not apply to:** subgraph titles (`subgraph X["① STAGES 0–8 — …"]`). These size their own
container rather than clipping, so long section headers are fine and should not be shortened.
Edge labels (`-->|approved|`), `classDef`, and `class` lines are also unaffected.

### Writing short secondary lines

The secondary line is a compressed tagline, not a sentence. To get under 26:

- Drop the third item: `Decompose · Sequence · Decide` → `Decompose · Sequence`
- Use the noun or the verb, not both: `Plans · Delegates · Self-Corrects` → `Plan · Delegate · Correct`
- Abbreviate known terms: `Requirements · Architecture · Code Gen · QA` → `Reqs · Arch · Code · QA`
- Cut leading filler: `Feed back → Prompts · Index` → `→ Prompts · Index`

Keep the `·` separator and Title Case — that is the established style across all 11 diagrams.

### Verifying before commit

Local Mermaid (`@mermaid-js/mermaid-cli`) word-wraps labels and renders *both* the broken and
correct forms fine, so a clean local render does **not** prove the profile page is clean. Use it
only to confirm diagrams still parse. The character count is the real check — audit it directly:

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

Then eyeball the rendered page on github.com/bryancalabro after pushing — that is the only
authoritative check.
