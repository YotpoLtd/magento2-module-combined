# Repository check instructions

## Environment

- **No toolchain is pinned in this repository.** It is a Composer metapackage (`composer.json:13`) with no code, no
  `composer.lock`, no PHP version file and no CI. `composer.json` declares no `php` requirement.
- **No variables or credentials are needed.** Every dependency is public on Packagist.
- TODO(ai-dlc): the agent image, and that it is not the runtime image. There is no agent image and no runtime
  image in this repository.

## Always, on every changed file

**There is no formatter and no linter in this repository.** Do not add one, and do not reformat lines as a side
effect of a fix.

```
TODO(ai-dlc): no check command traces to a CI step, script or doc of this repository -- the team names one
```

- **A violation your own edit introduced is part of the finding you are fixing** --
  iterate until it is clean. Never report a finding fixed while its checks are red, and
  never weaken or silence a check to get a commit through.
- **Pre-existing violations in code you did not touch are out of scope.** Leave them;
  fixing them widens the diff past the finding.
- **Reverting is the last resort, not the first move.** Only when the retry budget in
  the `code-review-fixer` skill is spent -- the rule is unclear, or satisfying it would
  change behaviour -- back that finding's edit out and report it unfixed, with the
  check's own message as the reason.

## By kind of change

Run the narrowest thing that covers what you touched.

| Change touches | Run |
|---|---|
| `composer.json` | TODO(ai-dlc): none is documented in CI, scripts or docs; `composer.json` is human-required, so an agent does not change it |
| `README.md` | nothing |
| Markdown, `docs/**` only | nothing |

A change spanning several areas runs each area's row, not the full build.

## Full validation

```
bash .agents/adlc/repo-validation.sh
```

The canonical pre-merge run, and the single definition of "everything passed" -- see the
script's header for the exit-code contract. Not part of the fix loop: run it once at the
end of a pass.

## Do not run

- **Anything needing infrastructure this environment does not have** -- none exists here: there are no
  integration or component suites. The install steps in `README.md:14-23` (`composer require`, `php bin/magento
  ...`) run in a merchant's Magento 2 root, not in this repository. Never run them here.
- **TODO(ai-dlc): dependency re-resolution** unless a dependency actually changed -- it
  re-fetches the whole graph over the network every time. No repo file names this repository's
  re-resolution command.
- **Anything that writes to the default branch**, publishes an artifact, pushes an image,
  or triggers a deploy. Here: creating or pushing a git tag -- Packagist publishes every tag as a release that
  merchants install.
- **`git push --force`**, and any rebase to sync the branch.
