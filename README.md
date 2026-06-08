# orient

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Release](https://img.shields.io/github/v/release/jqbit/orient)](https://github.com/jqbit/orient/releases)
[![checks](https://github.com/jqbit/orient/actions/workflows/checks.yml/badge.svg)](https://github.com/jqbit/orient/actions/workflows/checks.yml)
[![CHANGELOG](https://img.shields.io/badge/CHANGELOG-md-informational)](./CHANGELOG.md)
[![Stars](https://img.shields.io/github/stars/jqbit/orient?style=social)](https://github.com/jqbit/orient/stargazers)

A Markdown orientation map for AI coding agents — drop one `ORIENT.md` at the root of any repository, monorepo, docs tree, or Obsidian vault and let agents read from the same source of truth. Thin pointer blocks in `README.md`, [`AGENTS.md`](https://agents.md), and `CLAUDE.md` route Claude Code, Codex, Cursor, Gemini CLI, Copilot coding agent, and other `AGENTS.md`-compatible tools to that map — `CLAUDE.md` for Claude Code, `AGENTS.md` for compatible non-Claude tools.

No scripts. No generated code. Just Markdown and idempotent, versioned managed blocks that survive across agent ecosystems.

## What it does

- Drops a lean `ORIENT.md` at the root (or any subtree) — purpose, tree map, routing table, commands, no-go zones, drift stamp.
- Adds a thin `<!-- ORIENT:BEGIN v=1 -->` block to your agent-facing files so Claude Code plus AGENTS-compatible tools such as Codex, Cursor, Gemini CLI, OpenCode, Factory, and GitHub Copilot coding agent can read from the same source of truth.
- Adds a `<!-- ORIENT-README:BEGIN v=1 -->` block to `README.md` so human contributors get the same routing.
- Works on Obsidian-style vaults too: `ORIENT.md` plus MOC notes; no folder reorganization required.
- Idempotent: re-running the skill on an unchanged repo produces a byte-identical result.
- Versioned managed blocks (`v=1`) so future iterations can upgrade in place without trampling user content.

## Why

Agents waste enormous time on blind discovery: globbing, grepping, opening files to figure out where the build script lives. A two-minute read of a well-maintained `ORIENT.md` collapses that to near-zero. The pointer blocks give configured agents the same routing, and the managed-block algorithm guarantees re-running the skill never corrupts user content.

## Install

`orient` is a skill folder containing `SKILL.md` plus `references/`. Install it into agents that support `SKILL.md`-style skill packages. For agents that only read project context files, run `orient` from a supported host and commit the generated `ORIENT.md`, `AGENTS.md`, and `CLAUDE.md` files.

```sh
# Clone the repo somewhere durable
git clone https://github.com/jqbit/orient.git ~/dev/orient

# Install the skill into supported local skill trees.
# Pick the ones that apply to your setup.
mkdir -p ~/.claude/skills/orient
rsync -a --delete --exclude='.git' ~/dev/orient/ ~/.claude/skills/orient/

mkdir -p ~/.codex/skills/orient
rsync -a --delete --exclude='.git' ~/dev/orient/ ~/.codex/skills/orient/

mkdir -p ~/.hermes/skills/software-development/orient
rsync -a --delete --exclude='.git' ~/dev/orient/ ~/.hermes/skills/software-development/orient/
```

Each install copies the skill into the corresponding agent's skill tree. The frontmatter `name: orient` is the callable slug — folder name and frontmatter `name:` must match. Do not assume every `AGENTS.md`-compatible tool has a global `skills/` directory; use that tool's documented skill/package mechanism if it has one.

## Use

In any agent that loaded the skill:

```text
/orient
```

Or, if invoking from a one-shot prompt:

```text
Create and maintain a lean, text-only ORIENT layer for this repo/vault.
Use ORIENT.md as the canonical Markdown map, add or update a short
README.md pointer plus thin AGENTS.md (AGENTS-compatible agents) and
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
AGENTS.md                    # gains a managed ORIENT:BEGIN block (AGENTS-compatible tools — Codex, Cursor, Gemini CLI, Copilot, OpenCode, Factory, ...)
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
- **Portable.** Same map serves multiple agent ecosystems.
- **Lean.** A good `ORIENT.md` is 100–250 lines. Anything larger compresses into nested `<subtree>/ORIENT.md` files.

## Status

`v1.6.1` — `ORIENT.md`, `ORIENT` / `ORIENT-README` managed blocks; adapters narrowed to `AGENTS.md` + `CLAUDE.md`; install guidance tightened. See [`CHANGELOG.md`](./CHANGELOG.md).

## License

MIT. See [`LICENSE`](./LICENSE).

<!-- ORIENT-README:BEGIN v=1 -->
## Repo Map

For structure, routing, and agent guidance, read [`ORIENT.md`](./ORIENT.md).

Agents and contributors should consult `ORIENT.md` before broad repo exploration or large refactors.
<!-- ORIENT-README:END v=1 -->
