# Contributing to Cordly

First off — thank you. Cordly is a small project with big ambitions about
safety, and every bug report, doc fix, and question makes it better. This guide
explains how things work so you can spend your energy on the interesting part.

If anything here is unclear or feels like a barrier, that is a bug in this
document. Tell us in the [Discord](https://cordly.gg/discord) and we will fix it.

## Ways to contribute

You do not need to write Rust, C#, or Python to help.

| | |
| :--- | :--- |
| 🐛 **Report a bug** | Open a bug report on the repository that owns the behavior |
| 💡 **Propose a feature** | Describe the problem and who it affects — we will work out the solution together |
| 🏛️ **Raise an architecture question** | Use the architecture issue form for anything crossing repository boundaries |
| 📖 **Improve the docs** | If something confused you, it will confuse the next person. This counts |
| 🔍 **Review a pull request** | A second pair of eyes on permissions and destructive actions is genuinely valuable |
| 💬 **Answer a question** | Helping someone in Discord or Discussions is real contribution |

For **security vulnerabilities**, stop here and read the
[security policy](SECURITY.md) instead. Please do not open a public issue.

## Before you start writing code

**Small and obvious?** Typos, broken links, a clear one-line bug fix — just open
a pull request. No ceremony needed.

**Anything larger?** Open an issue first. This is not bureaucracy; it is us
saving you from building something we are about to restructure. Please open an
issue before starting on:

- New features or user-visible behavior changes
- Public or internal API contract changes
- Database schema or migration changes
- Anything that crosses a repository boundary
- Changes to permissions, approval flow, or destructive actions

Say what you plan to do and let the discussion settle before you invest real
time. If an issue already exists and nobody is assigned, comment that you are
picking it up so we do not duplicate work.

## Understanding the architecture

Cordly is a poly-repo, and the boundaries are the product. Read the
[organization profile](https://github.com/cordlybot) for the overview, then keep
these rules in mind:

1. **Work in the repository that owns the behavior.** Do not solve a problem in
   the bot that belongs in the API.
2. **Never couple repositories through source imports or a shared database.**
   Services meet at versioned HTTP, event, and artifact contracts. If you find
   yourself wanting to import across a boundary, that is the signal to open an
   architecture issue.
3. **Only the bot mutates Discord.** The AI service proposes; it never executes.
4. **Only an explicit, immutable approval makes a plan executable.** An approval
   is bound to one exact proposal and cannot be transferred to another.
5. **Every action carries** tenant and guild context, a correlation ID, the
   actor, an approval reference, and an idempotency key.
6. **Treat AI output as untrusted input** until schema and policy validation has
   passed.

If a change requires bending one of these, that is not automatically a "no" —
but it needs an architecture issue and a recorded decision, not a quiet
workaround.

## Development environment

Application services and their tests run in **Docker**. Please do not install or
run the Rust, .NET, Python, or Node runtimes directly on your host — container
parity is what keeps "works on my machine" out of the review conversation.

Each repository documents its own quality gate in its `README.md`. Run that gate
in full before you open a pull request, and paste the exact commands and their
results into the description.

## Commit messages

We use [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>: <short imperative summary>
```

Types in use: `feat`, `fix`, `docs`, `refactor`, `test`, `perf`, `style`,
`build`, `ci`, `chore`.

```
feat: bind approvals to an immutable proposal hash
fix: preserve full capability manifest contract
docs: make Docker the development workflow
```

Keep the summary under about 72 characters and in the imperative mood ("add",
not "added"). Put the reasoning in the body — the *why* is worth more than the
*what*, which the diff already shows. Keep each commit focused on one coherent
change.

## Opening a pull request

Fill in the pull-request template honestly. A reviewer needs to understand:

- **The problem** you are solving, with the issue linked
- **Your approach**, including the non-goals you deliberately left out
- **Risk review** — authorization and tenant scope, secrets and privacy, Discord
  permissions and destructive actions, AI failure modes
- **Contracts and data** — compatibility, migrations, generated clients, or an
  explicit "none"
- **Validation** — the exact commands you ran and what they printed
- **Deployment and rollback** — flags, migration ordering, and how to undo it

A few things that make review go smoothly:

- Keep the pull request scoped to one repository and one coherent change
- Add or update tests for behavior changes, including the important failure paths
- Keep contracts backward compatible, or clearly document the version and
  migration impact
- Update the docs in the same pull request, or link a follow-up issue
- Include screenshots only when a visual surface actually changed
- Mark it as a draft if you want early feedback — that is encouraged

> [!IMPORTANT]
> **Never commit** secrets, Discord tokens, API keys, user content, personal
> data, generated credentials, production incident evidence, or local absolute
> paths. If you commit one by accident, tell a maintainer immediately rather
> than quietly force-pushing — a leaked token needs rotating, not just deleting.

## Review, and what to expect

A green CI run does not replace human judgment, especially for permissions,
destructive actions, AI safety, and tenant isolation. Expect questions on those
even when the tests pass — it is not a comment on your work.

Cordly currently has one maintainer, so review may take a few days. If a week
goes by with no response, a polite nudge on the pull request is entirely
welcome and will not annoy anyone.

Feedback is about the code, never about you. If a review ever reads otherwise,
please say so.

## Licensing

By contributing, you agree that your contributions are licensed under the
[MIT License](LICENSE) that covers the repository you are contributing to, and
that you have the right to submit the work under that license. Do not paste in
code you do not have the rights to relicense.

Participation in all Cordly spaces is covered by our
[Code of Conduct](CODE_OF_CONDUCT.md).
