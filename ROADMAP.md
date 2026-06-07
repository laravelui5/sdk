# Roadmap

What's planned for `laravelui5/sdk` on the way to `1.0`, and how each part of the
SDK earns a stable promise. This is direction, not a dated commitment — order is
firm, timing follows the work.

## How the SDK stabilizes

A `1.0` does not mean every surface is frozen on day one. It means every surface is
**honestly labelled** — you always know what you can depend on.

- **Provisional** — shipped and usable; may still change in a minor, with notice.
- **Frozen-Provisional** — validated in our own applications; minor-incremented per
  release, on this roadmap.
- **Frozen** — a public contract under full semantic versioning; breaking it requires
  a major release.

Because the SDK's customers are known and reachable, we graduate surfaces inside that
relationship rather than freezing everything prematurely. A small, genuinely stable
core under a larger, honestly-labelled body is the goal.

## Now — consolidation (`0.16.x`)

The contract surface has been reviewed end to end. The current line documents it,
hardens it, and closes the gaps the review surfaced — no new feature area, a steadier
foundation to build on.

## Planned

Order is settled; each lands as its own minor.

| Line | Focus |
|:--|:--|
| **0.17** | **Identity & impersonation** — partner-validated authorization, acting-as support, and actor-level parameters (slots). |
| **0.18** | **ValueHelp** — the data-binding value-help successor to the global dialog, for search-and-select across your UIs. |
| **0.19** | **Settings personalization** — per-user setting overrides, validated end to end against the settings app. |
| **0.20** | **SQL Query Layer** — a typed query foundation for reporting and analytics. |
| **0.21** | **Reporting API** — parameterized, query-backed reporting surfaces. |
| **0.22** | **Analytics API** — analytic tiles and cards with inlined data. |
| **0.23** | **Database & runtime assurance** — a multi-database test matrix (incl. a PostgreSQL strictness pass) and a multi-PHP-version run. |

Alongside these, the companion UI5 apps (Partners, Settings, address library, auth)
roll out as the surfaces they exercise mature — driving each toward
**Frozen-Provisional**. A surface reaches **Frozen** once it has been proven by
independent consumers.

## Following along

- Releases and breaking-change notes: [`CHANGELOG.md`](./CHANGELOG.md).
- Questions, requests, and bug reports: [open an issue](https://github.com/laravelui5/sdk/issues).
- Documentation: [laravelui5.com](https://laravelui5.com).
