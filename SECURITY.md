# Security policy

Thank you for helping keep Cordly and the communities that use it safe. We take
every report seriously and we will not take legal action against research
conducted in good faith within the boundaries described below.

## Supported versions

Cordly is pre-release. Only the current `main` branch of each repository and the
latest deployment receive security fixes. There are no maintained release
branches yet, and no backports to older commits.

## Reporting a vulnerability

**Please report privately. Do not open a public issue.**

Use GitHub's private vulnerability reporting:

> **[→ Report a vulnerability](https://github.com/cordlybot/.github/security/advisories/new)**

If the affected repository has private reporting enabled, prefer that
repository's own advisory form so the report lands with the right context.
Otherwise use the link above and name the affected repository in the report —
it reaches the same maintainer either way.

If you cannot use GitHub at all, send a maintainer a direct message in our
[Discord](https://cordly.gg/discord) asking for a private channel to disclose
in. Do **not** post vulnerability details in a public Discord channel.

### What to include

- The affected repository, plus the commit, tag, or image digest
- Impact — what an attacker gains, and who is affected
- Prerequisites — required permissions, roles, or preconditions
- A minimal, safe reproduction
- Any suggested mitigation, if you have one

### What not to include

Never send live credentials, Discord tokens, real user data, personal
information, exploit traffic captured against systems you do not own, or
private Discord message content. Redact before you send; a sanitized report is
just as actionable.

### What happens next

| Stage | Target |
| :--- | :--- |
| Acknowledgement that we received the report | Within 3 business days |
| Triage decision, severity assessment, and coordination plan | Within 7 business days |
| Fix, or a written plan with dates if it will take longer | Case by case, communicated |

We will keep you updated as the fix progresses and will credit you in the
advisory when it publishes, unless you would rather stay anonymous. Just tell
us which you prefer.

## Research safety

Research is welcome. Please stay inside these boundaries:

- **Use your own test environment.** Run Cordly locally or against a Discord
  server you personally own.
- **Do not access other users' data.** If you stumble into someone else's data,
  stop, do not save it, and tell us what you saw in the report.
- **Do not disrupt Discord communities or infrastructure.** No denial of
  service, no volumetric or stress testing, no spam.
- **Do not use social engineering** against maintainers, contributors, users, or
  Discord staff.
- **Do not persist.** Once you have demonstrated impact, stop — no backdoors, no
  lateral movement, no data exfiltration.
- **Do not test production** without written authorization from a maintainer.

Please also respect [Discord's Terms of Service](https://discord.com/terms) and
[Developer Policy](https://discord.com/developers/docs/policies-and-agreements/developer-policy);
a finding is not worth getting an account or a server banned over.

## Areas we care most about

Cordly's core promise is that AI can propose anything but change nothing without
approval. Findings that undermine that promise are the highest severity we have:

- **Approval binding** — making an action executable without a valid, explicit
  approval, or swapping the plan after an approval was granted
- **Prompt injection crossing the AI-to-action boundary** — untrusted content in
  a Discord message influencing what gets executed
- **Tenant and guild authorization** — reaching data or actions belonging to a
  guild the actor does not manage
- **Discord OAuth and token handling** — leakage, confusion, or scope escalation
- **Service-to-service authentication** — impersonating the API, bot, or AI service
- **Bot action idempotency** — replay or duplicate delivery causing repeated or
  destructive effects
- **Permission escalation** — via role hierarchy, permission overwrites, or
  protected-object bypass
- **Knowledge provenance** — poisoning templates, references, or evaluation data
- **Secrets in logs or build artifacts**

Reports outside these areas are still welcome — this list is about priority, not
eligibility.

## Out of scope

Missing hardening headers with no demonstrated impact, automated scanner output
without a working proof of concept, best-practice opinions unattached to a
concrete attack, denial of service, social engineering, and vulnerabilities in
Discord itself (report those to
[Discord's security bug bounty program](https://discord.com/security)).
