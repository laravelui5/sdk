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

## [0.17.13] - 2026-07-04

User impersonation arrives — a controlled way to act as another partner — and a new
Launchpad becomes the safe place every session lands.

### Added

- **User impersonation.** A privileged user can act as another partner — to reproduce
  what that user sees, verify their access, or work on their behalf. The whole session
  then runs *as* that partner (every permission check resolves against them) while every
  action stays *attributed to* the real user, so an audit never loses who actually acted.
  Who may act as whom is an explicit, **time-bound grant**: an administrator issues a
  delegation — a validity window, a note, and a full history that is added to, adjusted,
  or ended early but never deleted — from the target partner's own record. Holding a grant
  is the entire permission: there is no blanket "impersonate anyone," and you can only ever
  act as those granted to you — impersonation never chains. A user starts, switches, or
  ends it from the shell's identity menu.

- **The Launchpad — a safe landing for every session.** A new single-page launchpad is the
  post-login landing and the destination of every impersonation transition. It is reachable
  by any signed-in user and shows only what the acting partner may open — so beginning to
  act as a partner who cannot open your current app never strands the session: you land on
  the launchpad and continue from what *they* can reach.

- **Shell app navigation.** The shell's navigation rail now switches between apps and opens
  their routes directly, each landing in the right place — subject to the same app-level
  access checks used across the shell.

## [0.17.12] - 2026-07-04

Per-actor parameter values arrive — a governed, time-aware store for the values behind
Core's parameters — and the partner console gains addresses and a parameters view.

### Added

- **Actor parameter values.** Store per-actor values for any Core parameter — an
  actor's default currency, locale, reporting period, and so on. Values are **governed**
  (each parameter carries an edit level that decides who may set it), **time-aware**
  (every value has a validity window, so a historical report reproduces the value that
  was in force at the moment it covers), and **admin-on-behalf** (an administrator can
  set a value for another actor, subject to the edit level). They resolve automatically
  through the parameter pipeline, so a dashboard, report, or query receives "this
  actor's value" without asking for it; an unauthenticated request simply contributes
  nothing and falls through to the defaults.

- **The `ui5:slot` inspector now shows per-actor overrides.** Listing a single
  parameter shows the catalog definition as before, and beneath it every partner's
  stored value alongside the Core default and its validity window.

- **Addresses in the partner console.** A partner's stored addresses now appear on the
  partner detail page — each rendered as correctly line-broken text, tagged by postal
  kind (primary, PO box, c/o, mailing) and validation state.

- **A partner parameters view.** The detail page drills into a partner's parameter
  values: every catalog parameter with the partner's override where one is set and the
  Core default everywhere else, each showing its edit level and validity.

## [0.17.11] - 2026-07-03

Partner addresses arrive as a faithfully-stored attribute of the partner — with a
validation provider you choose and pluggable, country-aware rendering.

### Added

- **A partner address model.** A partner's addresses are now stored as a governed
  attribute of the partner: structured where the world allows it, free-form where it
  doesn't, each tagged by postal kind (primary, PO box, c/o, mailing) — plus a "no
  postal address" fact for GPS-only places such as a construction site or an
  address-to-be. One primary address per partner, and a complete-or-GPS capture rule,
  are enforced on every write.

- **Validation is a provider you choose — bought, never owned.** Address validation
  plugs in through a single provider seam: bind Google, Radar, Mapbox, OpenStreetMap, or
  a national address register, and a picked address is stored faithfully — never bound
  to one vendor's place-id namespace. The SDK ships the seam, not a bundled vendor.
  Manual entry works out of the box with no provider bound at all.

- **Country-aware rendering, without owning worldwide correctness.** Captured addresses
  render to correctly line-broken text through a formatter selected automatically from
  the address's country — a good-enough default out of the box, and a seam to drop in a
  country-specific renderer where you need one.

## [0.17.10] - 2026-07-03

The partner console gains its interface: a searchable, filterable catalogue and a
single-partner detail page.

### Added

- **A searchable partner catalogue.** The partner console now opens on a live list of
  every partner in your tenant, with instant search and filters for companies vs people
  and by structural role — customer, supplier, employee, and the rest — plus a
  current / all / past-and-future view so you can see who holds a role now, ever, or no
  longer. Selecting a partner opens their detail beside the list.

- **A single-partner detail page.** Each partner opens as an object page: identity
  master data in the header (name, type, VAT, e-mail…), the partner's people and
  organisation shown in place — their functional contacts (primary, billing, technical)
  and their employment and membership relationships — and tabs that summarise the
  partner's authorization (security roles, groups, and abilities) and settings, each
  with a live count. Additional detail tabs — parameters and delegations among them —
  are laid out now and fill in as those capabilities land.

### Fixed

- **The partner list sends only what it shows.** The registry read now returns just the
  columns the catalogue displays; master-data fields such as VAT number, phone, and
  birthdate no longer travel to the browser with the list.

## [0.17.9] - 2026-07-01

The LeanShell shell comes up end to end — command palette, help, and navigation —
alongside a new access-gated partner console, plus navigation and help polish.

### Added

- **A partner master-data console, over OData.** The Partners module now exposes the
  full partner registry as an OData entity set (`Partners`), gated by an app-level
  access ability (`partners-admin`). A user who holds the grant opens a live
  master-data console; a user who does not never sees the app at all. Available right
  after `ui5:sync`.

### Changed

- **Infrastructure modules stay out of the navigation rail.** Platform-plumbing
  modules — the login flow, diagnostics — no longer clutter the shell navigation. An
  infrastructure module that genuinely has a screen opts in by implementing the new
  `ShowInNavigation` marker (the Partners console does). Your own business modules are
  unaffected — they always appear.

### Fixed

- **The LeanShell now initializes end to end.** A cluster of start-up issues could take
  the whole shell offline whenever any single piece — the help index, the navigation
  payload, discovery — wasn't ready, leaving the command palette, help, and navigation
  dead together. The shell now assembles cleanly: global search (Cmd+K), contextual
  help (F1), and navigation (Cmd+B) all work.

- **The default user avatar renders again.** The placeholder avatar pointed at a
  location the SDK never served, leaving a broken image in the shell header for anyone
  without a picture. It now ships and publishes with the rest of the shell assets.

- **Help opens as a proper overlay.** The help viewer (F1) docks as a full-height drawer
  above your application instead of rendering inside the page, and its search field is
  clearly visible.

## [0.17.8] - 2026-06-28

A richer partner-relationship vocabulary and a friendlier `ui5:doc`.

### Added

- **Four built-in partner-relationship types.** The Partners module now ships a broader
  set of business-partner connections, so you can model real organisational structure
  without inventing your own:
  - **Shareholder** — holds an equity stake in a company (distinct from a wholly-owned subsidiary).
  - **Member** — belongs to a group, association, or consortium, without employment or ownership.
  - **Successor** — the legal successor after a merger or rename, keeping master-data history intact.
  - **Affiliate** — a related or sister company under a common parent.

  Each ships with built-in context help and is available right after `ui5:sync`.
  (Responsibilities a partner holds *for* an organisation — bill-to, ship-to, primary
  contact — remain partner *roles*, which you declare on your own module.)

### Changed

- **`ui5:doc` now shows the new help document's UUID.** Scaffolding a help page prints its
  generated UUID and file path in the success message, so you can wire it straight into your
  module instead of reading it off the created folder.

## [0.17.7] - 2026-06-28

Context-help for your Customizing catalogs, a leaner built-in partner model, and a
safety guard on primary relationships.

### Added

- **Context-help for Customizing rows.** A catalog entry can now point at a help
  document: implement `helpUuid()` on your `CustomizingEntry` attribute and the
  synced row carries a `help_uuid` that the context-help control resolves at runtime.
  References are validated when you build help (`ui5:help`) — a help id with no
  document fails the build, the same guarantee module-level `#[Help]` already gives
  you. (On an already-migrated database, add the nullable `help_uuid` column to the
  catalog tables you want help on; the feature is optional and stays dormant until
  you do.)

### Changed

- **BREAKING — `CustomizingEntry` now declares `helpUuid(): ?string`.** Any
  Customizing attribute you maintain must add the method (return `null` for "no
  help"). The built-in attributes already do.
- **Primary relationships are now guarded on write.** Saving a second *active,
  overlapping* primary relationship of the same type for one partner — e.g. two
  concurrent primary employers — now fails fast with a clear exception instead of
  silently producing an ambiguous data-access scope. Past (non-overlapping) history
  and non-primary relationships are unaffected.

### Removed

- **BREAKING — the SDK no longer ships a built-in partner-role catalog.** The generic
  roles (`supplier`, `employee`, `department`, `cost_center`, `bill_to`, `ship_to`,
  `payer`, `approver`, `project_lead`) and the `PartnerRoleCode` enum are removed. The
  SDK never used these codes itself, so the vocabulary is yours: declare the roles your
  application actually needs on your own module, using the same `#[PartnerRole]`
  attribute and the same `sdk_partner_roles` table. The relationship-type catalog
  (`employed_by`, `owns`) still ships, and the `#[PartnerRole]` attribute and
  `PartnerRoleScope` enum remain. **If you relied on the built-in roles, declare the
  ones you need before upgrading.**

## [0.17.6] - 2026-06-27

The SDK now delivers its built-in apps through Core's multi-app packaging — the
groundwork for shipping more than one app from the single SDK package (Partners
today, Settings next).

### Changed

- **BREAKING — requires Core `^1.0.6`.** The multi-app packaging lands in Core
  1.0.6; update Core before pulling this release.
- The built-in Partners module moved to the `LaravelUi5\Sdk\Partners` namespace.
  No action needed unless you reference its module/app/manifest classes directly.

## [0.17.5] - 2026-06-27

A cache-command fix and a platform bump.

### Fixed

- `ui5:cache` no longer fails when your registry declares a `#[Slot]`. A slot's
  generated settings carry typed enums (`ValueType` / `Scope` / `EditLevel`), and
  these now round-trip through the cache correctly instead of aborting the build.

### Changed

- **BREAKING — minimum PHP is now 8.4** (`"php": "^8.4"`). Upgrade your platform
  before pulling this release.

## [0.17.4] - 2026-06-25

The SDK now ships its own configuration defaults, so a host no longer hand-authors the shell.

### Added

- Documented config defaults for `navigation_service`, `context`, `discovery`,
  `shell`, `intents`, and `help`, merged automatically into the `ui5.*` namespace.
  A fresh install gets a working LeanShell, discovery, and help out of the box;
  override any key in your own `config/ui5.php` (your value wins).

## [0.17.3] - 2026-06-24

Faster cached authorization, and a firm deploy order.

### Added

- `Security\AbilityRef` and `Security\Contracts\AbilityIndexInterface`. The registry
  cache now bakes the sync-assigned ability ids, so the production `CachedRegistry`
  resolves abilities with **zero per-request queries**.

### Changed

- **Deploy order is now `migrate → sync → cache`.** `ui5:cache` reads the ids that
  `ui5:sync` assigns and fails loudly if run before it. `ui5:cache` now writes a
  single cache file (`bootstrap/cache/ui5.php`).

## [0.17.2] - 2026-06-24

LUX Weave navigation — cross-app navigation from shared business concepts.

### Added

- **`#[Concept(name, model)]`** — declare a shared business concept (a canonical
  name + its Eloquent model) on the module that owns the model. Pure identity: no
  UI, no intents.
- **`#[Weave(concept, route, in/out)]`** — declare a cross-app navigation entry
  point on the contributing Ui5App, named by the target concept. A hub concept
  gains an outbound menu **automatically** from the apps that weave into it — with
  no edits to the hub.
- Registry seams `concept()`, `inboundEntriesForConcept()`,
  `outboundEntriesForConcept()`, `conceptsProvidedBy()`; the `weave.navigate`
  intent; and `context.weave` shell delivery — the current app's outbound entries,
  server-side filtered to what the signed-in actor may access.

### Changed / BREAKING

- **BREAKING — `#[SemanticObject]` is replaced by `#[Concept]`.** `name` is now
  required and the `intents[]` argument is removed (Weave is navigation-only;
  action intents keep their own config-driven registration).
- **BREAKING — `#[SemanticLink]` is replaced by `#[Weave]`**, moved from a model
  relation method onto the Ui5App class and gaining `concept` / `route` / `in` / `out`.
- **BREAKING — registry introspection** `objects()` / `links()` / `intentRefs()` are
  replaced by `concepts()` / `consumers()`; `getNavigationIntents()` is removed.
- **Re-run `ui5:cache` after upgrading** — the cached navigation-graph schema changed.

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
