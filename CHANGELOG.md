# Changelog

All notable changes to `laravelui5/sdk` are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## Versioning

Since `1.0.0` (2026-07-24) the SDK follows semantic versioning strictly: additions ship in a
minor, fixes in a patch, and **a breaking change requires a major**. You can pin `^1.0` and
upgrade without reading ahead.

*Historical note for the `0.x` line: before `1.0`, a breaking change could ship in a minor or
patch release, flagged **BREAKING** inline. That no longer applies.*

## [1.1.0] - 2026-08-18

**The Launchpad now shows a tile only when the user may actually open the app behind it.**

Until this release the launcher showed every launch tile to every signed-in user, whatever their
permissions. Clicking one you were not authorized for came back with *Not authorized*. That cost
you twice: a dead end for the user, and a launcher that told everyone which applications exist in
your installation — including the ones they have no business seeing.

A launch tile is now rendered only if the acting user may open its target app. The decision is
made by the same access check that authorizes the click, reading the `#[Access]` gate you already
declared on the app. What the launcher offers and what the click permits can no longer disagree.

**There is nothing to declare and nothing to configure.** It works off your existing `#[Access]`
attributes. An app without an `#[Access]` gate is open, as before, and its tile stays visible to
everyone.

Do note the behaviour change: tiles for gated apps will disappear for users who lack the grant.
That is the point of the release, but it is a visible change on a running installation — if a tile
you expect vanishes, the grant is missing, not the tile.

**If you write launch tiles:** the gate reads the navigation target from the tile itself, so there
is no second place to declare it and nothing to keep in sync. One shape is not yet covered — a
launch tile that resolves slot values before it renders stays visible, because its target is not
known early enough to decide on. If you have one, tell us; it is the next case we intend to close.

No migration, no sync, no republished assets. Take the release.

## [1.0.2] - 2026-08-17

**The shell now loads the Core UI5 library properly, instead of pulling two of its modules by
hand.**

Every page start fetched `LaravelUi5.js` and `Connection.js` as separate requests, because asking
for a module by name does not tell UI5 to load the library it belongs to — and only loading the
library fetches its `library-preload.js`. The shell now loads the library first. One request
replaces two, the rest of the library comes along for free, and anything else that reaches for it
later is already served.

A failed library load also reports to the console now. Previously it left the shell half-started
and silent, which is the worst way for a failure to present itself.

No API change. Nothing to do on your side beyond taking the release and republishing assets:

```bash
php artisan ui5:publish --force
```

## [1.0.1] - 2026-08-17

**Fixes a migration failure on MySQL and MariaDB.**

Adopting `1.0.0` and running `php artisan migrate` failed while creating the partner-addresses
table. Its `partner_id` column was declared one integer width wider than the `id` column it
references, and MySQL and MariaDB reject a foreign key between mismatched widths (error 3780).
The column now matches. Nothing else changes, and no contract moves — if your migrations already
ran, this release is a no-op for you.

SQLite does not enforce that match, which is why the defect survived a test suite and reached a
release. We have added the gap itself to the roadmap, not just the fix.

**If you already hit this**, the failed run left the table behind *without* recording the
migration, so a plain retry fails on the existing table. Drop it, then migrate again:

```sql
DROP TABLE sdk_partner_addresses;
```

```bash
php artisan migrate
```

## [1.0.0] - 2026-07-24

**The SDK's spine is complete.** `1.0` marks the point where identity, interoperability, and settings
are done. The surfaces you build against are validated in real applications, and earn a stable promise.

`1.0` does not mean every corner is frozen forever. It means every surface is **honestly labeled**. You
always know what you can depend on today, what may still move with notice, and what is planned but not
yet built. A small, genuinely stable spine under a larger, honestly-labeled body is more dependable than
a premature wall-to-wall freeze. This distinction is deliberate.

### What the SDK gives you

The SDK takes the app you *declare* in Core and makes it *run* like enterprise software: authorized,
personalized, multi-tenant ready, and able to compose with other people's modules. Your work stays on your
domain, not the platform beneath it. Piece by piece:

- **Identity, security & platform** — every app you register is gated by partner-validated, time-aware
  permissions, so *who may open it, see a control, or run an action* is resolved from grants at request
  time, never wired in by hand.
- **Runtime & data** — your declared metadata becomes a live database catalog, your OData reads come back
  already scoped to the caller, and settings personalize down to the user, team, app, or tenant.
- **Interaction & dispatch** — every change goes through one typed, transactional action, while navigation,
  intents, and impersonation all ride the same authorized dispatch.
- **Shell & UI composition** — your app opens inside the LeanShell (global navigation, search, dialogs, and help)
  and composes with other vendors' modules through LUX Weave, so a tile, a value help, or a "Related"
  link can reach across module boundaries.
- **Integration points** — is where your app meets the outside world: login, tenancy, address validation, and
  export. You bind *your* provider to the SDK's contract, never locked to one vendor or made to carry a
  dependency you didn't choose.
- **Content & tooling** — context-sensitive help travels with the UI, and a command out of the `ui5:*` command line family
  scaffolds, syncs, or compiles the whole thing.
- **Testing** — and you prove your security model against a real database with the scenario DSL. No
  mocks, because authorization is too important to fake.

### The complete 1.0 surface — seven pillars

The same seven pillars at a glance, their building blocks and each one's honest stability label:

| Pillar | What's inside | Stability                                                     |
|:--|:--|:--------------------------------------------------------------|
| **Identity, security & platform** | Partner-validated, time-aware authorization (`#[Access]` / `#[Act]` / `#[Read]`); the SDK runtime and its seam to Core; partner identity | **Stable** |
| **Runtime & data** | Registry sync to the database catalog; scoped OData reads; scoped settings & personalization; per-actor values | **Stable**                                                    |
| **Interaction & dispatch** | The typed **action** contract — the authorized, transactional write path (OData stays read-only); intents; impersonation; the request edge | **Stable**                                                    |
| **Shell & UI composition** | The **LeanShell** (navigation, search, dialogs, context, help); the **LUX Weave** interoperability layer; the dialog & value-help bases; the **Launchpad** | **Stable**                                                    |
| **Integration points** — *bring your own provider* | Login / auth (a JetStream-style default); multi-tenant tenancy (bring your own DB-switching resolver; a standalone default ships); address validation (bind your licensed provider); data export (CSV built in, xlsx pluggable); code-list customizing — **no forced third-party dependency** | **Stable**                                                    |
| **Content & tooling** | The UUID-keyed, full-text help system; the `ui5:*` command line | **Stable**                                                    |
| **Testing** | The scenario-based security-test DSL | **Open by design**                                            |

### How stability works

Every surface carries one honest label — and at 1.0, every surface but the deliberately-open test DSL is
**Stable**:

- **Stable (soft-freeze)** — validated in our own production applications. The shape is settled and on
  the public roadmap; any change is additive and announced. This is where the whole 1.0 spine sits.
- **Open by design** — public, but deliberately reserved to evolve. At 1.0, this is the security-test DSL.

**Why nothing is "frozen forever" yet — and why that is the point.** A Stable surface hardens into a
full, semantically-versioned freeze once at least two independent teams have built on it: we don't lock a
contract's final details until real outside use has proven them. A 1.0 with an honest soft-freeze is more
dependable than one with a single secretly-shaky "frozen" promise. **The label you read today is the
label we keep.**

## [0.20.4] - 2026-07-23

The closing polish on the 0.20 hardening line before `1.0`: the **LUX Weave** navigation attributes
take their long-term shape, and cross-module "Related" links now work end to end.

### Changed — BREAKING

- **Two clear attributes for the two kinds of cross-module link.** Declaring how your module connects
  to a shared business object (a *Concept*) is now split into two intention-revealing attributes,
  replacing the single attribute with `in`/`out` flags:
  - `#[ConceptEntry(concept, route)]` — *"my app is the home of this Concept"*: the one canonical way
    in to its detail. A Concept has exactly one, enforced at build time.
  - `#[Weave(concept, route, label, icon)]` — *"my app offers a related view of this Concept"*: a
    doorway that shows up in that Concept's "Related" menu. A Concept can gather many.

  The single front door and the many side doors can no longer be confused, and declaring a second
  front door is now a build error instead of a silent surprise.

### Fixed

- **Cross-module "Related" links now navigate.** Choosing a related module from a hub's "Related" menu
  now opens the target correctly; previously the navigation was rejected before it could dispatch.

### Upgrading

- Replace `#[Weave(…, in: true, route: '…')]` with `#[ConceptEntry(concept: '…', route: '…')]`.
- Drop the `out: true` flag from your remaining `#[Weave]` declarations — they are outbound by
  definition now.
- Delete any route-less `#[Weave(…, in: true)]` — the separate "consumer" declaration is gone; a link
  no longer needs one.
- Run `php artisan ui5:sync`, then `php artisan ui5:cache`, to refresh the registry.

## [0.20.3] - 2026-07-23

A small hardening fix on the 0.20 line: the default login provider is now cleanly decoupled from your
application's own `User` class.

### Changed

- **Login provider decoupling.** `EloquentLoginProvider` — the batteries-included default that maps a
  partner to their sign-in — no longer names a specific host `User` type in its type hints; it continues
  to resolve your configured auth model dynamically. This keeps the package free of any assumption about
  your app's model namespace. No behavior change; safe to upgrade.

## [0.20.2] - 2026-07-21

More 0.20 hardening: the built-in help scaffolding now shows the supported way to wire
context-sensitive (F1) help.

### Fixed

- **Correct guidance for context help.** The `ui5:doc` and `ui5:help` command help, and the
  help-page starter template, pointed at a `#[Context(...)]` attribute that no longer exists. They now
  point at the supported approach — wrapping the relevant controls in the `Context` control and giving
  it the help page's id — so a new help page is wired correctly from the start.

## [0.20.1] - 2026-07-21

A round of polish on the settings console, continuing the 0.20 hardening line.

### Changed

- **"Set by" shows a name, not an id.** A setting's provenance now names the person who set the value,
  instead of showing an internal identifier.
- **A tidier override editor.** The create/edit form now matches the shape of the other admin apps —
  aligned label/value columns with the actions in the footer — and its value and partner pickers use the
  standard in-field search affordance.

### Fixed

- **Model-backed setting types display correctly.** In a partner's settings overview, settings that
  reference a business object showed their type as "?"; they now show the correct label.

## [0.20.0] - 2026-07-21

Opens the **0.20 hardening line** — a round of per-surface polish that brings the completed core to a
solid, dependable state on the way to `1.0`. This first release rounds out the Partners app and links
it both ways with the Settings console.

### Added

- **See a partner's personalized settings at a glance.** A partner's detail page now lists every
  setting that has been overridden for that partner — which setting, in which application, and its
  current value and type — in one place. No more hunting to find what a given partner carries.

- **Jump straight from an override to the setting.** Each entry links directly into the Settings
  console at that exact setting, so you go from *"who has this override"* to managing it in a single
  click. Together with the existing jump from a setting's owner back to the partner, the Partners and
  Settings apps now connect **both ways**.

## [0.19.5] - 2026-07-21

Finishes the scoped-picker story from `0.19.3`/`0.19.4`: a setting's chosen list now travels all the
way to the picker it opens. When you edit a setting that points at a business object, the field opens
its picker **to the list the setting declares** — read from the setting itself. So two settings backed
by the same picker can offer different lists (one "your colleagues", another "everyone with a login")
with no code, just each setting's configuration.

### Added

- **A setting's chosen list reaches its picker.** The named list a setting declares (`0.19.3`) is now
  carried on the setting's own data, so its editor opens the picker to exactly that list. Same picker,
  different settings, different lists — decided by configuration, not code.

## [0.19.4] - 2026-07-21

The multi-list picker becomes **usable end-to-end**: a field can now open a picker **to a named
list**, and which list you may open — and which rows it holds — is decided and enforced on the
server, never by the page that opened it.

### Added

- **Open a picker to a named list.** `LaravelUi5.openValueHelp({ namespace, scope })` opens the
  shared picker for a **named list** (its `scope`). The server resolves that name to a real,
  permission-checked data set and hands the picker the rows — the caller names *which list*, never
  *which rows*, so a picker can't be talked into showing more than it should.

- **A shared partner picker with two built-in lists.** One partner picker now offers **your
  colleagues** (the people in your own organization) and **partners that have a login**. Each list
  is a first-class, permission-gated set: a user who may not read a list is refused at the data
  boundary, not handed an empty one.

- **Scoped to who you are, not to what the page claims.** The "your colleagues" list is resolved
  from your **signed-in identity**, never from an id the page passes in — so it always means *your*
  organization and can't be pointed at someone else's. Lists that depend on a chosen subject
  (rather than the signed-in user) are a distinct, explicitly-authorized kind; that difference is
  now a first-class part of how a list is defined.

### Changed

- **A value help is a *shape*, not a list — BREAKING.** The former colleague picker is now the
  general **partner picker**, and "colleagues" is one of its lists. Opening it takes a
  `{ namespace, scope }` pair. If you opened the old colleague-picker namespace directly, switch to
  the partner picker with the `colleagues` scope.

## [0.19.3] - 2026-07-21

Continuing the multi-list pickers from `0.19.2`: a setting that opens a picker can now name **which
list** of that picker it draws from.

### Added

- **Point a setting at a specific list of its picker.** A value help can offer several named lists
  (`0.19.2`); when you bind a picker to a setting, you now name the list the setting draws from — so
  a setting backed by a multi-list picker opens straight to the right one (a "default approver"
  drawing from *colleagues*, say, rather than every partner).

- **The list is part of the binding, so it is never left ambiguous.** Naming a picker for a setting
  now means naming its list as well. A binding that points at a multi-list picker without choosing a
  list is refused rather than left half-specified, so a setting always resolves to one well-defined
  list.

## [0.19.2] - 2026-07-20

Groundwork for **pickers that offer more than one list**. A value help — the picker a field opens
to choose a business object — can now be defined with a declared shape and several **named lists**,
and one module can add a list to a value help owned by another, the same marketplace pattern as
dashboard contributions.

### Added

- **Contribute a named list to a value help.** Mark a data set with `#[ContributesScope]`, naming
  the value help it joins and the list it provides. The set becomes that list, with no change to
  the app that owns the value help. The set's own read permission carries over, so a list a user
  may not read stays protected.

- **Contributed lists are checked against the value help's shape.** A value help declares its shape
  once — the columns a selection returns — and every contributed list is validated against it when
  your app syncs. A list that doesn't match the shape is rejected then, so a mismatched contribution
  is caught at build time rather than surfacing in front of a user.

## [0.19.1] - 2026-07-20

Your data reads can now be **authorized**. Until now, any signed-in user who could reach a data
endpoint could read everything it served. You can now require a specific permission to read a
given data set, and the server enforces it — a user without the permission is refused at the data
boundary, not handed a silently empty list.

### Added

- **Read permissions for a data set.** Require a permission to read any read-only data set: a user
  who holds it reads the set as before, and a user who doesn't is refused. Sets you don't mark stay
  open, so nothing you already expose changes.

- **Honest refusals, not empty results.** A denied read comes back as a clear refusal, so a user —
  and your UI — can tell "you may not see this" apart from "there's nothing here." When a protected
  set is pulled in alongside data a user *is* allowed to see, only the protected part is withheld:
  the rest still loads, with a note about what was held back.

## [0.19.0] - 2026-07-19

Your installation gains a complete **settings administration** surface. Every setting an app
declares becomes browsable, inspectable at each level — from the platform default down to one
person's own preference — and editable in place with the right control for its type. Beneath it
is a scoped configuration model with real authorization, so *who* may change *what*, and at
*which* level, is enforced rather than assumed.

### Added

- **A multi-scope settings console.** Browse every setting by the app that owns it, and for any
  setting see its value at each level that applies — the platform default, then tenant, site, and
  a person's own — in one place. Create, edit, or reset an override at any level your authority
  permits; the platform default is protected and stays the floor to fall back to.

- **The right control for every value.** Editing shows the control a value's type calls for — a
  switch for a flag, a number field, a date picker, or a picker for a value that points at a
  business object — so values are entered correctly instead of typed as free text.

- **Personal settings, and setting them on someone's behalf.** A setting marked personal resolves
  per user, so each person carries their own preference. An administrator with enough authority
  can set or clear a person's value for them — support-desk-grade — and every change records who
  last made it.

- **Write authority, enforced on the server.** Each write is checked three ways: may you touch
  this setting at all, may you write at the level you chose, and — for a personal setting — may
  you write it for another person. The console only ever offers what will actually be allowed.

- **`php artisan ui5:intake` — onboard an installation in one step.** Establish the installation's
  owner organization as a named, repeatable, idempotent command, run before `ui5:sync`. It keeps
  exactly one owner per installation and makes a clean install a simple, ordered sequence.

- **Open a shared object at its own screen, from anywhere.** The navigation companion to the
  "Related" doorways from `0.18.4`: a link can carry a user straight to a shared object's own
  screen — a Partner's detail, say — named only by the object and its key, with no route knowledge
  on the calling side and no change to the app that owns it.

## [0.18.5] - 2026-07-17

Two new commands make the shared business objects in your installation **visible**, and let a package
**describe itself** for the coming marketplace. See how your modules connect today; get listing-ready for
tomorrow.

### Added

- **`php artisan ui5:concept` — see what connects to what.** Run it with no argument to list every shared
  business object (a *Concept* — a Partner, a Product, an Order) installed in your app. Name one to see its
  details and, more usefully, the two sides of its graph: the modules that reach *into* it and the doorways
  other modules contribute *out* from it. A quick, honest map of how your modules relate through the objects
  they share.

- **`php artisan ui5:describe` — publish your package's marketplace description.** Reads the classification
  you declare on a module and writes it into your package's `composer.json`, so a marketplace listing can be
  built from it **without installing the package**. Run it with `--check` in CI and it fails the build if the
  description ever drifts from what your code actually declares — one source of truth, kept honest.

- **Classify a module for the marketplace.** Three optional markers describe how a package should appear:
  `#[Category('domain/capability')]` (where it lists, with an optional sales reference), `#[Tag('…')]`
  (process facets that aid discovery), and `#[Media]` (a listing icon and image gallery from a convention
  folder). Together with the Concepts your module defines, these become its marketplace record — declared in
  your code, published on demand.

## [0.18.4] - 2026-07-17

A business object's screen now grows **cross-app doorways** on its own. Tell the platform that your module
relates to a shared object — a Partner, say — and the Partner's screen offers a **"Related"** link straight
into your module, with **no change to the app that owns the Partner**. It's the navigation companion to the
dashboard contributions from `0.18.3`: ship a module later, and the connection simply appears.

### Added

- **Weave your module to a shared object, and its screen gains a doorway to you.** Declare the shared object
  you relate to (a *Concept*) and the relationship (a *weave*); the object's screen renders a "Related" menu
  with an entry into your module. The list is filtered to what the current user is actually allowed to open —
  a doorway a user may not use simply isn't shown — and the app that owns the object is never edited to gain
  it. Add another relating module next month and its doorway just shows up alongside the rest.

- **A first connection ships with the built-in apps.** A Partner's screen now offers a **"Settings"** doorway
  into the Settings app — a link the Partners app was never touched to add. Remove the relating module and the
  doorway disappears with it.

- **Every shared object carries a readable name** for these menus and for discovery, so a Concept shows up as
  "Business Partner", not an internal identifier.

## [0.18.3] - 2026-07-16

A tile or card from one module can now appear on **another module's dashboard** — the contributing
module simply declares where its tile belongs, and the dashboard grows to include it with **no change
to the app that owns it**. Ship a second module months later and its panel just shows up.

### Added

- **Contribute a tile or card to another module's dashboard.** Mark any tile or card with
  `#[Contribute('<target group>', weight: …)]` — naming the published dashboard group it belongs to —
  and register it on your own module as usual. The platform places it in that group; the module that
  owns the dashboard is never touched to receive it. Contributions order by the `weight` you set (a
  smaller number ranks higher) and follow the group's own tiles.

  The tile stays entirely yours: its data, its content, and its access rule travel with it. A viewer
  who may not see it simply doesn't — a clean gap, never a broken panel — and if one contribution ever
  fails, the rest of the dashboard still renders. Remove the contributing module and its tile
  disappears on its own, with nothing to clean up.

  A first contribution ships with the built-in apps: the Settings app adds a "settings overrides" tile
  to the Partners overview — a panel the Partners app was never edited to include.

## [0.18.2] - 2026-07-16

A packaging refinement that completes the multi-app work from `0.17.6`: the SDK's built-in apps —
Partners, Settings, and the Launchpad — now each carry their own frontend, so every one of them loads
reliably from a standard install. How you use them is unchanged.

### Changed

- **Each built-in app now ships its own frontend.** Partners, Settings, and the Launchpad used to draw
  their compiled UI from one shared place in the package; each now keeps its own, right next to its
  code. The apps look and behave exactly as before — the change simply makes them resolve dependably in
  a plain install, with no development-only setup. Nothing to do on your side.

## [0.18.1] - 2026-07-16

The shell gains a **sign-out**, and the **LUX Launchpad** takes its first real shape. You can now end
your session from inside the app — no browser detour — and place your own tiles on a launcher, where a
launch tile opens its app in a single press.

### Added

- **Sign out from the shell.** The identity menu now offers a sign-out that ends your session cleanly
  and returns you to the start page. Moving between accounts no longer means clearing browser cookies
  by hand.

- **Place a tile on the LUX Launchpad.** Mark any tile with
  `#[Launchpad('<domain>/<capability>', weight: …)]` and it appears on the Launchpad. The platform
  gathers every marked tile into one launcher, ordered by the weight you set — the tile declares where
  it belongs and the Launchpad composes them, with no wiring in between. (The full domain/capability
  tree is on the way; today the launcher is a single flat grid.)

- **Launch tiles that open an app.** A launcher tile can carry a navigation intent, so a single press
  opens the target app: the platform resolves the app and takes you there, after checking you may
  enter. A first, compact launch tile ships for the Partners module.

## [0.18.0] - 2026-07-15

A field can now open a **value help** — a searchable picker that browses a business object and hands
back the selection — and, for the first time, a value help provided by **one module can be opened from
another**. The platform hosts the provider's picker, shows its data with its own translations, and
returns the selection to the caller, even though the two modules share nothing. The provider stays in
control of who may open it. Your code also gains the acting and authenticated partner on the client.

### Added

- **Value help — a picker any field can open.** Build a search help as a small modal — a dialog with
  your list inside — plus a short controller that shapes the list and returns the choice. A field
  opens it by name and `await`s the outcome: the selected items, an empty result (a deliberate
  "clear"), or nothing when the user backs out — so a cancel needs no error handling. Each choice
  comes back as `{ key, text }`, with an optional payload for the cases that need more than a key and a
  label. What the list *is* — its data source, its controls, its filters — stays inside the view, your
  business; the platform only carries the selection back. The write behind a "Create…" button is a
  normal action, so inline creation composes without a special path.

- **Open a value help across modules.** A picker belongs to the module that defines it, but another
  module can open it — the platform instantiates the provider, renders its picker with the provider's
  own data and language, and delivers the selection back to the caller. The provider authorizes the
  open with **its own access rule**, checked when the picker opens: being allowed into the calling
  screen is not the same as being allowed to browse the provider's records, so a cross-module picker
  gates itself. **Re-run `ui5:sync`** so a picker's access rule is applied.

- **The acting and authenticated partner, on the client.** `LaravelUi5.getActor()` and
  `getPrincipal()` now return the current partner — the one you're acting as, and the one who logged
  in — as a small identity you can read in the browser (id, name, first name, avatar, gender). A
  screen can scope a request to the acting user without a round-trip to the server.

## [0.17.23] - 2026-07-14

Two fixes for things that should have just worked. A button you gate to a role now appears for the
people who hold that role — not only the action behind it, the button itself. And help pages render
clean, without a stray paragraph mark in front of every heading.

### Fixed

- **Role-gated controls now reach the people they're meant for.** When you mark a control visible
  only to a given role (the standard `sap.ui.viewModifications` gate), a holder of that role now
  actually sees it. Previously the visibility grant was recorded but never wired to its role, so the
  control stayed hidden even for the admins entitled to it — while the action behind the control was
  correctly allowed. The two now agree: the button shows, and the action runs. **Re-run `ui5:sync`**
  to apply the grant.

- **Help pages no longer show `¶` marks before headings.** Every heading in a compiled help page was
  getting a permalink pilcrow in front of it — clutter with no purpose in the help viewer. It's gone.
  **Re-run `ui5:help`** to recompile.

## [0.17.22] - 2026-07-13

The context your code runs in carries two more things it always needed. An action — or a read-only
screen like a card, tile, or report — now receives the **dimensions it's scoped to** and the
**settings that configure it**, both already resolved for the person acting, without you threading
them through by hand. And per-user settings finally stick: a user's own choices are saved and read
back as theirs.

### Added

- **Read the dimensions a request is scoped to.** Your code can declare the ambient parameters it
  depends on — a sales org, a currency, a reporting period — and receive them already resolved for the
  current request and the acting user. No more passing the same context object down through every
  layer by hand. Opt in where you want them; leave it out where you don't.

- **Settings are available wherever you act, resolved for the acting user.** The configuration declared
  for an artifact — its shared defaults and each user's personal overrides — is handed to your code as
  ready-to-use values, resolved by scope so a user's own choice wins over the shared default. Ask for a
  setting by name and get the real, typed value back.

### Fixed

- **Per-user settings now actually personalize.** A user's own setting override was never being stored
  as personal, and never read back correctly — so everyone effectively shared one value. Overrides are
  now saved per user and visible only to their owner: your setting, your value; the shared default for
  everyone else. If your app lets users tailor their own preferences, this is the release where that
  starts working.

### Changed

- **Requires LaravelUi5 Core 2.2 or newer.** Pin Core to `^2.2` when you upgrade.

### Good to know

- **Dimensions can be influenced by the request, so authorize your writes.** A scoped dimension may be
  supplied on the request — convenient for reads, but on an action that changes data it's your code's
  job to confirm the acting user is allowed to operate on that value before writing. The SDK hands you
  the value; guarding the write is yours (a dedicated, declarative guard for this is planned).

## [0.17.21] - 2026-07-12

Actions gain a voice. When a user runs one of your actions, the SDK now confirms success, explains
failure on the right field, and keeps the lists in view up to date — and for the actions you write,
there's a single, well-defined way to make all of that happen. You'll see the first of it when editing
a partner's web & social profiles; the rest is the foundation everything else builds on next.

### Added

- **Actions confirm success and explain failure — clearly, and in the right place.** A successful
  action shows a brief confirmation. A failed one puts each error **on the field it belongs to** (the
  control turns red with its message) and gathers everything into a **message overview** you open from
  the form — validation problems and business-rule messages side by side, each with its own severity.
  No more silent failures or errors stranded far from the field that caused them.

- **Lists refresh themselves after a write.** When an action changes data, the views showing that data
  update on their own — no manual reload, no stale rows after a save.

- **One contract for the actions you write.** An action handler now receives what it needs — the
  validated input, who is acting — and hands back a single outcome: commit with a confirmation, or roll
  back with messages. The SDK wraps the whole thing in a database transaction, so a rollback leaves
  nothing half-written, and turns the outcome into the exact response the front end expects. Scaffolding
  a new action (`ui5:action`) generates this shape for you.

### Changed

- **Requires LaravelUi5 Core 2.1 or newer.** This release builds on Core 2.1's extensible scaffolding
  and Core's bundled client update; pin Core to `^2.1` when you upgrade.

- **BREAKING (internal action).** The partner console's *Save Profile* action is the first to move to
  the new contract; its success response now returns the saved record's id under `data` and its
  confirmation under `message`. If you called that action directly, read the id from `data.id`.

## [0.17.20] - 2026-07-10

The partner console gains a complete **Authorization** surface — manage a partner's login and
everything they're allowed to do, right where you view them.

### Added

- **Manage a partner's login.** Create a login for a partner — you set the initial password, and they
  must change it on first sign-in — then lock or unlock it, reset the password, or delete it, all from
  the partner's Authorization tab. Deleting a login also ends that partner's active access, so nothing
  is left dangling. Login management works out of the box for a standard Laravel `users` table; an app
  that keeps its logins elsewhere plugs in its own without touching the rest.
- **Assign roles, groups and abilities.** Give a partner a security role, add them to a group, or grant
  an individual ability — and revoke any of them, one row at a time. A revoke *ends* the grant but keeps
  the record, so there's a clear audit trail of who had what and when.
- **Grant abilities with a searchable picker.** The picker lists every ability across your apps,
  live-searchable, with a one-click toggle to grant or revoke — and it flags when an ability is already
  reached through a role or group, so you never grant twice by accident.
- **See exactly what a partner can do — and why.** A *View Effective Permissions* read-out resolves
  everything a partner effectively holds across every app, showing the source of each permission
  (granted directly, via a role, or via a group), with live search.

### Fixed

- **Console actions report errors clearly.** A validation error in a console action — a password that's
  too short, say — now returns a readable message instead of failing quietly.

## [0.17.19] - 2026-07-09

The partner console gains two more ways to organize who a partner **is** and who may **act on their
behalf** — classifications and delegations, both managed right where you read them.

### Added

- **Classify partners at a glance.** Tag a partner as a **customer, supplier, competitor,** or
  **employee** straight from its header, and remove a tag just as easily. The colored badges you
  already saw now have a management panel behind them, so a classification is set where you see it —
  and a partner can carry more than one.
- **Delegate one partner's access to another — managed from both sides.** A *delegation* lets one
  person stand in for another for a set period. The console now shows and manages both directions:
  **who may act as this partner**, and **whom this partner may act as**. Grant a delegation by
  choosing the colleague, the dates, and an optional note that records *why*; end an active one early
  in a single step. Three touches keep it safe and tidy:
  - **In-company only.** The chooser offers just the colleagues from the partner's own organization —
    delegation is a within-the-company arrangement, not an open field.
  - **Planned ahead? It's visible now.** Schedule a delegation to begin later and it appears the
    moment you create it, so nothing you've arranged stays hidden until it starts.
  - **Cancelling one that never started leaves no trace.** End a delegation before it ever takes
    effect and it's simply removed — there's nothing to keep. Ending one that was already in effect
    keeps its record.

## [0.17.18] - 2026-07-09

The partner console becomes a **complete master-data workbench** — every part of a partner's
record can now be created, edited, and organized in place.

### Added

- **Manage a partner's whole record from the console.** Alongside viewing a partner you can now
  **edit its identity**, and add / edit / remove its **web & social links**, its **addresses**, its
  **organizational relationships** (the org chart), and its **contact people** — each in a focused
  panel that opens beside the partner you're looking at, so you never lose your place. Related
  partners are **linked, not retyped**: attach an existing partner as a contact or a relationship
  and release it again later, and jump straight to it with one click. Every list updates the moment
  you save.
- **Create departments.** From an organization you can create a department that belongs to it,
  wired into the org chart in a single step. A department carries its parent company's name, so it's
  always clear which company it belongs to.
- **Archive and restore partners.** Retire a partner you no longer work with — it drops out of the
  working list by default and comes back with a **"show archived"** toggle. Fully reversible, and
  nothing is deleted; downloads follow the same rule, so an export matches what you see on screen.

### Changed

- **Creating a partner now distinguishes companies and people.** *Create a partner* is split into
  **create a company** and **create a person**, so each kind can grow its own rules over time.
  **BREAKING — permissions:** the single *create-partner* permission is replaced by **create-company**
  and **create-person**. Re-grant these wherever you had granted *create-partner*.

## [0.17.17] - 2026-07-08

Your app can now let people **download a filtered list as a file**.

### Added

- **Export a list to a file.** A table's current view — with the filters and search the user has
  applied — becomes a downloadable spreadsheet, saved straight to the browser with no extra tab and
  no page navigation. The work happens on the server: it gathers the *whole* filtered set (not just
  the rows currently loaded on screen), builds the file, and confirms the user is allowed to export
  before handing anything back. Exporting an unfiltered, very large registry is politely refused
  with a *"narrow the list first"* message, so downloads stay fast and deliberate. The partner
  console is the first surface to offer it. Files are **CSV** today — they open directly in Excel
  and every spreadsheet app — and **Excel `.xlsx` is a small add-on your host app can plug in** when
  you want it; the SDK carries no spreadsheet library of its own.

### Changed

- **Requires Core 1.3.0.** The download is triggered through Core's new export entry point, which
  ships in Core 1.3.0. Update Core before upgrading.

## [0.17.16] - 2026-07-07

Opening things becomes one consistent path: ask the platform to **open an artifact by name** — a
dialog, an app — and it resolves it, checks access, and opens the right kind of thing.

### Changed

- **One way to open any artifact.** The global dialogs from 0.17.15 opened through a
  dialog-specific step; that is now the platform's single *open-an-artifact* request, identified by
  the artifact's name — the same identity you already use to call an action. Your app opens a dialog
  exactly as the shell opens an app, and **opening an artifact from the command palette now works**
  (it previously led to a dead end).

- **BREAKING — the call to open a dialog changed.** If you adopted global dialogs in 0.17.15,
  dispatch **`ui5-artifact.open`** with a **`namespace`** parameter, in place of `ui5.dialog.open`
  with `dialog`. One line at your call site; nothing else about dialogs changed.

## [0.17.15] - 2026-07-07

You can now create partners from the console, and the platform gains **global dialogs** — a
shell-native way to open a focused, secured modal interaction from anywhere.

### Added

- **Create partners from the console.** The partner catalogue gains a **Create** action that
  opens a tailored form — one for a company, one for a person, each asking only for the fields
  that kind needs. On save, the new partner is added to the registry and opened, ready to work
  on. Creating a partner is a governed action, so only the users you grant it can add one.

- **Global dialogs.** A new building block: a modal dialog that belongs to an app but opens
  *through the shell*. Opening one is a real, server-authorized step — each dialog carries its
  own access rule, checked before it appears — and the shell can host a dialog even when its
  owning app is not the one on screen. Because a dialog is a first-class, named part of the
  platform, it is reachable the same way as everything else — from an action, a shortcut, or the
  command palette — so a create/edit interaction can be offered wherever it makes sense, declared
  once.

### Changed

- **Requires Core 1.2.0.** Opening a dialog goes through Core's new client navigation entry
  point, which ships in Core 1.2.0. Update Core before upgrading.

## [0.17.14] - 2026-07-07

Actor parameter values get simpler: a per-user convenience store — set a value once and
it is remembered — in place of the validity windows and edit-level governance shipped in
0.17.12.

### Changed

- **Actor parameter values are now a plain per-user store.** A stored value — your
  default currency, timezone, reporting period, or date range — is a convenience you set
  once and the SDK remembers, not an audited grant. Each person holds **one current value
  per parameter**: setting replaces the previous value, clearing removes it. Values still
  resolve automatically through the parameter pipeline (a dashboard, report, or query
  receives "this actor's value" without asking), and an administrator can still set one on
  another actor's behalf. What's gone is the **validity window** and the **edit-level
  governance** from 0.17.12 — a convenience value has no timeline to reproduce and no grant
  to authorize, so a report now reads the value in force *now* rather than the one in force
  at the moment it covers.

- **BREAKING — the actor-parameter reader and writer contracts changed shape, and the
  stored-values table is reshaped.** `ActorParameterWriterInterface` is now
  `set(actor, name, value, context)` and `clear(actor, name, context)` — the
  validity-window arguments and the write authorization are gone, and the acting partner
  is taken from the passed context (provenance only). `ActorParameterReaderInterface` drops
  the point-in-time (`$at`) argument from `get` / `all` / `fill`. Update your call sites if
  you implement or invoke these directly. The stored-values table loses its validity
  columns (one value per person per parameter), so any values captured under 0.17.12–0.17.13
  lose their historical windows — re-set them after upgrading. The `ui5:slot <name>`
  inspector's per-actor table now reads **Partner · Value · By**.

### Removed

- **The time-window and write-authorization guards for actor values**
  (`InvalidTimeWindowException`, `WriteAuthorizationException`) — with no window to validate
  and no write-governance to enforce, both are retired. A parameter's `editable` level
  remains as descriptive metadata but no longer decides who may store a value.

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
