# magento2-module-combined

## Purpose
The `yotpo/module-yotpo-combined` Composer **metapackage** for Magento 2 (`composer.json:2,13`). It contains no PHP code: it only pins one exact version of `yotpo/module-yotpo-messaging` and one of `yotpo/module-yotpo-reviews` (`composer.json:9-12`), so a merchant installs the whole Yotpo extension with `composer require yotpo/module-yotpo-combined` (`README.md:14-23`). Packagist publishes every git tag as a version (for example `4.3.13` at `bd6eb77`), so a merged and tagged change is a release that merchants install. The repository is **public**. Owned by team Orbits (`CODEOWNERS`).

## Stack
- Language: none. The only artifact is `composer.json` (`"type": "metapackage"`)
- Package manager: Composer; published on Packagist from git tags
- Key dependencies (other repositories, pinned exactly):
  - `yotpo/module-yotpo-messaging` (`YotpoLtd/magento2-module-messaging`)
  - `yotpo/module-yotpo-reviews` (`YotpoLtd/magento2-module-reviews`)
  - both of them pin `yotpo/module-yotpo-core` (`YotpoLtd/magento2-module-core`) exactly
- CI: none. There is no `.github/` directory, no test suite and no linter

## Directory Structure
Top-level only (run `ls -A` to verify if stale):
- `composer.json`: the metapackage: name, `version`, and the two pinned requirements
- `README.md`: public install and usage guide for merchants
- `CODEOWNERS`: code owners for the human-required paths
- `docs/`: harness docs (conventions, troubleshooting, metrics, ADRs, spec template)
- `.agents/adlc/`: ADLC pipeline contract files
- `.claude/protected-files.json`: criticality tiers

## Key Files
- `composer.json:4`: `version`. Must equal the git tag of the release (see `docs/conventions.md`)
- `composer.json:9-12`: the two exact pins. Both must exist on Packagist and require the same `yotpo/module-yotpo-core`
- `README.md:6-10`: Magento / module version compatibility lines shown to merchants
- `.claude/protected-files.json`: what an agent may and may not change

## Don't Touch
- `composer.json`: human-required. Every change is a release that merchants install
- `CODEOWNERS`, `.claude/protected-files.json`, `.agents/adlc/**`: human-required pipeline configuration

## Quick Start
There is nothing to build, test, lint or run: no command is defined in any CI step, script or doc of this repository. `.agents/adlc/REPO_CHECKS.md` marks what the team still has to name.

- **Build / Test / Lint:** none (see above)
- **Install (merchant side, in a Magento 2 root, never here):** `README.md:14-23`
- **Release:** bump `version` and the pins in `composer.json`, merge the PR (merge commit), then push a git tag equal to `version` on the merge commit (for example tag `4.3.13` on `bd6eb77`, PR #103). Tags have no `v` prefix

## Development Workflow (Spec-Driven)

> Requires: `superpowers` plugin. Install with `/plugin install superpowers@claude-plugins-official`

All feature work, bug fixes, and refactors MUST start with `/superpowers:brainstorming`.
Do NOT write code without first brainstorming and planning.
The Superpowers pipeline guides you through the rest (planning, execution, finishing).

Specs and plans MUST be committed to the feature branch and included in the PR.

## Common Patterns
- Version bump: `version` goes up with every release, even when only one pin changes (commits `995f6c1`, `93d4f48`)
- Pins are exact versions (`"4.3.9"`), never ranges, and must already be published on Packagist (PR #98)
- Before pinning, check that the messaging and reviews versions require the same `yotpo/module-yotpo-core` (PR #102)
- Commits and PR titles follow `type(scope):message`, e.g. `fix(combined):...`, `feat(core):...` (see `git log`). PRs are merged with a merge commit

Full details: [docs/conventions.md](docs/conventions.md).

## Anti-Patterns
See [conventions.md](docs/conventions.md#anti-patterns) for the full list.

## Reference Docs
Read BEFORE starting work on the relevant area:
- [Architecture](ARCHITECTURE.md): package graph, release flow, constraints
- [Docs Index](docs/index.md): entry point for the docs/ folder (conventions, troubleshooting, and any other deep docs)
- Protected Files (`.claude/protected-files.json`): criticality tiers (human-required / review-required / auto-safe); CODEOWNERS mirrors the human-required tier
- [Spec template](docs/specs/TEMPLATE.md): copy per feature. Specs are committed to the PR
