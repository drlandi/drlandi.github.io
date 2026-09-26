---
title: "Gems & Landi: AWS Cloud Labs Documentation Series"
summary: "An ongoing, self-published documentation standard for hands-on AWS labs — networking, security, and serverless — written with more depth than the official docs."
date: 2026-08-23
---

## What this is

An in-depth documentation series covering hands-on AWS cloud labs, maintained in
Markdown/Obsidian and published in "The-Forge" repository. It exists because most
AWS tutorials show you *that* something works, not *why* it's configured that way —
and the gap between those two things is where production incidents come from.

## Scope

The series currently covers labs across:

- **Networking** — VPC design decisions, subnet strategy, routing edge cases
- **Security** — IAM policy structure, WAF rules, the reasoning behind least-privilege
  configurations rather than just the steps to enable them
- **Serverless** — Lambda, API Gateway, and DynamoDB patterns, including the failure
  modes that don't show up until you've pushed real traffic through them

## Why the depth matters

Coming from a background where SOPs and technical documentation directly determined
whether a NOC engineer at 3 AM could resolve an incident without escalating, I write
these labs the same way: assume the reader needs to *understand* the decision, not just
copy the command. That's the standard I'm holding this series to — and it's also, frankly,
the standard I'd want from anyone documenting infrastructure I depend on.

---

*Full series: [The-Forge repository](https://github.com/drlandi)*
