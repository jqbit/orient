# yorient

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Release](https://img.shields.io/github/v/release/jqbit/yorient)](https://github.com/jqbit/yorient/releases)
[![checks](https://github.com/jqbit/yorient/actions/workflows/checks.yml/badge.svg)](https://github.com/jqbit/yorient/actions/workflows/checks.yml)
[![CHANGELOG](https://img.shields.io/badge/CHANGELOG-md-informational)](./CHANGELOG.md)
[![Stars](https://img.shields.io/github/stars/jqbit/yorient?style=social)](https://github.com/jqbit/yorient/stargazers)

A Markdown orientation map for AI coding agents — drop one `YORIENT.md` at the root of any repository, monorepo, docs tree, or Obsidian vault and let every agent read from the same source of truth. Thin pointer blocks in `README.md`, [`AGENTS.md`](https://agents.md), `CLAUDE.md`, and `GEMINI.md` route Claude Code, Codex, Cursor, Gemini, Copilot, and every other `AGENTS.md`-compatible agent to that map.

No scripts. No generated code. Just Markdown and idempotent, versioned managed blocks that survive across agent ecosystems.

## What it does

- Drops a lean `YORIENT.md` at the root (or any subtree) — purpose, tree map, routing table, commands, no-go zones, drift stamp.
- Adds a thin `<!-- YORIENT:BEGIN v=1 -->` block to your agent-facing files so Claude Code, Codex, Cursor, Hermes, Gemini/Antigravity, OpenCode, Pi, Factory/Droid, GitHub Copilot, and other `AGENTS.md`-compatible tools all read from the same source of truth.
- Adds a `<!-- YORIENT-README:BEGIN v=1 -->` block to `README.md` so human contributors get the same routing.
- Works on Obsidian-style vaults too: `YORIENT.md` plus MOC notes; no folder reorganization required.
- Idempotent: re-running the skill on an unchanged repo produces a byte-identical result.
- Versioned managed blocks (`v=1`) so future iterations can upgrade in place without trampling user content.

## Why

Agents waste enormous time on blind discovery: globbing, grepping, opening files to figure out where the build script lives. A two-minute read of a well-maintained `YORIENT.md` collapses that to near-zero. The pointer blocks make sure every agent — regardless of which `*.md` convention it reads — gets the same routing, and the managed-block algorithm guarantees re-running the skill never corrupts user content.

## Install

`yorient` is a skill folder containing `SKILL.md` plus `references/`. Install it under the skills directory of each agent you use.

```sh
# Clone the repo somewhere durable
git clone https://github.com/jqbit/yorient.git ~/dev/yorient

# Install the skill into the agents you use
# (pick the ones that apply)
mkdir -p ~/.claude/skills && cp -r ~/dev/yorient ~/.claude/skills/yorient
mkdir -p ~/.codex/skills && cp -r ~/dev/yorient ~/.codex/skills/yorient
mkdir -p ~/.gemini/skills && cp -r ~/dev/yorient ~/.gemini/skills/yorient
mkdir -p ~/.gemini/antigravity/skills && cp -r ~/dev/yorient ~/.gemini/antigravity/skills/yorient
mkdir -p ~/.cursor/skills && cp -r ~/dev/yorient ~/.cursor/skills/yorient
mkdir -p ~/.factory/skills && cp -r ~/dev/yorient ~/.factory/skills/yorient
mkdir -p ~/.pi/agent/skills && cp -r ~/dev/yorient ~/.pi/agent/skills/yorient
mkdir -p ~/.config/opencode/skills && cp -r ~/dev/yorient ~/.config/opencode/skills/yorient
mkdir -p ~/.agents/skills && cp -r ~/dev/yorient ~/.agents/skills/yorient
mkdir -p ~/.hermes/skills/software-development && cp -r ~/dev/yorient ~/.hermes/skills/software-development/yorient
```

Each install copies the skill into the corresponding agent's skill tree. The frontmatter `name: yorient` is the callable slug — folder name and frontmatter `name:` must match.

## Use

In any agent that loaded the skill:

```text
/yorient
```

Or, if invoking from a one-shot prompt:

```text
Create and maintain a lean, text-only YORIENT layer for this repo/vault.
Use YORIENT.md as the canonical Markdown map, add or update a short
README.md pointer plus thin AGENTS.md/CLAUDE.md/GEMINI.md adapters for my
target agents, do one broad structural scan using normal file/search tools
only, launch read-only parallel subtree explorers for major domains,
synthesize their Markdown reports into concise ASCII-tree routing docs,
preserve existing content with managed v=1 blocks, and do not create
scripts or non-Markdown helper files.
```

## What gets written

For a typical repo, `yorient` produces:

```text
YORIENT.md                   # canonical map (purpose, tree, routing, commands, no-go, drift stamp)
README.md                    # gains a managed YORIENT-README:BEGIN block pointing to YORIENT.md
AGENTS.md                    # gains a managed YORIENT:BEGIN block (generic + Codex + Hermes + OpenCode + Copilot)
CLAUDE.md                    # gains a managed YORIENT:BEGIN block (Claude Code)
GEMINI.md                    # gains a managed YORIENT:BEGIN block (Gemini / Antigravity)
<subtree>/YORIENT.md         # optional nested map for large subsystems
```

Existing content in those files is preserved. New runs of `yorient` only touch the managed-block regions.

## Repo contents

| File | Purpose |
|---|---|
| `SKILL.md` | The skill itself — instructions an agent reads to do the work. |
| `references/rebrand-and-pointer-blocks.md` | Detection commands, migration decision tree, replacement map. |
| `references/example-YORIENT.md` | Fully worked example for a Node monorepo, as a calibration target. |
| `references/by-ecosystem.md` | Quick reference for manifests, layouts, and commands per ecosystem. |

## Design principles

- **Map first, details on demand.** `YORIENT.md` routes; it does not document.
- **Markdown only.** No scripts, no generated CLIs, no helper code.
- **Idempotent blocks.** Managed blocks at `v=1` upgrade in place; unversioned legacy blocks are detected and rewritten.
- **Portable.** Same map serves every agent ecosystem.
- **Lean.** A good `YORIENT.md` is 100–250 lines. Anything larger compresses into nested `<subtree>/YORIENT.md` files.

## Status

`v1.4.0` — post-release audit closing 8 high-value findings: algorithm/placement reconciliation, hardened detection regex, ecosystem-claim corrections, [`agents.md`](https://agents.md) citation, community scaffolding, dogfood (this repo demonstrates the skill on itself), CI safety net, README polish. See [`CHANGELOG.md`](./CHANGELOG.md).

## License

MIT. See [`LICENSE`](./LICENSE).

<!-- YORIENT-README:BEGIN v=1 -->
## Repo Map

For structure, routing, and agent guidance, read [`YORIENT.md`](./YORIENT.md).

Agents and contributors should consult `YORIENT.md` before broad repo exploration or large refactors.
<!-- YORIENT-README:END v=1 -->
