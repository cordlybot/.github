# Agent instructions

Guidance for AI coding agents working in this repository. Human contributors
should read [CONTRIBUTING.md](CONTRIBUTING.md) instead — it covers the same
ground with more context.

## What this repository is

`.github` is a GitHub *special repository*. Files here become organization-wide
defaults for every repository under [cordlybot](https://github.com/cordlybot).
A repository that ships its own copy of a file overrides the default; anything
it omits falls back here.

**A change in this repository lands in every repository at once.** Treat that as
the governing constraint on every edit.

## Scope

This repository owns organization metadata, community-health files, issue and
pull-request templates, Dependabot configuration for its own Actions, and
reusable CI workflows. It owns nothing else.

Do not add application code, service configuration, deployment manifests, or
anything specific to a single repository. Those belong to the repository that
owns that behavior.

## Rules

1. **Keep reusable workflows least-privileged and deterministic.** Start from
   `permissions: contents: read` and add a permission only when a step provably
   needs it. Set an explicit `timeout-minutes` on every job.
2. **Pin every third-party action to an immutable commit SHA**, with the
   human-readable version as a trailing comment:
   `uses: actions/checkout@fbc6f39... # v5`. Never reference a mutable tag or
   branch.
3. **Never put secrets, real incident details, private contact information, or
   local absolute paths into an example.** Examples in this repository are read
   by the public.
4. **Quote and isolate untrusted input.** Pass workflow inputs through `env:`
   and reference them as shell variables rather than interpolating `${{ }}`
   directly into a `run:` block.
5. **Respect repository-local overrides.** A repository's own workflow or policy
   file wins; do not "fix" a downstream repository by changing a default here.
6. **Do not fabricate links, contacts, handles, or organization slugs.** The
   organization is `cordlybot`, the domain is `cordly.gg`, and the maintainer is
   `@isaacwallace123`. If a fact is not one of those and not already in the
   repository, ask rather than inventing it.

## Before handing off

- Confirm every YAML file parses and that the `permissions` block on each
  workflow is still minimal.
- Confirm the required community-health files still exist and are non-empty:
  `README.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`,
  `SUPPORT.md`, `GOVERNANCE.md`, `LICENSE`, `CODEOWNERS`, `profile/README.md`.
- Confirm no link in a changed file points at a repository, page, or address
  that does not exist yet.
- Report what changed, what you validated and how, and anything you deliberately
  left undone.

The `organization-health-ci.yml` workflow checks the first two automatically on
every push and pull request. It does not check the third — that one is on you.
