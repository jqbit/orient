# orient

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Release](https://img.shields.io/github/v/release/jqbit/orient)](https://github.com/jqbit/orient/releases)
[![checks](https://github.com/jqbit/orient/actions/workflows/checks.yml/badge.svg)](https://github.com/jqbit/orient/actions/workflows/checks.yml)
[![CHANGELOG](https://img.shields.io/badge/CHANGELOG-md-informational)](./CHANGELOG.md)
[![Stars](https://img.shields.io/github/stars/jqbit/orient?style=social)](https://github.com/jqbit/orient/stargazers)

A Markdown orientation map for AI coding agents — drop one `ORIENT.md` at the root of any repository, monorepo, docs tree, or Obsidian vault and let every agent read from the same source of truth. Thin pointer blocks in `README.md`, [`AGENTS.md`](https://agents.md), and `CLAUDE.md` route Claude Code, Codex, Cursor, Gemini, Copilot, and every other `AGENTS.md`-compatible agent to that map — `CLAUDE.md` for Claude Code, `AGENTS.md` for everything else.

No scripts. No generated code. Just Markdown and idempotent, versioned managed blocks that survive across agent ecosystems.

## What it does

- Drops a lean `ORIENT.md` at the root (or any subtree) — purpose, tree map, routing table, commands, no-go zones, drift stamp.
- Adds a thin `<!-- ORIENT:BEGIN v=1 -->` block to your agent-facing files so Claude Code, Codex, Cursor, Hermes, Gemini/Antigravity, OpenCode, Pi, Factory/Droid, GitHub Copilot, and other `AGENTS.md`-compatible tools all read from the same source of truth.
- Adds a `<!-- ORIENT-README:BEGIN v=1 -->` block to `README.md` so human contributors get the same routing.
- Works on Obsidian-style vaults too: `ORIENT.md` plus MOC notes; no folder reorganization required.
- Idempotent: re-running the skill on an unchanged repo produces a byte-identical result.
- Versioned managed blocks (`v=1`) so future iterations can upgrade in place without trampling user content.

## Why

Agents waste enormous time on blind discovery: globbing, grepping, opening files to figure out where the build script lives. A two-minute read of a well-maintained `ORIENT.md` collapses that to near-zero. The pointer blocks make sure every agent — regardless of which `*.md` convention it reads — gets the same routing, and the managed-block algorithm guarantees re-running the skill never corrupts user content.

## Install

`orient` is a skill folder containing `SKILL.md` plus `references/`. Install it under the skills directory of each agent you use.

```sh
# Clone the repo somewhere durable
git clone https://github.com/jqbit/orient.git ~/dev/orient

# Install the skill into the agents you use
# (pick the ones that apply)
mkdir -p ~/.claude/skills && cp -r ~/dev/orient ~/.claude/skills/orient
mkdir -p ~/.codex/skills && cp -r ~/dev/orient ~/.codex/skills/orient
mkdir -p ~/.gemini/skills && cp -r ~/dev/orient ~/.gemini/skills/orient
mkdir -p ~/.gemini/antigravity/skills && cp -r ~/dev/orient ~/.gemini/antigravity/skills/orient
mkdir -p ~/.cursor/skills && cp -r ~/dev/orient ~/.cursor/skills/orient
mkdir -p ~/.factory/skills && cp -r ~/dev/orient ~/.factory/skills/orient
mkdir -p ~/.pi/agent/skills && cp -r ~/dev/orient ~/.pi/agent/skills/orient
mkdir -p ~/.config/opencode/skills && cp -r ~/dev/orient ~/.config/opencode/skills/orient
mkdir -p ~/.agents/skills && cp -r ~/dev/orient ~/.agents/skills/orient
mkdir -p ~/.hermes/skills/software-development && cp -r ~/dev/orient ~/.hermes/skills/software-development/orient
```

Each install copies the skill into the corresponding agent's skill tree. The frontmatter `name: orient` is the callable slug — folder name and frontmatter `name:` must match.

## Use

In any agent that loaded the skill:

```text
/orient
```

Or, if invoking from a one-shot prompt:

```text
Create and maintain a lean, text-only ORIENT layer for this repo/vault.
Use ORIENT.md as the canonical Markdown map, add or update a short
README.md pointer plus thin AGENTS.md (every non-Claude agent) and
CLAUDE.md (Claude Code) adapters, do one broad structural scan using
normal file/search tools
only, launch read-only parallel subtree explorers for major domains,
synthesize their Markdown reports into concise ASCII-tree routing docs,
preserve existing content with managed v=1 blocks, and do not create
scripts or non-Markdown helper files.
```

## What gets written

For a typical repo, `orient` produces:

```text
ORIENT.md                   # canonical map (purpose, tree, routing, commands, no-go, drift stamp)
README.md                    # gains a managed ORIENT-README:BEGIN block pointing to ORIENT.md
AGENTS.md                    # gains a managed ORIENT:BEGIN block (every non-Claude agent — Codex, Cursor, Gemini, Copilot, OpenCode, Hermes, Pi, Factory/Droid)
CLAUDE.md                    # gains a managed ORIENT:BEGIN block (Claude Code)
<subtree>/ORIENT.md         # optional nested map for large subsystems
```

Existing content in those files is preserved. New runs of `orient` only touch the managed-block regions.

## Repo contents

| File | Purpose |
|---|---|
| `SKILL.md` | The skill itself — instructions an agent reads to do the work. |
| `references/rebrand-and-pointer-blocks.md` | Detection commands, migration decision tree, replacement map. |
| `references/example-ORIENT.md` | Fully worked example for a Node monorepo, as a calibration target. |
| `references/by-ecosystem.md` | Quick reference for manifests, layouts, and commands per ecosystem. |

## Design principles

- **Map first, details on demand.** `ORIENT.md` routes; it does not document.
- **Markdown only.** No scripts, no generated CLIs, no helper code.
- **Idempotent blocks.** Managed blocks at `v=1` upgrade in place; unversioned legacy blocks are detected and rewritten.
- **Portable.** Same map serves every agent ecosystem.
- **Lean.** A good `ORIENT.md` is 100–250 lines. Anything larger compresses into nested `<subtree>/ORIENT.md` files.

## Status

`v1.6.0` — `ORIENT.md`, `ORIENT` / `ORIENT-README` managed blocks; adapters narrowed to `AGENTS.md` + `CLAUDE.md`. See [`CHANGELOG.md`](./CHANGELOG.md).

## License

MIT. See [`LICENSE`](./LICENSE).

<!-- ORIENT-README:BEGIN v=1 -->
## Repo Map

For structure, routing, and agent guidance, read [`ORIENT.md`](./ORIENT.md).

Agents and contributors should consult `ORIENT.md` before broad repo exploration or large refactors.
<!-- ORIENT-README:END v=1 -->
