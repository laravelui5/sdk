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

## Planned

Order is settled; each lands as its own minor. **1.0 arrives with Settings (0.19)** —
the point at which the core is complete and carries a stable promise: identity,
interoperability, and settings, the surfaces you build against. The reporting,
analytics, and database-assurance work below arrives as **post-1.0 minors, built as
customers need them** — planned, not gates on the way to 1.0.

| Line | Status                 | Date                   | Focus                                                                                                                                                                                                                                                                                                                                                   |
|:---- |:-----------------------|:-----------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **0.16** | ✅&nbsp;**Shipped**     | 2026&#8209;06&#8209;09 | **Consolidation** – The contract surface has been reviewed end to end. The current line documents it, hardens it, and closes the gaps the review surfaced. No new feature area, a steadier foundation to build on.                                                                                                                                      |
| **0.17** | ✅&nbsp;**Shipped**     | 2026&#8209;07&#8209;14 | **Identity & impersonation** — partner-validated authorization, acting-as support, and actor-level parameters (slots).                                                                                                                                                                                                                                  |
| **0.18** | 🚧&nbsp;**In&nbsp;progress** | 2026&#8209;07&#8209;15 | **LUX Weave — the interoperability layer** — the substrate that lets independent apps compose across shared business concepts. **Value help shipped (0.18.0)** — a searchable picker one module can open from another, with the provider authorizing the open. **The LUX Launchpad's first launcher + a shell sign-out shipped (0.18.1)** — mark a tile to place it on the launcher, and a launch tile opens its app. **Dashboard composition shipped (0.18.3)** — mark a tile or card with `#[Contribute]` and it appears on another module's dashboard, with no change to the app that owns it. Still landing on this line: cross-app navigation between related records. |
| **0.19** | ⏳ **Waiting**          | –                      | **Settings personalization** — per-user setting overrides, validated end to end against the settings app. **The last piece of the core — `1.0` lands here.**                                                                                                                                                                                            |
| **🎯&nbsp;`1.0`** | ⏳ **Waiting**          | –                    | **The core earns its stable promise** — identity, interoperability, and settings, frozen under full semantic versioning. Everything below arrives as a later minor.                                                                                                                                                                                     |
| *(post‑1.0)* | ⏳ **Planned**          | –                | **SQL Query Layer** — a typed query foundation for reporting and analytics.                                                                                                                                                                                                                                                                             |
| *(post‑1.0)* | ⏳ **Planned**          | –                | **Reporting API** — parameterized, query-backed reporting surfaces.                                                                                                                                                                                                                                                                                     |
| *(post‑1.0)* | ⏳ **Planned**          | –                | **Analytics API** — analytic tiles and cards with inlined data.                                                                                                                                                                                                                                                                                         |
| *(post‑1.0)* | ⏳ **Planned**          | –                | **Database & runtime assurance** — a multi-database test matrix (incl. a PostgreSQL strictness pass) and a multi-PHP-version run, **built on demand**.                                                                                                                                                                                                  |

Alongside these, the companion UI5 apps (Partners, Launchpad, Settings, auth)
roll out as the surfaces they exercise mature — driving each toward
**Frozen-Provisional**. A surface reaches **Frozen** once it has been proven by
independent consumers.

- ✅ Done — shipped, exit criterion verified
- 🚧 In progress — actively being worked
- ⏳ Not started — queued
- ⚠️ Blocked — waiting on external dependency

## Following along

- Releases and breaking-change notes: [`CHANGELOG.md`](./CHANGELOG.md).
- Questions, requests, and bug reports: [open an issue](https://github.com/laravelui5/sdk/issues).
- Documentation: [laravelui5.com](https://laravelui5.com).
