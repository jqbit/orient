# YORIENT.md

## Purpose

This is the public repo for the `yorient` skill — a Markdown-only orientation
layer for AI coding agents. The repo is its own demo: it ships the skill, and
also uses the skill on itself.

## Start Here

- Want to read the skill itself? `SKILL.md`.
- Want to install the skill into your agents? `README.md § Install`.
- Want to extend or modify the skill? `.github/CONTRIBUTING.md`, then `SKILL.md`.
- Want a worked example of a `YORIENT.md`? `references/example-YORIENT.md`.
- Want to migrate from `orient-map` or `ORIENT.md`? `references/rebrand-and-pointer-blocks.md`.
- Tracking releases? `CHANGELOG.md`.

## Tree Map

```text
yorient/
|___SKILL.md                                # the skill body (canonical)
|___references/
|   |___rebrand-and-pointer-blocks.md       # migration playbook + detection regex
|   |___example-YORIENT.md                  # fully worked YORIENT.md (Node monorepo)
|   |___by-ecosystem.md                     # per-ecosystem entrypoint cheatsheet
|___README.md                               # public-facing pitch + install
|___YORIENT.md                              # this file — yorient applied to its own repo
|___AGENTS.md                               # thin adapter (managed block)
|___CLAUDE.md                               # thin adapter (managed block)
|___CHANGELOG.md                            # semver release log
|___LICENSE                                 # MIT
|___.gitattributes                          # line-ending normalization
|___.markdownlint.json                      # CI lint config
|___.github/
    |___CONTRIBUTING.md
    |___CODE_OF_CONDUCT.md
    |___SECURITY.md
    |___PULL_REQUEST_TEMPLATE.md
    |___ISSUE_TEMPLATE/
    |   |___bug-report.yml
    |   |___feature-request.yml
    |___workflows/
        |___checks.yml                      # markdownlint + link check
```

## Routing Map

| If you need to... | Start at | Then check | Avoid |
|---|---|---|---|
| Change the skill body | `SKILL.md` | `references/*.md` for cross-links; update `CHANGELOG.md` | rewriting unrelated sections |
| Add an ecosystem to the adapter table | `SKILL.md § Agent Adapter Targets` | `references/by-ecosystem.md` if a new manifest pattern | inventing rules without an official-docs citation |
| Adjust the managed-block algorithm | `SKILL.md § Managed Block Algorithm` | `references/rebrand-and-pointer-blocks.md` (detection regex) | breaking `v=0` (legacy) compatibility without a major bump |
| Fix a typo or broken link | the affected file | `npx markdownlint-cli2 "**/*.md"` locally | a force-push to `main` |
| Add CI checks | `.github/workflows/checks.yml` | `.markdownlint.json` for rule changes | adding heavy toolchains |

## Commands

- Lint Markdown: `npx markdownlint-cli2 "**/*.md"`
- Check links (lychee available locally): `lychee --offline '**/*.md'` for relative-only, or `lychee '**/*.md'` for full
- Preview the README on GitHub: push a branch, open in browser
- Inspect the released artifact: `gh release view v1.4.0`

## Conventions

- Markdown only. No scripts in the skill workflow (meta scripts in `.github/workflows/` are fine).
- `SKILL.md` and `references/*.md` are the *canonical skill*; they get mirrored into agent skill directories.
- `README.md`, `CHANGELOG.md`, `LICENSE`, `YORIENT.md`, `AGENTS.md`, `CLAUDE.md`, `.github/`, `.gitattributes`, `.markdownlint.json` are *repo-only*; they do not sync to skill mirrors.
- Versioned managed-block markers (`v=1`). Legacy `v=0` blocks are detected and upgraded; never silently rewritten with a different format.
- Semver. `CHANGELOG.md` entry per release.

## No-Go / High-Risk Areas

- `.git/` — never write into it.
- `LICENSE` — do not modify without explicit permission; license changes are PR-blocking.
- Frontmatter `name:` in `SKILL.md` — changing this changes the callable slug across every agent install. Do not rename casually.

## Subsystem Notes

This repo is small enough that no nested `<subtree>/YORIENT.md` files are needed.

## Maintenance

Update this file when top-level files change shape (new `references/*.md`, new repo-only file, ecosystem additions to the adapter table that would change the routing map).

Last reviewed: 2026-05-25.
