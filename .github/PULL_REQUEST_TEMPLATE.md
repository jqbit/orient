# Pull request

## Summary

<!-- One paragraph: what changes, and why. -->

## Type of change

- [ ] Bug fix (corrects a wrong or ambiguous statement in the skill body or references)
- [ ] New ecosystem support (adds a row to the Agent Adapter Targets table)
- [ ] Documentation improvement (clarity, typos, broken links)
- [ ] Algorithm or schema change (managed blocks, drift check, smoke test)
- [ ] Tooling / CI / repo hygiene

## Checklist

- [ ] `SKILL.md` frontmatter `version:` bumped if the change is behavioral (see CONTRIBUTING § Versioning).
- [ ] `CHANGELOG.md` updated with an entry under the appropriate version.
- [ ] If `references/` files changed, the body of `SKILL.md` still links to them correctly.
- [ ] Markdown lints clean locally (`npx markdownlint-cli2 "**/*.md"`).
- [ ] No new external-link rot (relative links resolve; external URLs respond 200 or are documented as transient).
- [ ] For schema/algorithm changes: legacy `v=0` blocks still upgrade correctly per the documented algorithm.

## Related issues / discussions

<!-- Link to the issue this PR closes or the discussion it follows. -->

## Reviewer notes

<!-- Anything the reviewer should pay extra attention to. Edge cases, tested ecosystems, known follow-ups. -->

