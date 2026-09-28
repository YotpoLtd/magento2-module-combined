# Code Review Fix instructions

Check commands come only from `.agents/adlc/REPO_CHECKS.md`. It names none yet, so a fix
here is verified by reading the change, not by running a command.

## Convention pointers

- `docs/conventions.md` has the Do's and Don'ts and the release pattern (commit `995f6c1`, PR #103).
- Pins are exact versions, and `version` moves with them.

## Stack specifics

- This is a Composer metapackage with no code (`composer.json:13`). There is no build, test, linter or formatter;
  do not add one as part of a fix.
- Verifying a pin needs Packagist metadata
  (`https://repo.packagist.org/p2/yotpo/module-yotpo-<module>.json`), not a local command.

## Protected paths

`.claude/protected-files.json` is the source of truth. For this repo in particular:

- **Never write** (human-required): `composer.json`, `.github/workflows/**`, `.agents/adlc/**`, `CODEOWNERS`,
  `.claude/protected-files.json`. Almost every real finding here needs a `composer.json` change (a pin, the
  `version`), so it is out of scope: report it with the exact change a human should make, and don't work around it.
- **Review-required:** `README.md`, `CLAUDE.md`, `AGENTS.md`, `ARCHITECTURE.md`, `.claude/**`.
- **Never** create, move or delete a git tag; tags are the release.

## On-demand CI run

There is no CI in this repository (no `.github/workflows/`), so there is no on-demand run and no
`ci-on-demand.yaml`. Do not create one.
