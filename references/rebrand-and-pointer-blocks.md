# Yorient Rebrand + Pointer Blocks

Use this reference when migrating an existing orientation setup (legacy `orient-map`, `ORIENT.md`, or `yourient`) to the canonical `yorient` branding, or when adding pointer blocks to a fresh project.

## Canonical spellings

- Skill / slash command: `yorient`
- Canonical map file: `YORIENT.md`
- Adapter block anchor: `YORIENT` (versioned: `YORIENT:BEGIN v=1` / `YORIENT:END v=1`)
- README block anchor: `YORIENT-README` (versioned: `YORIENT-README:BEGIN v=1` / `YORIENT-README:END v=1`)
- Never emit `yourient`, `YOURIENT`, `/yourient`, or `YOURIENT.md`.

## Detection

Inside the target repo or vault, list legacy map files:

```sh
find . -maxdepth 4 -type f \
  \( -iname 'ORIENT.md' -o -iname 'orient.md' -o -iname 'orientation.md' \) \
  -not -path '*/.git/*' -not -path '*/node_modules/*'
```

Scan for stale references in committed Markdown:

```sh
grep -rnE '\b(ORIENT\.md|orient-map|yourient|YOURIENT(\.md)?)\b' . \
  --include='*.md' \
  --include='AGENTS.md' --include='CLAUDE.md' --include='GEMINI.md' --include='README.md' \
  --exclude-dir=.git --exclude-dir=node_modules
```

Find all managed blocks (any version, any whitespace, with or without `v=N`):

```sh
grep -rnE '<!--[[:space:]]*YORIENT(-README)?[[:space:]]*:[[:space:]]*(BEGIN|END)([[:space:]]+v[[:space:]]*=[[:space:]]*[0-9]+)?[[:space:]]*-->' . \
  --include='*.md' \
  --include='AGENTS.md' --include='CLAUDE.md' --include='GEMINI.md' --include='README.md' \
  --exclude-dir=.git --exclude-dir=node_modules
```

This permissive opener-detector matches all of the following variants and is portable across GNU grep, BSD grep, and `ripgrep`:

- `<!-- YORIENT:BEGIN -->` (legacy unversioned)
- `<!-- YORIENT:BEGIN v=1 -->` (current)
- `<!-- YORIENT-README:BEGIN v=42 -->` (future versions)
- `<!--  YORIENT : BEGIN  -->` (loose whitespace around `:`)
- `<!-- YORIENT : BEGIN v = 1 -->` (loose whitespace around `=`)
- `<!--YORIENT:END-->` (no internal whitespace)

To narrow to only **legacy unversioned** (`v=0`) blocks that need upgrading:

```sh
grep -rnE '<!--[[:space:]]*YORIENT(-README)?[[:space:]]*:[[:space:]]*(BEGIN|END)[[:space:]]*-->' . \
  --include='*.md' \
  --include='AGENTS.md' --include='CLAUDE.md' --include='GEMINI.md' --include='README.md' \
  --exclude-dir=.git --exclude-dir=node_modules
```

To extract the version attribute from a matched line for upgrade decisions, post-process with a second pass:

```sh
sed -nE 's/.*YORIENT(-README)?[[:space:]]*:[[:space:]]*(BEGIN|END)[[:space:]]+v[[:space:]]*=[[:space:]]*([0-9]+).*/\3/p'
```

### Edge cases worth knowing

- **CRLF line endings.** `grep` matches per logical line regardless of `\n` vs `\r\n`. Detection works under either convention; the managed-block algorithm in `SKILL.md` preserves the file's existing line-ending convention when writing.
- **UTF-8 BOM at file start.** GNU `grep` skips the BOM on line 1; BSD `grep` treats it as part of the line, which can make a `^` anchor fail. The detection regexes above do not use `^`, so they match in both. The write side (the managed-block algorithm) preserves the BOM byte-for-byte.
- **Anchors split across lines.** If a `BEGIN` or `END` marker is wrapped across two lines (rare; only happens with hand-edited HTML), detection misses it. The managed-block algorithm treats this as malformed input and refuses to write. Repair the marker manually before re-running.
- **Multiple block families on one line.** Not supported. Each managed block occupies its own full line.

## Migration decision tree

After detection, decide per file:

1. **Only `ORIENT.md` (or `orient.md`/`orientation.md`) present, no `YORIENT.md`:**
   - Inside a git repo: `git mv ORIENT.md YORIENT.md`.
   - Outside git: `mv ORIENT.md YORIENT.md`.
   - Update internal anchors and link text inside the file from `ORIENT.md` to `YORIENT.md`.

2. **Both legacy file and `YORIENT.md` present:**
   - `YORIENT.md` is canonical.
   - Reduce the legacy file to a single redirect line: `See [YORIENT.md](./YORIENT.md).`
   - Or delete it outright if no external links point at it.

3. **Neither present:**
   - Create `YORIENT.md` from scratch using the shape in `SKILL.md` (`## YORIENT.md Shape`) and the worked example in `references/example-YORIENT.md`.

## Replacement map

For each Markdown file that the detection scans surfaced, apply these literal string replacements (case-sensitive unless noted):

| From | To |
|---|---|
| `ORIENT.md` | `YORIENT.md` |
| `orient-map` | `yorient` |
| `yourient` | `yorient` |
| `YOURIENT.md` | `YORIENT.md` |
| `YOURIENT` (as bare word) | `YORIENT` |
| `/orient-map` (slash-command) | `/yorient` |

Preserve any intentional legacy mention inside migration docs (such as this file). Do not blanket-replace inside `references/rebrand-and-pointer-blocks.md` itself.

## Managed block shapes

### README pointer

```md
<!-- YORIENT-README:BEGIN v=1 -->
## Repo Map

For structure, routing, and agent guidance, read [`YORIENT.md`](./YORIENT.md).

Agents and contributors should consult `YORIENT.md` before broad repo exploration or large refactors.
<!-- YORIENT-README:END v=1 -->
```

### Agent adapter (AGENTS.md / CLAUDE.md / GEMINI.md)

```md
<!-- YORIENT:BEGIN v=1 -->
# Yorient Map

This project uses `YORIENT.md` as the canonical repo/vault map.

Before broad file search or structural edits:
1. Read `YORIENT.md`.
2. Check for a nearer nested `YORIENT.md` or `AGENTS.md` in the target subtree.
3. Use the routing map there to choose files and commands.
4. Keep this adapter thin; update `YORIENT.md` when structure changes.
<!-- YORIENT:END v=1 -->
```

## Full rebrand checklist

When migrating a project end-to-end:

1. Rename skill frontmatter `name:` to `yorient` in every skill mirror.
2. Rename on-disk skill directory to `yorient` in every mirrored agent skill tree.
3. Replace `ORIENT.md` references in the skill body with `YORIENT.md`.
4. Add or update the managed README pointer block at `v=1`.
5. Add or update managed adapter blocks at `v=1` in `AGENTS.md`, `CLAUDE.md`, and `GEMINI.md` per the algorithm in `SKILL.md` (`## Managed Block Algorithm`).
6. Sync the whole skill package across mirrors — copy `references/`, `templates/`, and `scripts/` too when present.
7. Preserve all non-managed file content.
8. Remove stale mirrored `yourient` or `orient-map` directories so there is only one callable slug per ecosystem.
9. If the Hermes local skill had an older telemetry key (`~/.hermes/skills/.usage.json`), migrate it to `yorient`.

## Relative-link rule

Always link to the nearest relevant `YORIENT.md` from the file being edited. Do not hardcode root-level links if a nearer nested map exists.
