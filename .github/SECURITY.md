# Security Policy

`yorient` is a documentation-only skill: Markdown files plus a small managed-block algorithm specification. It does not ship executable code, does not invoke remote services, and does not handle credentials. The realistic threat surface is therefore narrow:

- **Markdown injection** into a downstream agent's context (e.g., a `YORIENT.md` crafted to mislead an agent into a harmful action). Mitigated upstream by the agents that read the file; report cases where the skill's recommended shape makes injection materially easier.
- **Supply-chain confusion** — a typosquatted clone of this repo or a malicious mirror under the `yorient` name.
- **Documented attack patterns** in the skill body (e.g., recommending an unsafe command).

## Reporting a vulnerability

For anything sensitive, please **do not** file a public issue. Use one of:

1. **GitHub private security advisory** — open at <https://github.com/jqbit/yorient/security/advisories/new> (preferred).
2. **Email** the maintainer via the address on the GitHub profile at <https://github.com/jqbit>.

Include:

- A description of the issue and why you believe it is a security concern.
- Steps to reproduce, including the agent ecosystem and version if relevant.
- The specific file(s) and line(s) involved.
- Your suggested fix, if you have one.

You should receive an acknowledgement within 5 business days. Confirmed reports will be fixed in a patch release.

## Supported versions

The latest `v1.x` release receives fixes. Older majors are not supported.

## Out of scope

- Reports about an agent ecosystem's own behavior unrelated to this skill.
- Reports about user-authored `YORIENT.md` content in third-party repos.
- Reports about commercial support, downtime, or service-level concerns — this is an unstaffed open-source project.
