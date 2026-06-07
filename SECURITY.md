# Security Policy

The LaravelUi5 SDK is an authorization-bearing package — it resolves and enforces
who may see and do what. We take vulnerabilities in it seriously and appreciate
responsible disclosure.

## Reporting a vulnerability

**Please do not open a public issue for security reports.**

Report privately by **encrypted email to `security@pragmatiqu.io`**, using the PGP key
published at https://laravelui5.com/security. GitHub *private vulnerability reporting*
is also enabled on this repository.

Helpful to include:

- the affected version (`composer show laravelui5/sdk`),
- a description of the issue and its impact (e.g. authorization bypass, data
  exposure, privilege escalation),
- steps to reproduce or a proof of concept,
- any suggested remediation.

## What to expect

- **Acknowledgement** within **3 business days**.
- An initial assessment (severity, affected versions) shortly after.
- A coordinated fix and release; we will keep you informed of progress.
- Credit in the release notes if you wish (and consent to disclosure timing).

We ask that you give us a reasonable window to remediate before any public
disclosure.

## Supported versions

The SDK is on the `0.x` line. Security fixes are issued for the **latest released
minor**; older lines are patched only by prior arrangement with the affected
licensee. Once `1.0` ships, this policy moves to standard semantic-versioning support
windows.

| Version | Supported |
|:--|:--|
| latest `0.x` minor | ✅ |
| older `0.x` minors | ⚠️ by arrangement |

## Scope

**In scope** — vulnerabilities in the `laravelui5/sdk` package itself: the
authorization engine, row-level scoping, the shell context/search/intent surfaces,
settings resolution, and the command/HTTP edges it ships.

**Out of scope** —

- **Host misconfiguration.** The SDK's guarantees depend on the host wiring its
  authentication guard, the user→partner binding, the morph map, the middleware
  order, and running `ui5:sync`. See the SDK security docs for these responsibilities.
- **`laravelui5/core` and `laravelui5/odata`** — report against
  [their](https://github.com/laravelui5/core) [repositories](https://github.com/laravelui5/odata).
- **Third-party dependencies** — report upstream; tell us if it affects the SDK so we
  can pin or mitigate.

## A note on client-side visibility

The SDK projects an abilities snapshot and filters navigation for the client. This is
a **UX optimization, never an enforcement boundary**. Server-side checks (the
request middleware, intent dispatch, and query-level scoping) are the actual gate.
Reports that rely solely on bypassing client-side hiding will be assessed against the
server-side behaviour.
