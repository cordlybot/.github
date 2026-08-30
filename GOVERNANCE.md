# Governance

This document describes how decisions get made in Cordly. It is deliberately
short, because the honest answer right now is short.

## Current model

Cordly uses **maintainer-led governance**, and at present there is a single
maintainer: [@isaacwallace123](https://github.com/isaacwallace123).

That means final decisions on product scope, architecture, security response,
and releases rest with one person. We would rather state that plainly than
imply a committee that does not exist. It also means the process below is
designed to be replaced — see [Evolving this model](#evolving-this-model).

## How decisions are made

Decisions should be **transparent and evidence-based**. In practice:

- **Ordinary changes** are decided in the pull request that makes them.
- **Cross-repository, contract, or trust-boundary changes** need an architecture
  issue first, and the outcome is recorded as an Architecture Decision Record in
  `cordly-docs`. A decision that is not written down did not happen.
- **Disagreements** are resolved by argument and evidence, not seniority. If a
  claim can be settled by a benchmark, a proof of concept, or a test, that beats
  an opinion.

## What can block a change

Some concerns override preference, momentum, and even a majority of opinions in
a thread. A change can be blocked on:

- **Security** — including approval binding and the AI-to-execution boundary
- **Privacy** — user data handling, retention, and minimization
- **Discord platform compliance** — Terms of Service and Developer Policy
- **Tenant isolation** — one guild's data or actions reaching another
- **Irreversible user impact** — anything that could destroy a community's data
  or configuration without a path back

These are the properties the whole design exists to protect. "It is more
convenient" is not a counterargument to any of them.

## Conflicts of interest

Maintainers disclose material conflicts of interest and hand off conduct and
security matters to an uninvolved maintainer wherever possible.

With a single maintainer that is not always possible. Where it is not, the
[Code of Conduct](CODE_OF_CONDUCT.md) documents an external escalation path to
GitHub Support and Discord Trust & Safety, and you should feel free to use it.

## Repository ownership

Each repository has a defined boundary, listed on the
[organization profile](https://github.com/cordlybot). Changes are reviewed by
the owner of the repository they land in. A change touching several boundaries
needs review from each affected owner plus a shared decision record.

## Evolving this model

This document should be revisited as soon as **regular external contributors
emerge**. At that point we expect to add:

- Named maintainers with defined areas, and a documented path to becoming one
- A conduct contact who is not the person any given report might concern
- An explicit process for resolving disagreement between maintainers
- A release and deprecation policy with supported versions

If you are reading this because you want to help maintain Cordly: that is a
conversation worth having. Start in the [Discord](https://cordly.gg/discord).
