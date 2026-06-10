# Changelog

All notable changes to `laravelui5/sdk` are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## Pre-1.0 versioning

While the SDK is on the `0.x` line and its contract surface stabilizes toward `1.0`,
a breaking change may ship in a **minor or patch** release. Every such change is
flagged **BREAKING** inline so you can coordinate the upgrade deliberately. Full
semantic versioning begins at `1.0`.

The `0.16.x` line is the pre-1.0 **stabilization sweep**: a complete review of the
contract surface, with the seams frozen, several defects fixed, and the public APIs
named for the long term.

## [0.17.1] - 2026-06-10

The Customizing engine's first real consumer, plus the i18n tooling that pairs with it.

### Added

- `Partners\Attributes\PartnerRole`
- `Partners\Attributes\RelationshipType`
- `Partners\Enums\PartnerRoleCode`
- `Partners\Enums\RelationshipTypeCode`
- `ui5:i18n` command — expands the i18n keys for the synced Customizing catalogs
  into a base `.properties` bundle.

## [0.17.0] - 2026-06-10

### Added

- `Customizing\Contracts\CustomizingEntry` — the meta-interface a Customizing attribute class fulfils. Consumers ship their own attributes against it.
- `SdkRegistry::customizing()` + the `registerCustomizing()` capture pass in `afterLoad()` Pass 1. Intentionally not exported to the registry cache.
- `Customizing\CustomizingWorker` — the generic flat projection of every declared catalog.

### Changed

- Sync workers relocated into the domains they project. *(Internal relocation: the four worker classes are `@internal`; no public contract changed. In-house consumers only.)*

## [0.16.29] - 2026-06-07

### Changed
- **BREAKING** — the navigation `Entry` value object is now immutable; its
  ability id is set at construction. Only affects code that mutated an `Entry`
  after creating it.

## [0.16.28] - 2026-06-07

### Fixed
- Hardened the help document/asset endpoint against path traversal.
### Added
- First tests for the help compilation pipeline.

## [0.16.27] - 2026-06-07

### Added
- An authoring guide for row-level **scoped entity sets** (the `#[Scoped*]` family).
### Changed
- Internal scope-applier rename for clarity; no consumer impact.

## [0.16.26] - 2026-06-07

### Changed
- **BREAKING** — `IntentResult`'s `type` is now a typed enum (`IntentResultType`).
  The factory methods and the JSON wire shape are unchanged; only direct
  construction or string comparison of `type` is affected.

## [0.16.25] - 2026-06-07

### Added
- `ui5:publish` gains a `--force` flag and asks for confirmation before its
  destructive copy when run interactively (CI/deploy runs proceed unprompted).
- Manuals (`php artisan help …`) for every `ui5:*` command.
### Changed
- All `ui5:*` commands now share a uniform, console-guarded base.

## [0.16.24] - 2026-06-07

### Fixed
- `ui5:sync` now **exits non-zero** when a worker fails (it previously rolled back
  but reported success). Failure is reported concisely, without a stack trace.
### Changed
- **BREAKING (internal API)** — the Sync service/contracts are marked internal; only
  the `ui5:sync` command is a public surface.

## [0.16.23] - 2026-06-07 — Platform / Core seam

### Fixed
- Slots (the Parameter API) were dropped from the cached registry, disabling them in
  production; they now round-trip the cache correctly.
### Changed
- **BREAKING** — the runtime `SdkContext` constructor takes an additional collaborator
  (org-partner resolution). Code that constructs `SdkContext` directly must pass it;
  the resolver/factory and the request pipeline do this automatically.

## [0.16.22] - 2026-06-07 — Security spine

### Removed
- **BREAKING** — the `Use` ability type (route-gating inside a monolith) is retired.
  A capability not all users may exercise is its own app.
### Changed
- **BREAKING** — the capability set is renamed off the overloaded word *scope*:
  `SdkContext::scope()` → `abilities()` (returning an `AbilitySet`). "Scope" now
  refers exclusively to row-level data visibility.
- **BREAKING** — the row-scope attributes (`#[ScopedToActor]`, `#[ScopedToOrgActors]`,
  `#[ScopedByRole]`) moved to the Partners namespace.
### Fixed
- Corrected a visibility query column, a per-request cache, and two model helpers.

## [0.16.21] - 2026-06-07 — Partners & Tenancy

### Added
- A working default tenant resolver, so single-tenancy works out of the box; the
  tenant descriptor now carries a curated organization identity (name, logo, legal
  fields).
### Changed
- **BREAKING** — `Tenant` / `TenantInterface` reshaped to the curated identity
  subset; `getAssetPath()` and `getLocale()` removed (the logo lives on the tenant's
  organization; locale is an installation detail).
- **BREAKING** — `PrimaryEmploymentResolver` renamed to `OrgPartnerResolver`.
