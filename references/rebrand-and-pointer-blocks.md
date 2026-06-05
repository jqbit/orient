# Orient Rebrand + Pointer Blocks

Use this reference when migrating an existing orientation setup (legacy `orient-map`, `yourient`, or lowercase `orient.md`) to the canonical `orient` branding, or when adding pointer blocks to a fresh project.

## Canonical spellings

- Skill / slash command: `orient`
- Canonical map file: `ORIENT.md`
- Adapter block anchor: `ORIENT` (versioned: `ORIENT:BEGIN v=1` / `ORIENT:END v=1`)
- README block anchor: `ORIENT-README` (versioned: `ORIENT-README:BEGIN v=1` / `ORIENT-README:END v=1`)
- Never emit `yourient`, `YOURIENT`, `/yourient`, or `YOURIENT.md`.

## Detection

Inside the target repo or vault, list map files (including obsolete spellings):

```sh
find . -maxdepth 4 -type f \
  \( -iname 'ORIENT.md' -o -iname 'orient.md' -o -iname 'orientation.md' \) \
  -not -path '*/.git/*' -not -path '*/node_modules/*'
find . -maxdepth 4 -type f -iname 'Y*RIENT.md' \
  -not -path '*/.git/*' -not -path '*/node_modules/*'
```

Scan for stale references in committed Markdown:

```sh
grep -rnE '\b(orient-map|yourient|YOURIENT(\.md)?)\b' . \
  --include='*.md' \
  --include='AGENTS.md' --include='CLAUDE.md' --include='GEMINI.md' --include='README.md' \
  --exclude-dir=.git --exclude-dir=node_modules
```

Find all managed blocks (any version, any whitespace, with or without `v=N`). The `Y?` prefix matches both current and pre-1.5 anchor families:

```sh
grep -rnE '<!--[[:space:]]*Y?ORIENT(-README)?[[:space:]]*:[[:space:]]*(BEGIN|END)([[:space:]]+v[[:space:]]*=[[:space:]]*[0-9]+)?[[:space:]]*-->' . \
  --include='*.md' \
  --include='AGENTS.md' --include='CLAUDE.md' --include='GEMINI.md' --include='README.md' \
  --exclude-dir=.git --exclude-dir=node_modules
```

This permissive opener-detector matches all of the following variants and is portable across GNU grep, BSD grep, and `ripgrep`:

- `<!-- ORIENT:BEGIN -->` (legacy unversioned)
- `<!-- ORIENT:BEGIN v=1 -->` (current)
- `<!-- ORIENT-README:BEGIN v=42 -->` (future versions)
- `<!--  ORIENT : BEGIN  -->` (loose whitespace around `:`)
- `<!-- ORIENT : BEGIN v = 1 -->` (loose whitespace around `=`)
- `<!--ORIENT:END-->` (no internal whitespace)

Pre-1.5.0 managed blocks used the same shape with an extra leading `Y` on the anchor family name; treat them as one family and rewrite to `ORIENT` / `ORIENT-README` on the next skill run.

To narrow to only **legacy unversioned** (`v=0`) blocks that need upgrading:

```sh
grep -rnE '<!--[[:space:]]*Y?ORIENT(-README)?[[:space:]]*:[[:space:]]*(BEGIN|END)[[:space:]]*-->' . \
  --include='*.md' \
  --include='AGENTS.md' --include='CLAUDE.md' --include='GEMINI.md' --include='README.md' \
  --exclude-dir=.git --exclude-dir=node_modules
```

To extract the version attribute from a matched line for upgrade decisions, post-process with a second pass:

```sh
sed -nE 's/.*Y?ORIENT(-README)?[[:space:]]*:[[:space:]]*(BEGIN|END)[[:space:]]+v[[:space:]]*=[[:space:]]*([0-9]+).*/\3/p'
```

### Edge cases worth knowing

- **CRLF line endings.** `grep` matches per logical line regardless of `\n` vs `\r\n`. Detection works under either convention; the managed-block algorithm in `SKILL.md` preserves the file's existing line-ending convention when writing.
- **UTF-8 BOM at file start.** GNU `grep` skips the BOM on line 1; BSD `grep` treats it as part of the line, which can make a `^` anchor fail. The detection regexes above do not use `^`, so they match in both. The write side (the managed-block algorithm) preserves the BOM byte-for-byte.
- **Anchors split across lines.** If a `BEGIN` or `END` marker is wrapped across two lines (rare; only happens with hand-edited HTML), detection misses it. The managed-block algorithm treats this as malformed input and refuses to write. Repair the marker manually before re-running.
- **Multiple block families on one line.** Not supported. Each managed block occupies its own full line.

## Migration decision tree

After detection, decide per file:

1. **Obsolete Y-prefixed map file present, no `ORIENT.md`:**
   - Inside a git repo: rename it to `ORIENT.md` (`git mv` the file whose basename matches `Y*RIENT.md`).
   - Outside git: `mv` to `ORIENT.md`.
   - Update internal anchors and link text to `ORIENT` / `ORIENT-README`.

2. **Both obsolete map file and `ORIENT.md` present:**
   - `ORIENT.md` is canonical.
   - Reduce the obsolete file to a single redirect line: `See [ORIENT.md](./ORIENT.md).`
   - Or delete it outright if no external links point at it.

3. **Only lowercase `orient.md` or `orientation.md` (no uppercase `ORIENT.md`):**
   - Prefer `git mv orient.md ORIENT.md` (or `orientation.md` → `ORIENT.md`) when the content is a map file, not unrelated prose.

4. **Neither present:**
   - Create `ORIENT.md` from scratch using the shape in `SKILL.md` (`## ORIENT.md Shape`) and the worked example in `references/example-ORIENT.md`.

## Replacement map

For each Markdown file that the detection scans surfaced, apply these literal string replacements (case-sensitive unless noted):

| From | To |
|---|---|
| `orient-map` | `orient` |
| `yourient` | `orient` |
| `YOURIENT.md` | `ORIENT.md` |
| `YOURIENT` (as bare word) | `ORIENT` |
| `/orient-map` (slash-command) | `/orient` |
| Pre-1.5 map filename (`Y*RIENT.md`) | `ORIENT.md` |
| Pre-1.5 adapter anchor family | `ORIENT` |
| Pre-1.5 README anchor family | `ORIENT-README` |
| Pre-1.5 skill directory / slash command | `orient` / `/orient` |

Preserve any intentional legacy mention inside migration docs (such as this file). Do not blanket-replace inside `references/rebrand-and-pointer-blocks.md` itself.

## Managed block shapes

### README pointer

```md
<!-- ORIENT-README:BEGIN v=1 -->
## Repo Map

For structure, routing, and agent guidance, read [`ORIENT.md`](./ORIENT.md).

Agents and contributors should consult `ORIENT.md` before broad repo exploration or large refactors.
<!-- ORIENT-README:END v=1 -->
```

### Agent adapter (AGENTS.md / CLAUDE.md / GEMINI.md)

```md
<!-- ORIENT:BEGIN v=1 -->
# Orient Map

This project uses `ORIENT.md` as the canonical repo/vault map.

Before broad file search or structural edits:
1. Read `ORIENT.md`.
2. Check for a nearer nested `ORIENT.md` or `AGENTS.md` in the target subtree.
3. Use the routing map there to choose files and commands.
4. Keep this adapter thin; update `ORIENT.md` when structure changes.
<!-- ORIENT:END v=1 -->
```

## Full rebrand checklist

When migrating a project end-to-end:

1. Rename skill frontmatter `name:` to `orient` in every skill mirror.
2. Rename on-disk skill directory to `orient` in every mirrored agent skill tree.
3. Replace obsolete map filenames and skill-directory names with `ORIENT.md` / `orient`.
4. Add or update the managed README pointer block at `v=1`.
5. Add or update managed adapter blocks at `v=1` in `AGENTS.md`, `CLAUDE.md`, and `GEMINI.md` per the algorithm in `SKILL.md` (`## Managed Block Algorithm`).
6. Sync the whole skill package across mirrors — copy `references/`, `templates/`, and `scripts/` too when present.
7. Preserve all non-managed file content.
8. Remove stale mirrored `yourient` or `orient-map` directories so there is only one callable slug per ecosystem.
9. If the Hermes local skill had an older telemetry key (`~/.hermes/skills/.usage.json`), migrate it to `orient`.

## Relative-link rule

Always link to the nearest relevant `ORIENT.md` from the file being edited. Do not hardcode root-level links if a nearer nested map exists.
