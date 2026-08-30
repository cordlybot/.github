# Getting help

Cordly is pre-release and under active development, so there is no support SLA
yet — but there are people who will help. Here is where to go.

## Where to ask

| Your situation | Go here |
| :--- | :--- |
| A question, setup help, or "am I thinking about this right?" | [Cordly Discord](https://cordly.gg/discord) |
| A reproducible bug | A **bug report** issue on the repository that owns the behavior |
| An idea or a missing capability | A **feature proposal** issue |
| Something crossing repository boundaries or affecting a contract | An **architecture decision** issue |
| A security vulnerability | **[Report it privately](SECURITY.md)** — never in a public issue |
| Something about the product, not the code | [cordly.gg](https://cordly.gg) |

The Discord is the fastest route for anything conversational. Nobody there will
mind a beginner question, and "I could not figure out how to start" is useful
feedback, not a failure on your part.

## Writing a good bug report

The issue forms ask for these, and they are what turn a report into a fix:

- **Repository and version** — the repository name plus a commit, tag, or image
  digest
- **Expected behavior** — what you thought would happen
- **Actual behavior** — what happened instead
- **Minimal reproduction** — the shortest set of steps that triggers it, with
  sanitized configuration
- **Environment** — OS, runtime and toolchain versions, Docker or Kubernetes
  setup, relevant dependencies
- **Sanitized evidence** — correlation IDs, error messages, and logs

If you cannot produce a minimal reproduction, open the issue anyway and say so.
A vague report is far better than a silent one.

> [!WARNING]
> **Never post** tokens, credentials, API keys, personal data, private Discord
> messages, member lists, or production logs containing sensitive values —
> not in an issue, not in Discord, not in a screenshot. Redact before you send.
> If you realize you posted a secret, delete it and rotate it immediately, then
> tell a maintainer.

## Response expectations

Cordly currently has one maintainer, so please be patient — and do not read
silence as disinterest. If a week passes with no response on an issue, a polite
bump is welcome.

Security reports are the exception and are prioritized above everything else;
see the [security policy](SECURITY.md) for the response targets we hold
ourselves to.
