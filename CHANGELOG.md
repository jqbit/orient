# Changelog

All notable changes to `orient` are documented here. This project follows [Semantic Versioning](https://semver.org/).

## [1.6.0] — 2026-06-07

Adapter targets narrowed to `AGENTS.md` and `CLAUDE.md`. Every non-Claude agent (Codex, Cursor, Gemini/Antigravity, Copilot, OpenCode, Hermes, Pi, Factory/Droid) is routed through the portable `AGENTS.md`; only Claude Code uses `CLAUDE.md`. `orient` no longer emits or manages `GEMINI.md` or `.cursor/rules/*.mdc` unless the user explicitly asks. The Agent Adapter Targets table, managed-block examples, detection-grep includes, workflow steps, pitfalls, README, and one-shot prompt are all updated to match. Managed-block schema is unchanged (`v=1`); pre-existing `GEMINI.md` blocks are left untouched rather than auto-created.

## [1.5.0] — 2026-06-05

Canonical `orient` branding: `ORIENT.md`, `ORIENT` / `ORIENT-README` managed blocks, `jqbit/orient` URLs. Pre-1.5 pointer blocks and map filenames upgrade in place.

## [1.4.0] — 2026-05-25

Post-release audit pass. Three parallel review tracks (content correctness, repo hygiene, ecosystem alignment) produced 30+ findings; the top 8 high-impact / low-effort items are folded in. Backward-compatible with v1.3.0 — `v=1` blocks unchanged; legacy `v=0` blocks still upgrade in place.

### Changed

- **Algorithm + placement reconciliation.** The `## Managed Block Algorithm` now spells out the byte-level rules — BOM preservation, CRLF vs LF detection, trailing-newline preservation, malformed-input abort behavior — and explicitly folds the placement rule (H1 vs append) into the zero-blocks-found branch. Closes a contradiction in v1.3.0 where an agent following the algorithm step-by-step would always append at end, ignoring the Placement Rule's "under H1" guidance.
- **Hardened legacy-block detection regex.** `references/rebrand-and-pointer-blocks.md` now ships a permissive opener-detector (POSIX character classes, version-attribute-tolerant) that matches across GNU grep, BSD grep, and ripgrep. Also documents the CRLF and UTF-8 BOM edge cases.
- **Ecosystem accuracy pass.** Cursor row in the Agent Adapter Targets table softened (the prescriptive CLI-vs-IDE split was unverified against Cursor docs); `CLAUDE.md` clarified as a community convention, not Anthropic's official skill schema; Gemini global-config sharing between Gemini CLI and Antigravity noted as a known footgun.
- **README polish.** Badges row (license, release, CI checks, CHANGELOG, stars); opening rewritten so "Markdown orientation map for AI coding agents" appears in the first paragraph; Obsidian/vault target now mentioned in the headline agent list to match the SKILL.md table.

### Added

- **[`agents.md`](https://agents.md) citation.** A new paragraph at the top of `## Agent Adapter Targets` names the convention, links the spec, and clarifies that orient layers atop `AGENTS.md` rather than replacing it.
- **Community scaffolding (`.github/`).** `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md` (Contributor Covenant 2.1), `SECURITY.md`, `ISSUE_TEMPLATE/bug-report.yml`, `ISSUE_TEMPLATE/feature-request.yml`, `PULL_REQUEST_TEMPLATE.md`. Community-health score expected to jump from 42% → ≥80%.
- **Dogfood.** This repo now applies orient to itself: a root `ORIENT.md` map, minimal `AGENTS.md` and `CLAUDE.md` adapter files (just the managed block), and a `ORIENT-README:BEGIN v=1` block appended to `README.md`. The repo is its own live demo.
- **CI safety net.** `.github/workflows/checks.yml` runs `markdownlint-cli2` plus `lychee` link checks (internal hard-fail, external warn-only) on every push and pull request. `.markdownlint.json` provides a permissive baseline tuned to this repo's prose-and-table style.
- **`.gitattributes`.** Normalizes line endings (`text=auto eol=lf`), marks `*.md` files as `linguist-language=Markdown` so GitHub's language bar counts them correctly.
- **Branch protection.** `main` now requires linear history; force-pushes and deletions blocked.

### Fixed

- **Nested fence in `references/example-ORIENT.md`.** The outer wrapper now uses 4 backticks so it correctly contains the inner 3-backtick `text` fences (CommonMark spec).
- **README managed-block placement** added per the dogfood pass, demonstrating the ORIENT-README block on the repo's own README.

### Deferred to v1.5+

Signed tag pipeline (GPG/SSH); Anthropic skill-marketplace submission; generated-output CC0 clarification.

## [1.3.0] — 2026-05-25

Initial public release. Closes 10 gaps surfaced in an internal audit of the v1.2.0 skill body.

### Added

- **Managed block algorithm** (`SKILL.md § Managed Block Algorithm`). Explicit, idempotent rule for inserting, replacing, and de-duplicating pointer blocks. Re-running the skill on an unchanged file is now guaranteed byte-identical.
- **Versioned block markers.** All managed blocks now use `<!-- ORIENT:BEGIN v=1 -->` / `<!-- ORIENT:END v=1 -->` (and `ORIENT-README:BEGIN v=1` / `ORIENT-README:END v=1`). Unversioned legacy blocks are detected as `v=0` and upgraded in place. Future schema versions can be added without breaking past content.
- **Adapter-block placement rule.** Concrete rule for where the block goes in new vs existing adapter files (new file → under the first H1; existing file → appended at end with one blank line above/below).
- **Worked example** (`references/example-ORIENT.md`). Fully filled-in `ORIENT.md` for a hypothetical Node monorepo. Calibration target for length, density, and voice.
- **Ecosystem cheatsheet** (`references/by-ecosystem.md`). Per-ecosystem manifest files, source/test conventions, "read first" hints, and common no-go directories for Node/TS, Python, Rust, Go, polyglot monorepos, docs sites, and Obsidian vaults.
- **Drift check** (`SKILL.md § Drift Check`). Five-step verification recipe — Tree Map paths still exist, commands still parse, top-level dirs match disk, no-go zones still real, `Last reviewed:` stamp.
- **Smoke test** (`SKILL.md § Smoke Test`). Per-agent installation verification steps for Claude Code, Codex, Hermes, Cursor CLI, Factory, Pi, OpenCode, Gemini, and Antigravity.
- **Commit stance** (`SKILL.md § Commit Stance`). Explicit guidance that `ORIENT.md` and managed pointer blocks are committed artifacts.
- **Slug source callout** (`SKILL.md § Branding Rules § Slug Source`). The footgun about frontmatter `name:` driving the callable slug is now surfaced at the top of the Branding Rules, not buried in pitfalls.
- **Cursor IDE vs CLI distinction** in the Agent Adapter Targets table.
- **Migration playbook** (`references/rebrand-and-pointer-blocks.md`). Now includes concrete detection regexes, a three-branch decision tree for legacy `ORIENT.md` files, and an explicit string replacement map.

### Changed

- **Subagent fan-out thresholds** are now tied to measurable file counts (`git ls-files | wc -l`) with explicit buckets (<300, 300–2k, 2k–10k, >10k). Token-budget guidance (~25k per explorer report) and harness concurrency-limit notes added.
- **Common Pitfalls** reorganized; legacy items merged, two new items added (`v=1` requirement, drift checks).
- **Verification Checklist** extended to include block versioning and smoke-test coverage.

### Compatibility

- `v=0` (unversioned) blocks are read transparently and upgraded to `v=1` on the next write. No manual migration needed.
- `ORIENT.md`, `orient-map`, and `yourient` legacy spellings are detected by the migration playbook and replaced.

## [1.2.0] — pre-release baseline

Initial internal release covering portable `ORIENT.md` shape, agent adapter targets table, parallel subtree exploration, basic managed blocks, Obsidian/vault variant, and verification checklist. Used internally to rebrand from `orient-map`.
