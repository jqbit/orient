# Contributing to yorient

Thanks for your interest. `yorient` is small and focused: a Markdown-only orientation skill for AI coding agents. Most useful contributions fall into a few shapes.

## Where to start

- **Bug or accuracy issue** in `SKILL.md`, a `references/*.md` file, or the example: open an issue using the bug template.
- **New ecosystem support** (a new agent/tool that should appear in the Agent Adapter Targets table): open a feature-request issue with a link to that tool's official context-file docs.
- **Documentation improvement** (clarity, typo, broken link): a PR is fine; an issue first is also fine.
- **Schema change** (a new version of the managed-block format, i.e. `v=2`): please open an issue first to discuss the upgrade path before submitting a PR.

## Repo layout

The canonical files are:

- `SKILL.md` — the skill body. Frontmatter `name:` and `version:` matter for ecosystem compatibility.
- `references/` — `rebrand-and-pointer-blocks.md`, `example-YORIENT.md`, `by-ecosystem.md`. Reference material the skill body links to.
- `README.md`, `CHANGELOG.md`, `LICENSE` — repo-only. Not synced to agent skill installs.
- `YORIENT.md`, `AGENTS.md`, `CLAUDE.md` — the skill applied to its own repo (dogfood).

## Local checks before opening a PR

If you have Node available, run:

```sh
npx markdownlint-cli2 "**/*.md"
```

Otherwise, eyeball the affected files for obvious table/fence problems. CI runs `markdownlint-cli2` and a link checker on every PR.

## Versioning

Semver. Bump rules:

- **Patch (`1.x.y` → `1.x.(y+1)`):** typo, link, formatting, or strictly additive doc that does not change behavior.
- **Minor (`1.x.y` → `1.(x+1).0`):** new section, new ecosystem entry, new reference file, or behavior change that is backward-compatible (legacy `v=0` blocks still work).
- **Major (`1.x.y` → `2.0.0`):** marker format change (e.g., introducing `v=2` semantics that legacy parsers can't read), or any non-backward-compatible algorithm change.

Update `CHANGELOG.md` in the same PR.

## Commit and PR style

- Conventional commit subjects are nice but not required; clear English is fine.
- One change per PR when practical. Sweeping rewrites are harder to review.
- The PR template has a small checklist — please fill it.

## Code of conduct

Contribution to this project is governed by [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md). By participating, you agree to follow it.

## License

By submitting a contribution you agree it is licensed under the same MIT terms as the rest of the project. See [`LICENSE`](../LICENSE).
