---
type: Conventions
title: magento2-module-combined Conventions
timestamp: 2026-09-28T06:56:03Z
---

# Coding Conventions

This repository has no code. Its conventions are about `composer.json` and releases.

## File Organization
- One file carries the package: `composer.json` (`name`, `description`, `version`, `license`, `require`,
  `"type": "metapackage"`).
- `README.md` is the public merchant guide; Packagist and GitHub show it.

## Naming
| Thing | Convention | Example |
|-------|-----------|---------|
| Release version | `MAJOR.MINOR.PATCH`, no `v` prefix (every tag from `4.0.0` to `4.3.13`) | `4.3.13` |
| Git tag | exactly the `version` value | tag `4.3.13` at `bd6eb77` |
| Pin | an exact version string, no `^`, `~` or `*` | `"yotpo/module-yotpo-reviews": "4.3.10"` |
| Branch | Jira key + short description | `TS-41-bump-version`, `ORB-8875-bump-version` |
| Commit / PR title | `type(scope):message` | `fix(combined):fix broken 4.3.11 dependency conflict, bump to 4.3.12` |

## Error Handling
Not applicable: there is no code. The failure modes are unresolvable releases (`docs/troubleshooting.md`).

## Logging
Not applicable.

## Testing
There are no tests and no CI. Nothing in the repository checks `composer.json` before a merge. Until a check
exists, a reviewer verifies each pin by hand on Packagist (see Patterns Library).

## Do's and Don'ts
| Do (with file:line ref) | Don't | Why |
|--------------------------|-------|-----|
| Bump `version` (`composer.json:4`) in the same PR as a pin change | Change a pin and keep the old `version` | A published version must not change content; the next tag needs a new number |
| Pin versions that are already on Packagist (`composer.json:10-11`) | Pin a version the module repository has not released yet | Unresolvable install (commit `b0d40e9`, PR #98) |
| Check that both pinned modules require the same `yotpo/module-yotpo-core` | Pin the newest of each without looking at their core pins | 4.3.11 could not be installed (PR #102) |
| Tag the merge commit with exactly the `version` value | Tag a different number, or add a `v` prefix | Composer skips a tag that does not match `version` [1] |
| Update `README.md:8-10` when a release changes Magento compatibility | Leave the requirements lines behind | Merchants read them to pick a version |

## Anti-Patterns

What agents should NEVER do in this repo:
| Anti-Pattern | Why It's Dangerous | See Also |
|--------------|-------------------|----------|
| Editing `composer.json` at all without a human | Every change becomes a public release merchants install | `.claude/protected-files.json` (human-required) |
| Version ranges in `require` | A combined version would install different modules over time | `composer.json:9-12` |
| Removing `"type": "metapackage"` | Composer would install it as a regular package with files, and it has none | `composer.json:13` |
| Adding PHP code, a `vendor/` or a `composer.lock` here | The code belongs in the module repositories; Composer ignores a dependency's `composer.lock` | `ARCHITECTURE.md` Boundaries |
| Creating or moving git tags | Tags are the release; Packagist publishes them | `CLAUDE.md` Quick Start |
| Adding secrets or merchant credentials to the README or docs | The repository is public | README usage section (`README.md:25-29`) |

## Patterns Library

### Version bump release
- **When to use:** a new messaging or reviews release must reach merchants.
- **Canonical implementation:** commit `995f6c1` (PR #103): `version` 4.3.12 → 4.3.13, messaging 4.3.8 → 4.3.9,
  reviews 4.3.9 → 4.3.10.
- **How it works:** bump the pin(s) and `version` in one commit, merge, then tag the merge commit.

### Verifying the pins
- **When to use:** every PR that changes `require`.
- **Canonical implementation:** none in the repository. By hand: open
  `https://repo.packagist.org/p2/yotpo/module-yotpo-messaging.json` and `.../module-yotpo-reviews.json`, find the
  pinned versions, and compare their `yotpo/module-yotpo-core` requirements.
- **How it works:** both versions must exist, and their core pins must be equal.

# Citations

[1] Composer schema, `version` (https://getcomposer.org/doc/04-schema.md#version)
[2] Git history: commits `b0d40e9` (PR #98), `93d4f48` (PR #102), `995f6c1` (PR #103)
