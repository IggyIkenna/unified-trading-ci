# unified-trading-ci

Shared reusable GitHub Actions workflows and composite actions for the `unified-trading-system` fleet.

## Why this repo exists

`unified-trading-pm` was briefly (accidentally) made private on 2026-08-05/06, which broke `quality-gates-v2` for
every other repo in the fleet: GitHub does not allow a public repository to call a `uses:` reusable workflow hosted
in a private repository — there is no setting or token scope that unlocks this, it's a categorical platform rule.

This repo exists so that can never happen again. It holds exactly the files other repos' `uses:` lines resolve
against — nothing else. `unified-trading-pm` (and every other private/sensitive repo in the fleet) can change
visibility freely without ever affecting CI for anyone, because the only thing that needs to stay public is this
small, self-contained repo.

Full context: `unified-trading-pm/plans/active/shared_ci_workflow_repo_extraction_2026_08_06.md`.

## Contents

- `.github/workflows/python-quality-gates-v2.yml` — the reusable Python quality-gates pipeline every repo's own
  `quality-gates-v2.yml` caller invokes via `uses:`.
- `.github/workflows/notify-slack.yml` — a reusable Slack-notification workflow, called locally
  (`uses: ./.github/workflows/notify-slack.yml`) from `python-quality-gates-v2.yml` above. Must stay at this path.
- `.github/workflows/image-build-validate.yml` — the reusable image-build gate.
- `.github/actions/setup-python-tools/` — composite action for Python environment setup.
- `.github/actions/setup-agent-tools/` — composite action for agent-tooling setup.

## Branch model

One branch: `main`. No LDR/staging promotion tiers — there's no application code here to promote through tiers,
just CI YAML. Every caller across the fleet should pin `@main`.

## Updating

These files are still authored/maintained the same way they always were (edited directly, or via
`unified-trading-pm/scripts/workflow-templates/rollout-workflow-templates.sh` where a template governs the file) —
this repo is just where they now live and where callers resolve them from. A change here affects every repo in the
fleet's CI on their next run — treat it with the same care as any other Tier-0 shared dependency.
