<div align="center">

<img src="https://raw.githubusercontent.com/cordlybot/.github/main/profile/assets/cordly-mark.png" alt="Cordly" width="128" height="128">

# Cordly

**Describe the change you want. Review exactly what it will do. Approve it. Then watch it happen.**

Cordly is an open, AI-assisted platform for setting up and managing Discord
communities — built so the AI can *suggest* anything and *change* nothing
without your explicit approval.

[![Website](https://img.shields.io/badge/cordly.gg-Visit%20the%20site-22C3F5?style=for-the-badge)](https://cordly.gg)
[![Discord](https://img.shields.io/badge/Discord-Join%20us-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://cordly.gg/discord)
[![License](https://img.shields.io/badge/License-MIT-1F6FEB?style=for-the-badge)](https://github.com/cordlybot/.github/blob/main/LICENSE)
[![Status](https://img.shields.io/badge/Status-Pre--release-F0883E?style=for-the-badge)](#project-status)

</div>

---

## Hey, welcome 👋

Setting up a Discord server is tedious. Channels, categories, roles,
permissions, automod rules, welcome flows, tickets — it is a lot of clicking,
and one wrong permission overwrite can quietly open a private channel to
everyone.

Cordly lets you say what you want in plain language:

> *"Add a support section with a private staff channel and a ticket button."*

Instead of immediately doing it, Cordly hands you a **proposal**: the exact
channels it would create, the precise permission overwrites it would apply,
the risks it spotted, and the reasoning behind each step. Nothing touches your
server until you press approve. Every applied change lands in an audit trail
you can read later.

That is the whole idea. **AI proposes. You decide. The bot executes.**

## Why we built it this way

Handing an AI model direct write access to a Discord server is a bad trade.
Models misread intent, get confused by prompt injection hiding in a message
someone posted, and occasionally invent things with total confidence. The cost
of a mistake is a deleted channel or a leaked private conversation.

So Cordly separates the four things that usually get mashed together:

| | Responsibility | Can it change your server? |
| :--- | :--- | :--- |
| 🧠 **Intent** | You describe the outcome you want | — |
| 📋 **Planning** | The AI service turns that into a typed, validated proposal | **No** |
| ✅ **Authority** | The control-plane API records your explicit approval | **No** |
| ⚙️ **Execution** | The Discord bot applies the approved plan | **Yes — and only this** |

These are separate services that talk over versioned contracts. The AI service
has no path to Discord. The bot has no way to invent an approval. An approval
is bound to one exact proposal, so a plan cannot be swapped after you have
signed off on it.

A few properties we treat as non-negotiable:

- **AI output is untrusted input** until schema and policy validation passes.
- **Only an explicit, immutable approval** makes a plan executable.
- **Only the bot mutates Discord** — nothing else gets write access.
- **Every action** is guild-scoped, authorized, idempotent, observable, and audited.
- **Services never reach into each other's database** or source tree.

## How a change actually flows

```
   You                AI service            Control plane            Bot
    │                     │                      │                    │
    │  "add a support     │                      │                    │
    │   section"          │                      │                    │
    ├────────────────────>│                      │                    │
    │                     │  typed proposal      │                    │
    │                     │  + risks + evidence  │                    │
    │                     ├─────────────────────>│                    │
    │                     │                      │                    │
    │  <── review the diff, risks and reasoning ─┤                    │
    │                     │                      │                    │
    │  approve (bound to this exact proposal)    │                    │
    ├───────────────────────────────────────────>│                    │
    │                     │                      │  idempotent action │
    │                     │                      ├───────────────────>│
    │                     │                      │                    ├──> Discord
    │  <────────── full audit trail ─────────────┤<───── result ──────┤
```

Re-deliver the same action twice and nothing happens twice. Lose a connection
mid-apply and the retry converges instead of duplicating. Fail a permission
check and you get a clear reason, not a half-built server.

## What's inside

Cordly is a poly-repo: each service is independently versioned and deployable,
with boundaries enforced by contracts rather than good intentions.

| Repository | Stack | What it does |
| :--- | :--- | :--- |
| `cordly-bot` | Rust · Serenity | Discord gateway, commands, and the only writer to Discord |
| `cordly-api` | C# · ASP.NET Core | Control plane: auth, authorization, state, approvals, audit |
| `cordly-ai` | Python · FastAPI | Model orchestration, retrieval, planning, evaluations |
| `cordly-knowledge` | Versioned artifacts | Templates, references, schemas, evaluation cases |
| `cordly-panel` | Angular | The authenticated dashboard where you review and approve |
| `cordly-www` | Angular · SSR | The public site at [cordly.gg](https://cordly.gg) |
| `cordly-cloud-ops` | Compose · Helm · Terraform | Local and production operations |
| `cordly-docs` | MkDocs | Architecture, contracts, decisions, runbooks |
| [`.github`](https://github.com/cordlybot/.github) | Org metadata | Community health files and reusable CI |

PostgreSQL is the system of record. Frontends talk only to the API.

> [!NOTE]
> **We are open-sourcing this in stages.** `.github` is public today; the
> service repositories are being prepared for public release and will appear
> here as they land. The goal is for practically the entire platform — bot,
> API, AI service, panel, and infrastructure — to be readable by anyone. If you
> want to be around when a repo opens up, [join the Discord](https://cordly.gg/discord).

## Project status

Cordly is **pre-release and under active development**. It is not ready to run
your community yet, and we would rather say so plainly than ship a demo that
breaks someone's server.

Where things stand:

- ✅ **Foundation** — independent repositories, container images, a full local
  stack, and the first validated knowledge artifact
- 🚧 **Trustworthy backend** — restoring the bot's feature set on stronger
  boundaries, building out the control plane, and hardening the AI runtime
- ⏭️ **One safe proposal** — a complete authenticated path from a described
  change to an audited, approved, idempotent apply
- ⏭️ **Management modules** — leveling, autoroles, welcome flows, reaction
  roles, automoderation, tickets, and full server blueprints

Progress is measured by safe, observable end-to-end behavior — not by how many
repositories exist.

## Getting involved

We would genuinely love the help, and there is no such thing as too small a
contribution here.

- 🐛 **Found something broken?** Open a bug report on the relevant repository.
- 💡 **Have an idea?** Feature proposals and architecture discussions are both
  welcome — describe the problem and who it affects, and we will work out the
  solution together.
- 📖 **Docs, typos, and unclear explanations** are real contributions. If
  something confused you, it will confuse the next person too.
- 🔒 **Security research** is welcome within the boundaries in our
  [security policy](https://github.com/cordlybot/.github/blob/main/SECURITY.md).
  Please report privately — never in a public issue.
- 💬 **Just want to talk?** The [Discord](https://cordly.gg/discord) is open.
  Questions from newcomers are not an interruption; they are the point.

Start with the [contributing guide](https://github.com/cordlybot/.github/blob/main/CONTRIBUTING.md),
and note that participation is covered by our
[code of conduct](https://github.com/cordlybot/.github/blob/main/CODE_OF_CONDUCT.md).

<div align="center">

---

**[Website](https://cordly.gg)** · **[Discord](https://cordly.gg/discord)** · **[Contributing](https://github.com/cordlybot/.github/blob/main/CONTRIBUTING.md)** · **[Security](https://github.com/cordlybot/.github/blob/main/SECURITY.md)** · **[Support](https://github.com/cordlybot/.github/blob/main/SUPPORT.md)**

<sub>Built with care for people who run communities. Cordly is not affiliated with or endorsed by Discord Inc.</sub>

</div>
