# Changelog

All notable changes to `ux-research-toolkit` documented in this file. Format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) + [Semantic Versioning](https://semver.org/).

> **Note on earlier versions.** This file starts at 2.2.0. Releases 1.x through 2.1.0
> shipped before it existed and are not reconstructed here; `git log` remains the
> record for those.

## [2.2.0] — 2026-08-03

### Added — the screens layer

- **`assets/schemas/layers/screens.schema.json`** — declares a journey-map step that
  is a real product screen: a navigable `path` and an `access` precondition alongside
  the narrative fields. That pairing is what lets a capturer visit the screen and a
  test suite derive its failure modes, without the journey being re-typed into a
  second artefact that then drifts from the first.
- **`assets/schemas/profiles/product-journey-map.profile.json`** — the profile that
  requires the layer, plus the capture contract: output directory, viewport, image
  format, and the access-level → session-environment-variable mapping. A separate
  profile rather than a flag on `journey-map`, because making the layer required on
  the existing profile would invalidate every map already written.

### The `access` vocabulary

Seven levels, ordered least to most restrictive:

| Level | Precondition |
|---|---|
| `pub` | no session |
| `aware` | reachable anonymously, personalised when signed in |
| `auth` | any signed-in account |
| `ent` | a purchased entitlement (an organization plan, a tier) |
| `ver` | a verified or eligible state (accepted terms, joined the event) |
| `scoped` | a specific permission beyond plain admin |
| `admin` | staff |

Measured rather than designed: the first draft had four values, taken from a sample of
one lane, and validating a real 195-screen map rejected 13 screens. The middle four are
not decoration — collapsing them into `auth`/`admin` makes a capturer bring the wrong
session, get a 403, and record a reachable screen as unreachable.

`scoped` is named for the concept rather than for the product surface where it was first
observed. A consuming product without that surface still has permissions scoped below
staff, and the level has to be nameable there too. Repositories implementing the capture
half must use the same token: the mapping is duplicated by hand on the consumer side,
and an unknown level is reported rather than guessed at.

[2.2.0]: https://github.com/DojoCodingLabs/ux-research-toolkit/releases/tag/v2.2.0
