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

Order is settled; each lands as its own minor. **The core is now complete** — identity,
interoperability, and settings (0.19), the surfaces you build against — and gets a round
of per-surface polish on the **0.20** line before it's frozen at **1.0**. The reporting,
analytics, and database-assurance work below arrives as **post-1.0 minors, built as
customers need them** — planned, not gates on the way to 1.0.

| Line | Status                 | Date                   | Focus                                                                                                                                                                                                                                                                                                                                                   |
|:---- |:-----------------------|:-----------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **0.16** | ✅&nbsp;**Shipped**     | 2026&#8209;06&#8209;09 | **Consolidation** – The contract surface has been reviewed end to end. The current line documents it, hardens it, and closes the gaps the review surfaced. No new feature area, a steadier foundation to build on.                                                                                                                                      |
| **0.17** | ✅&nbsp;**Shipped**     | 2026&#8209;07&#8209;14 | **Identity & impersonation** — partner-validated authorization, acting-as support, and actor-level parameters (slots).                                                                                                                                                                                                                                  |
| **0.18** | ✅&nbsp;**Shipped** | 2026&#8209;07&#8209;17 | **LUX Weave — the interoperability layer** — the substrate that lets independent apps compose across shared business concepts. **Value help shipped (0.18.0)** — a searchable picker one module can open from another, with the provider authorizing the open. **The LUX Launchpad's first launcher + a shell sign-out shipped (0.18.1)** — mark a tile to place it on the launcher, and a launch tile opens its app. **Dashboard composition shipped (0.18.3)** — mark a tile or card with `#[Contribute]` and it appears on another module's dashboard, with no change to the app that owns it. **Cross-app navigation shipped (0.18.4)** — a shared object's screen grows a "Related" doorway into every module that relates to it, with no change to the app that owns it. **Marketplace tooling shipped (0.18.5)** — `ui5:concept` maps how your modules connect, and `ui5:describe` publishes a package's marketplace description. |
| **0.19** | ✅&nbsp;**Shipped**     | 2026&#8209;07&#8209;21 | **Settings personalization** — per-user setting overrides plus **scoped value help** (a picker that shows only the rows a user is authorized to see), validated end to end against the settings app. **The last feature of the core — the core is now complete.**                                                                                                                                                                                            |
| **0.20** | 🚧&nbsp;**In&nbsp;progress** | –                      | **Hardening** — a round of per-surface polish across the completed core (Partners, Settings, and their companion apps) before it's frozen at `1.0`. No new surface; steadying what's there. **Partner settings overview shipped (0.20.0)** — a partner's detail lists the settings personalized for them, each linking straight to the Settings console (and back). **Settings console polish shipped (0.20.1)** — provenance shows names, a tidier override editor, and correct value-type labels. **Help scaffolding fix shipped (0.20.2)** — the built-in `ui5:doc`/`ui5:help` guidance now points at the supported way to wire context-sensitive help. |
| **🎯&nbsp;`1.0`** | ⏳ **Waiting**          | –                      | **The core earns its stable promise** — identity, interoperability, and settings, frozen under full semantic versioning. Everything below arrives as a later minor.                                                                                                                                                                                     |
| *(post‑1.0)* | ⏳ **Planned**          | –                      | **SQL Query Layer** — a typed query foundation for reporting and analytics.                                                                                                                                                                                                                                                                             |
| *(post‑1.0)* | ⏳ **Planned**          | –                      | **Reporting API** — parameterized, query-backed reporting surfaces.                                                                                                                                                                                                                                                                                     |
| *(post‑1.0)* | ⏳ **Planned**          | –                      | **Analytics API** — analytic tiles and cards with inlined data.                                                                                                                                                                                                                                                                                         |
| *(post‑1.0)* | ⏳ **Planned**          | –                      | **Database & runtime assurance** — a multi-database test matrix (incl. a PostgreSQL strictness pass) and a multi-PHP-version run, **built on demand**.                                                                                                                                                                                                  |

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
