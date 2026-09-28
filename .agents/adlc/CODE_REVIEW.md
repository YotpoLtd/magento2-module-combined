# Code Review instructions

## Also read

- `ARCHITECTURE.md` -- the package graph (combined → messaging + reviews → core) and the release flow.
- `docs/conventions.md` -- the release conventions, Do's and Don'ts, and the Anti-Patterns table.
- `docs/troubleshooting.md` -- the known failure modes of a release, and the known README drift.
- `.claude/protected-files.json` -- the criticality tiers; `human-required` is authoritative.

## Risk areas

This repository is one file, `composer.json`, and every merged and tagged change to it is a public release:

- **Pins that do not exist** -- a pin to a messaging or reviews version not yet on Packagist makes the combined
  version uninstallable (commit `b0d40e9`, PR #98).
- **Pins that conflict** -- messaging and reviews each pin `yotpo/module-yotpo-core` exactly. 4.3.11 paired messaging
  4.3.7 (core 4.3.8) with reviews 4.3.9 (core 4.3.9) and could not be installed (PR #102).
- **`version` vs tag** -- the release tag must equal `version`; Composer skips a tag that does not match
  (https://getcomposer.org/doc/04-schema.md#version).
- **No CI** -- nothing checks a PR before merge, so review is the only gate.

## Always check

| Condition | Severity |
|---|---|
| A pin in `require` names a version of `yotpo/module-yotpo-messaging` or `yotpo/module-yotpo-reviews` that is not on Packagist | critical |
| The pinned messaging and reviews versions require different `yotpo/module-yotpo-core` versions | critical |
| `require` changed but `version` did not, or `version` went down or repeats a published version | critical |
| A version range (`^`, `~`, `*`, `>=`) in `require` | major |
| `name` or `"type": "metapackage"` changed | critical |
| Any secret, API key or merchant credential in any file (the repository is public). Report the file and line only; never quote the value | critical |
| PHP code, `vendor/` or `composer.lock` added | major |
| A release that changes Magento compatibility without updating `README.md:8-10` | minor |

## Do not report

- **The `version` field being present.** Composer recommends omitting it; this repository has always set it and
  tags match it. Removing it is an owner decision, not a review finding.
- **Exact pins instead of ranges.** Deliberate: one combined version installs one known set of modules.
- **Missing tests or CI.** There is nothing to test; adding CI is an owner decision.
- **The documentation drift listed in `docs/troubleshooting.md`** (`README.md:3-4`, `README.md:8-10`), unless the PR
  touches those lines.
- **`AGENTS.md`**, unless a PR replaces the symlink to `CLAUDE.md` with a real file.
