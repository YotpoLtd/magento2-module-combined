# Blast Radius profile

## entryPoints

- `composer.json` -- the only thing an outside actor consumes. Packagist reads it at every git tag, and Composer on
  a merchant's Magento installation resolves its `require` block (`README.md:14-23`). There are no HTTP handlers,
  consumers, crons or code of any kind in this repository.

## areas

- package: `composer.json` -- name, release `version`, the two exact pins; every change here is a release
- merchant-docs: `README.md` -- public install and compatibility guide
- harness: `CLAUDE.md`, `AGENTS.md`, `ARCHITECTURE.md`, `docs/`
- pipeline-config: `.agents/adlc/`, `.claude/`, `CODEOWNERS`, `.github/`

## imports

No code and no import graph. The package names its dependencies by Composer package name in `composer.json:10-11`
(`yotpo/module-yotpo-messaging`, `yotpo/module-yotpo-reviews`); those live in other repositories
(`YotpoLtd/magento2-module-messaging`, `YotpoLtd/magento2-module-reviews`) and each pins
`yotpo/module-yotpo-core` (`YotpoLtd/magento2-module-core`). A text search in this repository cannot see their
reach; the reach of a pin change is every merchant who installs the new combined version.

## hotspots

- `composer.json` -- the whole product; a change reaches every merchant who installs or updates to the tagged version

## generatedFiles

None -- no file in this repository is written by a tool.

## tables

### criticality

- 1.0: `composer.json` -- a wrong pin, `version` or `type` makes a public release uninstallable (4.3.11, PR #102)
- 0.7: none
- 0.4: none
- 0.2: `README.md` -- the public merchant guide; wrong install steps or compatibility lines mislead merchants but break nothing
- 0.1: `CLAUDE.md`, `AGENTS.md`, `ARCHITECTURE.md`, `docs/**`, `.gitignore`

### change-type

- 0.95: any change to `composer.json` (`version`, `require`, `name`, `type`); `.github/workflows/**`, `.agents/adlc/**`, `.claude/protected-files.json`, `CODEOWNERS`
- 0.75: none (no shared infrastructure)
- 0.50: none (no logic)
- 0.30: none
- 0.15: `README.md` compatibility or install lines
- 0.05: comments, docs, formatting -- `docs/**`, `*.md` other than `README.md`
