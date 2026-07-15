# LaravelUi5 SDK

[![Latest release](https://img.shields.io/github/v/release/laravelui5/sdk?sort=semver&label=release)](https://github.com/laravelui5/sdk/releases/latest)
[![License: Commercial](https://img.shields.io/badge/license-commercial-blue)](./LICENSE)

**The productivity layer for enterprise OpenUI5 on Laravel.**

Where [`laravelui5/core`](https://github.com/laravelui5/core) describes — apps, libraries, cards, dashboards, OData — the SDK operationalizes: it turns that static metadata into a living runtime. A database-backed registry, a time-aware permission engine, a lean shell with navigation, search and help, and prebuilt business modules — so teams build enterprise UI5 apps in a Laravel-native workflow, faster and with less ceremony.

For documentation and usage examples, visit [laravelui5.com](https://laravelui5.com).

> **About this repository.** This is the public home of `laravelui5/sdk` — its **roadmap, changelog, and issue tracker**. The SDK is a **commercial package**; its source is distributed privately to licensees via Satis (see *Installation*). Source is not hosted here.

## Features

- **Database-backed artifact registry** — Core's registry persisted, queryable, and cache-compiled for production.
- **Time-aware RBAC** — abilities, roles, and groups resolved as `f(App, Actor, Timestamp)`; every grant time-bound; authorization enforced server-side and projected to the client.
- **LeanShell runtime** — navigation, branding, identity, a command palette (CmdK), and context-sensitive help, assembled from configuration.
- **Multi-locale help** — UUID-keyed topics (stable across refactoring), CommonMark rendering, and a full-text search index.
- **Intent dispatch** — named, authorized cross-module navigation and actions, with a declarative semantic graph.
- **Scoped settings** — configuration with precedence and per-user personalization, type-validated.
- **Row-level data scoping** — fail-closed `#[Scoped*]` entity sets bound to the acting partner.
- **Prebuilt modules & tooling** — Partners, Tenancy, and a synthetic test DSL for your own RBAC.

## Installation

The SDK is a commercial package distributed via the private Satis repository at `packages.pragmatiqu.io`. It requires a **paid LaravelUi5 license** from [pragmatiqu.io](https://pragmatiqu.io) and builds on [`laravelui5/core`](https://github.com/laravelui5/core).

Add the repository to your `composer.json`:

```json
{
    "repositories": [
        {
            "type": "composer",
            "url": "https://packages.pragmatiqu.io"
        }
    ]
}
```

Bind your license credentials and install:

```bash
composer config http-basic.packages.pragmatiqu.io your-email@example.com your-license-key
composer require laravelui5/sdk
```

`laravelui5/core` is pulled in automatically. The service provider registers via Laravel's package auto-discovery. See the [installation guide](https://laravelui5.com/sdk/installation) for configuration and verification.

## Status & roadmap

The SDK is on the `0.x` line while its contract surface stabilizes toward `1.0`. See [`CHANGELOG.md`](./CHANGELOG.md) for released versions and [`ROADMAP.md`](./ROADMAP.md) for what's planned and how each surface graduates from *Provisional* to *Frozen*.

## Issues & feedback

This repository is the SDK's public **issue tracker**. Bug reports, questions, and feature requests are very welcome — [open an issue](https://github.com/laravelui5/sdk/issues). Because the SDK source is licensed and not hosted here, there are no source pull requests; the conversation happens in issues.

## Security

If you discover a security vulnerability, please send an encrypted mail to *security@pragmatiqu.io* rather than opening a public issue. A public key is available on request until we publish it at https://laravelui5.com/security.

## License

Proprietary and commercial — **all rights reserved**. Use of the SDK is governed by your LaravelUi5 license agreement; see [pragmatiqu.io](https://pragmatiqu.io) for terms. This is not open-source software. (`laravelui5/core` is separately available under BSL 1.1; `laravelui5/odata` under MIT.)
