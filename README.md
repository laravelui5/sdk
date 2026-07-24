# LaravelUi5 SDK

[![Latest release](https://img.shields.io/github/v/release/laravelui5/sdk?sort=semver&label=release)](https://github.com/laravelui5/sdk/releases/latest)
[![License: Commercial](https://img.shields.io/badge/license-commercial-blue)](./LICENSE)

**The productivity layer for enterprise OpenUI5 on Laravel.**

Where [`laravelui5/core`](https://github.com/laravelui5/core) describes the SDK operationalizes: it turns that static metadata into a living runtime. A database-backed registry, a time-aware permission engine, a lean shell with navigation, search and help, and prebuilt business modules. So teams build enterprise UI5 apps in a Laravel-native workflow, faster and with less ceremony.

For documentation and usage examples, visit [laravelui5.com](https://laravelui5.com).

> **About this repository.** This is the public home of `laravelui5/sdk` — its **roadmap, changelog, and issue tracker**. The SDK is a **commercial package**; its source is distributed privately to licensees via Satis (see *Installation*). Source is not hosted here.

## Features

Everything the SDK adds rests on a **keystone spine**: authorization, the platform runtime, partner identity, and tenancy. The full surface organizes into **seven pillars**.

| Pillar | What's inside                                                                                                                                                                                                 |
|:--|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Identity, security & platform** | Time-aware RBAC (`#[Access]` / `#[Act]` / `#[Read]`, resolved as `f(App, Actor, Timestamp)`), the database-backed artifact registry, the immutable `SdkContext` runtime, and partner identity                 |
| **Runtime & data** | Registry sync to the database catalog, fail-closed row-level OData scoping, scoped settings with per-user personalization, and per-actor parameters                                                           |
| **Interaction & dispatch** | One typed, transactional **action** contract for every write (OData stays read-only), named authorized intent dispatch, and impersonation                                                                     |
| **Shell & UI composition** | The **LeanShell** (global navigation, command palette, dialogs, context help) and the **LUX Weave** layer that composes independent modules (shared value help, dashboard contribution, cross-app navigation) |
| **Integration points** | Login, tenancy, address validation, data export, and code-list customizing — each a **port you bind your own provider to**; no forced third-party dependency                                                  |
| **Content & tooling** | UUID-keyed, full-text multi-locale help, and the `ui5:*` command line for scaffolding, sync, and compilation                                                                                                  |
| **Testing** | A scenario-based DSL that proves your RBAC against a **real database** — no mocks                                                                                                                             |

The complete domain surface, each part's stability, and how it graduates live in [`ROADMAP.md`](./ROADMAP.md).

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

`laravelui5/sdk` reached **1.0** on 2026-07-23. The spine — identity, interoperability, and settings — is **Stable**: validated in our own applications and versioned semantically (a breaking change requires a major release). Each surface is honestly labelled **Provisional → Stable → Frozen**; see [`ROADMAP.md`](./ROADMAP.md) for what's planned and how each surface stabilizes, and [`CHANGELOG.md`](./CHANGELOG.md) for released versions.

## Issues & feedback

This repository is the SDK's public **issue tracker**. Bug reports, questions, and feature requests are very welcome — [open an issue](https://github.com/laravelui5/sdk/issues). Because the SDK source is licensed and not hosted here, there are no source pull requests; the conversation happens in issues.

## Security

If you discover a security vulnerability, please send an encrypted mail to *security@pragmatiqu.io* rather than opening a public issue. A public key is available on request until we publish it at https://laravelui5.com/security.

## License

Proprietary and commercial — **all rights reserved**. Use of the SDK is governed by your LaravelUi5 license agreement; see [pragmatiqu.io](https://pragmatiqu.io) for terms. This is not open-source software. (`laravelui5/core` is separately available under BSL 1.1; `laravelui5/odata` under MIT.)
