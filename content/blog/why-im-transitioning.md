---
title: "What 25 Years of Telecom Ops Taught Me About Cloud Infrastructure"
summary: "The instincts don't change when the hardware does — they just get renamed."
date: 2026-09-25
---

For 25 years I kept things running that were not allowed to go down. GSM and WCDMA
rollouts across six countries, NOC operations on carrier-grade Unix systems, prepaid
databases where a two-minute outage meant a support queue nobody wanted to own. The
tooling was Solaris, Nortel, Motorola, Tekelec. The discipline was ITIL and eTOM. None
of that tooling is what I work with today.

The discipline is exactly the same.

## The pattern repeats

When I sat down to design a failover-resilient architecture for a recent hackathon —
CloudFront in front of dual-origin S3 buckets, one in `sa-east-1`, one in `us-east-1` —
I wasn't thinking about AWS best-practice checklists. I was thinking about the same
question I asked for two decades: *what happens when this region goes dark, and does
anyone notice before the metrics do?*

The answer, then and now, is the same: decouple the parts that can fail independently,
make the failover automatic instead of procedural, and instrument the seams so a human
finds out from a dashboard instead of a phone call.

## What's actually new

The syntax is new. Lambda instead of a NOC runbook. DynamoDB instead of a prepaid
database on HP-UX. CloudWatch instead of a NOC engineer's mental model of "this metric
usually looks like that." Terraform instead of a change-management ticket. I'm learning
all of it deliberately, through 42 São Paulo's systems programming curriculum and through
building real AWS architectures rather than following tutorials.

But I'd be doing the field a disservice if I framed this as starting from zero. The
instinct for where systems actually break — not where the documentation says they might,
but where they actually do, at 3 AM, under real load — doesn't transfer through a
certification. It's the thing 25 years bought me, and it's the thing I'm not willing
to discount just because the infrastructure is now defined in YAML instead of racked
in a data center.

## Where this goes

I'm documenting the build-out as I go — architecture write-ups, the AWS labs series,
the systems programming work at 42 — because the value isn't just in having built these
things. It's in being able to explain *why* each decision was made, which is the part
most cloud tutorials skip and most production incidents expose.
