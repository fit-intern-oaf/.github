# Contributing to OpenAIFlex

Thanks for your interest. OpenAIFlex is an EU research project (EC-CNECT/2026/OP/0003)
delivering an AI-powered Advanced Distribution Management System built on open
source. We welcome contributions from consortium partners, LF Energy, CRESYM,
and the broader grid/energy community.

## Licensing

We follow **ADR-0003** (project-docs/adr/ADR-0003-licensing-matrix.md). Short version:

- **Net-new OpenAIFlex code** — EUPL-1.2.
- **Forks of upstream projects** (SOGNO, EDC, RE4DE, OET components) — match
  upstream licence (usually Apache-2.0); contribute non-OpenAIFlex-specific
  fixes back to upstream within 10 working days.
- Every source file carries an `SPDX-License-Identifier: <licence>` header on
  line 1 or 2.
- REUSE 3.3 compliance is enforced in CI.

Signing off commits (DCO) is required. Use `git commit -s`. We do not use a CLA.

## Development model

- Trunk-based: `main` is always releasable.
- Short-lived feature branches off `main`. PR opens with a CODEOWNERS reviewer.
- Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`,
  `ci:`, `build:`). semantic-release reads commit messages to compute versions.
- One logical change per PR. Keep PRs under 400 lines of diff where feasible.

## Cross-repo changes

Many changes touch two or more repos (e.g. a connector change + an adapter
profile update). Coordinate as follows:

1. Open a tracking issue in `openaiflex/core` describing the end-to-end change.
2. Open one PR per affected repo, each referencing the tracking issue.
3. The last PR to land must bump the pin in `core/manifest.yaml`.

## Tests

- Unit tests run via `reusable-unit-test.yml` on push + PR.
- Interop tests run from `openaiflex/core` via `task test-interop`; required
  before `main` → tag.
- Lab validation (Aachen lab + FIT test cluster) is manual at M5–M6; see `project-docs`.

## Reporting issues

- **Bugs** — use the bug issue template.
- **Security vulnerabilities** — do NOT open an issue. See `SECURITY.md`.
- **Feature requests** — use the feature issue template; tag the deliverable
  (D1.2, D3.1, etc.) in the title.

## Getting help

- GitHub Discussions on each repo for Q&A.
- `#openaiflex-dev` on LF Energy Matrix / Slack (set up by M6).
- Direct contact: Fraunhofer FIT project lead — see `SECURITY.md`.
