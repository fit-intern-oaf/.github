# .github — org defaults

Holds organisation-wide defaults for the OpenAIFlex GitHub organisations.
Published twice:

- During staging (now): `github.com/fit-intern-oaf/.github`
- From first stable release onward: `github.com/openaiflex/.github`

Both copies are kept in sync from this template directory until the FIT mirror
is decommissioned post-M6. See `project-source-code/README.md` → promotion.

Contents:
templates (CONTRIBUTING, SECURITY, CODE_OF_CONDUCT), issue/PR forms, Dependabot
baseline, and a catalogue of **reusable workflows** that per-component repos
invoke via `workflow_call`.

## Reusable workflows

| Workflow | Purpose | Called by |
|---|---|---|
| `reusable-lint.yml` | Lint Python + Node + YAML + Markdown | Every repo on PR |
| `reusable-unit-test.yml` | Unit tests on Python 3.11/3.12 + Node 20 matrix | Every repo on PR / push to `main` |
| `reusable-container-build.yml` | Build image, push to GHCR, generate SPDX SBOM with syft, sign with cosign (keyless OIDC) | Repos that ship a container |
| `reusable-reuse-check.yml` | REUSE 3.3 compliance check | Every repo on PR |
| `reusable-trivy-scan.yml` | Trivy filesystem + container-image scan | Every repo on PR + weekly schedule |
| `reusable-semantic-release.yml` | semantic-release → tag → CHANGELOG → Zenodo DOI hook | Repos on push to `main` |

Caller example (minimal `ci.yml`). **During staging** reference the FIT org;
at promotion run `core/scripts/flip-to-consortium.sh` to flip all `uses:`
lines in one pass.

```yaml
name: CI
on: [push, pull_request]
jobs:
  lint:
    uses: fit-intern-oaf/.github/.github/workflows/reusable-lint.yml@main
  test:
    uses: fit-intern-oaf/.github/.github/workflows/reusable-unit-test.yml@main
    with:
      python-matrix: '["3.11", "3.12"]'
      node-version: '20'
  reuse:
    uses: fit-intern-oaf/.github/.github/workflows/reusable-reuse-check.yml@main
```

## Required secrets per calling repo

| Secret | Scope | Set up by |
|---|---|---|
| `GITHUB_TOKEN` | Automatic | GitHub |
| `COSIGN_KEYLESS` | None — uses OIDC `id-token: write` permission | Workflow-level permissions block |
| `ZENODO_TOKEN` | Only on repos minting DOIs | WS-A release engineer |

## Pinned action versions

All reusable workflows pin third-party actions to a specific SHA. Dependabot
updates them weekly — see `.github/dependabot.yml`.
