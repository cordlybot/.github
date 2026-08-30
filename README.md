<div align="center">

<img src="https://raw.githubusercontent.com/cordlybot/.github/main/profile/assets/cordly-mark.png" alt="Cordly" width="96" height="96">

# `.github`

**Organization-wide community health files, policies, and reusable CI for [Cordly](https://cordly.gg).**

[![Organization health CI](https://github.com/cordlybot/.github/actions/workflows/organization-health-ci.yml/badge.svg)](https://github.com/cordlybot/.github/actions/workflows/organization-health-ci.yml)
[![License](https://img.shields.io/badge/License-MIT-1F6FEB)](LICENSE)
[![Discord](https://img.shields.io/badge/Discord-Join%20us-5865F2?logo=discord&logoColor=white)](https://cordly.gg/discord)

</div>

---

New here? Start with the [Cordly organization profile](https://github.com/cordlybot)
for what the project actually is. This repository is the plumbing behind it.

GitHub treats a repository named `.github` as special: the files in it become
the **defaults for every repository in the organization**. A repository that
ships its own `CONTRIBUTING.md`, issue templates, or workflows overrides these
defaults; anything it omits falls back here.

## What lives here

| Path | Applies to | Purpose |
| :--- | :--- | :--- |
| [`profile/README.md`](profile/README.md) | Organization page | The public landing page at [github.com/cordlybot](https://github.com/cordlybot) |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | All repositories | How to propose and land a change |
| [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) | All repositories | Community expectations and reporting |
| [`SECURITY.md`](SECURITY.md) | All repositories | Vulnerability reporting and research boundaries |
| [`SUPPORT.md`](SUPPORT.md) | All repositories | Where to ask which kind of question |
| [`GOVERNANCE.md`](GOVERNANCE.md) | Organization | How decisions get made |
| [`CODEOWNERS`](CODEOWNERS) | This repository | Review ownership for these shared files |
| [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE) | All repositories | Bug, feature, and architecture issue forms |
| [`.github/pull_request_template.md`](.github/pull_request_template.md) | All repositories | Default pull-request checklist |
| [`.github/workflows/`](.github/workflows) | Called by other repos | Reusable CI for Rust, .NET, Python, and docs |
| [`profile/assets/`](profile/assets) | Organization page | Brand assets used by the profile README |

## Reusable workflows

Four language-level quality gates live in [`.github/workflows`](.github/workflows).
They are `workflow_call`-only, request `contents: read` and nothing more, and
pin every third-party action to an immutable commit SHA.

| Workflow | Runs | Required input |
| :--- | :--- | :--- |
| [`rust-ci.yml`](.github/workflows/rust-ci.yml) | `cargo fmt --check`, `check`, `clippy -D warnings`, `test` | — |
| [`dotnet-ci.yml`](.github/workflows/dotnet-ci.yml) | `dotnet format --verify-no-changes`, `build --warnaserror`, `test` | `project` |
| [`python-ci.yml`](.github/workflows/python-ci.yml) | `ruff check`, `ruff format --check`, `mypy`, `pytest` | — |
| [`docs-ci.yml`](.github/workflows/docs-ci.yml) | `validate_docs.py`, `mkdocs build --strict` | — |

All four accept `working-directory` (default `.`). Rust accepts `toolchain`,
.NET accepts `dotnet-version`, and Python and docs accept `python-version`.

### Calling one

```yaml
name: CI
on: [push, pull_request]

jobs:
  rust:
    uses: cordlybot/.github/.github/workflows/rust-ci.yml@v1
    with:
      working-directory: .
      toolchain: stable
```

The `v1` tag tracks the latest reviewed v1.x release and is the recommended
reference for repositories inside this organization.

For stricter supply-chain guarantees, pin the exact commit instead. Resolve the
SHA behind a tag with:

```bash
git ls-remote https://github.com/cordlybot/.github refs/tags/v1
```

then reference it with the tag retained as a trailing comment:

```yaml
    uses: cordlybot/.github/.github/workflows/python-ci.yml@<sha-from-above> # v1
```

### What each workflow expects

- **Rust** — a `Cargo.toml` workspace with a committed `Cargo.lock`; every step
  runs `--locked`, and `RUSTFLAGS: -Dwarnings` makes warnings fatal.
- **.NET** — a solution or project path passed as `project`. Formatting is
  verified before the build, and the build runs with `--warnaserror`.
- **Python** — a `pyproject.toml` with a committed `uv.lock`, plus Ruff, mypy,
  and pytest as development dependencies. Dependencies install with
  `uv sync --frozen --all-groups`.
- **Docs** — `mkdocs.yml`, `requirements-docs.txt`, and `tools/validate_docs.py`.
  The build runs `--strict`, so a broken link or an orphan page fails CI.

Repository-specific pipelines are expected to add what these deliberately leave
out: integration tests, database services, security scanning, image builds, and
deployment gates.

## Dependabot

[`dependabot.yml`](.github/dependabot.yml) keeps **this repository's** GitHub
Actions up to date on a weekly schedule.

> [!IMPORTANT]
> GitHub does **not** inherit `dependabot.yml` from this special repository.
> Every application repository must commit its own configuration covering its
> own ecosystems — Cargo, NuGet, uv/pip, npm, Docker, or Terraform — with a
> reviewed schedule and grouping policy.

## Working on this repository

Changes here affect every repository in the organization, so the bar is a
little higher than usual:

1. Keep reusable workflows least-privileged and deterministic. Add a permission
   only when a step genuinely needs it.
2. Pin every third-party action to an immutable commit SHA with the human-readable
   version as a trailing comment.
3. Never put secrets, real incident details, or private contact information into
   an example.
4. Remember that a change to an issue form or the pull-request template lands in
   every repository at once.

[`organization-health-ci.yml`](.github/workflows/organization-health-ci.yml)
runs on every push and pull request. It parses every YAML file in the repository
and asserts that the required community-health files exist and are non-empty.

## License

MIT — see [LICENSE](LICENSE).
