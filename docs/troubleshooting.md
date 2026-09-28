---
type: Troubleshooting
title: magento2-module-combined Troubleshooting
timestamp: 2026-09-28T06:56:03Z
---

# Troubleshooting

## Common Issues

| Symptom | Cause | Fix |
|---------|-------|-----|
| `composer require yotpo/module-yotpo-combined` fails: "Your requirements could not be resolved", conflict on `yotpo/module-yotpo-core` | The pinned messaging and reviews versions require different core versions (4.3.11: messaging 4.3.7 → core 4.3.8, reviews 4.3.9 → core 4.3.9) | Release a new combined version whose pins share one core version (4.3.12, PR #102) |
| Install fails: a pinned `yotpo/module-yotpo-messaging` or `-reviews` version is not found | The pin names a version that is not on Packagist yet | Pin a published version and bump `version` (commit `b0d40e9`, PR #98) |
| A new tag does not show up on Packagist | The tag differs from `version` in `composer.json` at that commit; Composer skips such tags [1] | Tag the merge commit with exactly the `version` value |
| Merchants still get the old modules after a merge | The merge is not tagged yet | Push the tag on the merge commit |

## CI Failures

There is no CI in this repository (no `.github/` directory), so nothing fails before a merge. Broken releases
surface only at merchant install time.

## Known documentation drift (not fixed)

| Where | Drift |
|-------|-------|
| `README.md:3-4` | Says the repository "includes the files of the Yotpo extension". It is a metapackage with no files; the code lives in the module repositories. |
| `README.md:8-10` | Lists Magento 2.3.3+ and 2.4.8. PR #96 ("bump version to get compatible with Magento 2.4.9") is not reflected. |

[1] https://getcomposer.org/doc/04-schema.md#version
