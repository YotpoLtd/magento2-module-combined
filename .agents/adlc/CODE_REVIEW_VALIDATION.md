# Code Review Validation instructions

## Known false positives

- **"`version` should be omitted from `composer.json`."** Composer's general advice; this repository sets it on
  purpose and every tag matches it. Drop unless the PR makes tag and `version` diverge.
- **"Use a version range instead of an exact pin."** Exact pins are the point of the metapackage.
- **"No tests / no CI."** There is no code to test. Not a finding on a PR.
- **"Package has no files / no autoload."** It is `"type": "metapackage"` (`composer.json:13`).
- **README drift already listed in `docs/troubleshooting.md`** (`README.md:3-4`, `README.md:8-10`), unless the PR
  touches those lines.

## Always keep

- a pin to a messaging or reviews version that is not on Packagist;
- pinned messaging and reviews versions that require different `yotpo/module-yotpo-core` versions;
- a `require` change without a `version` bump, or a repeated or lower `version`;
- a changed `name` or `type`;
- any secret or credential in any file (the repository is public).

## Domain notes

- Verify a pin finding against Packagist metadata
  (`https://repo.packagist.org/p2/yotpo/module-yotpo-messaging.json`, `-reviews.json`), comparing each pinned
  version's `yotpo/module-yotpo-core` requirement. The code of those modules is in other repositories.
- A combined version is published when its tag is pushed, after the merge; a merged but untagged change has not
  reached merchants yet.
