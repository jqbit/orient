# Changelog

All notable changes to `yorient` are documented here. This project follows [Semantic Versioning](https://semver.org/).

## [1.3.0] — 2026-05-25

Initial public release. Closes 10 gaps surfaced in an internal audit of the v1.2.0 skill body.

### Added

- **Managed block algorithm** (`SKILL.md § Managed Block Algorithm`). Explicit, idempotent rule for inserting, replacing, and de-duplicating pointer blocks. Re-running the skill on an unchanged file is now guaranteed byte-identical.
- **Versioned block markers.** All managed blocks now use `<!-- YORIENT:BEGIN v=1 -->` / `<!-- YORIENT:END v=1 -->` (and `YORIENT-README:BEGIN v=1` / `YORIENT-README:END v=1`). Unversioned legacy blocks are detected as `v=0` and upgraded in place. Future schema versions can be added without breaking past content.
- **Adapter-block placement rule.** Concrete rule for where the block goes in new vs existing adapter files (new file → under the first H1; existing file → appended at end with one blank line above/below).
- **Worked example** (`references/example-YORIENT.md`). Fully filled-in `YORIENT.md` for a hypothetical Node monorepo. Calibration target for length, density, and voice.
- **Ecosystem cheatsheet** (`references/by-ecosystem.md`). Per-ecosystem manifest files, source/test conventions, "read first" hints, and common no-go directories for Node/TS, Python, Rust, Go, polyglot monorepos, docs sites, and Obsidian vaults.
- **Drift check** (`SKILL.md § Drift Check`). Five-step verification recipe — Tree Map paths still exist, commands still parse, top-level dirs match disk, no-go zones still real, `Last reviewed:` stamp.
- **Smoke test** (`SKILL.md § Smoke Test`). Per-agent installation verification steps for Claude Code, Codex, Hermes, Cursor CLI, Factory, Pi, OpenCode, Gemini, and Antigravity.
- **Commit stance** (`SKILL.md § Commit Stance`). Explicit guidance that `YORIENT.md` and managed pointer blocks are committed artifacts.
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

Initial internal release covering portable `YORIENT.md` shape, agent adapter targets table, parallel subtree exploration, basic managed blocks, Obsidian/vault variant, and verification checklist. Used internally to rebrand from `orient-map`.
