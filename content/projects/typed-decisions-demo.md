---
title: "Typed Decisions vs. Prose: A Support-Ticket Triage Demo"
summary: "A live, side-by-side demo of two ways an AI model can triage support tickets — fast typed decisions versus slower generated prose — built against a published model contract and localized for Brazilian Portuguese."
date: 2026-09-28
---

## The idea

TypeSafe AI describes its Jev model as a "System One" model: instead of generating prose, it
returns a typed decision, the way a reflex does, while a conventional LLM deliberates in
free text. The framing borrows from Kahneman's fast and slow thinking. I wanted to see what
that difference feels like in a concrete workflow, so I built a support-ticket triage demo
that races the two approaches against each other.

## What I built

- A side-by-side triage race: the same tickets go through a typed-decision path and a
  prose-generation path, so the difference in shape and speed is visible instead of
  described.
- The typed side follows the published Jev input/output contract. The API itself is in
  closed beta, so the demo simulates the contract locally, and the call site is scaffolded
  so the simulation can be replaced by the real request once access opens.
- Two deployed versions: English, and a Brazilian Portuguese version that is localized
  rather than translated — the tickets, labels, and flow are adapted to a Brazilian context.

**Try it:** [English demo](https://singular-kashata-ca802b.netlify.app/) ·
[Versão em português](https://jovial-mooncake-a2f140.netlify.app/)

## What it does and doesn't show

Because the typed model is simulated, the demo shows the interaction pattern and the
contract, not real latency or accuracy numbers. That is the honest boundary of the work.
The pattern is still the interesting part: a decision returned as a typed value can be
validated, routed, and tested like any other data, while free text has to be parsed and
trusted first.

When API access opens, I'll swap in the real call, measure latency and agreement against
the prose path, and publish the results here.

## Why it matters to me

Most of my career was spent on systems where an unpredictable component was a liability.
Constraining what an AI model is allowed to return is the same instinct applied to a new
kind of component: make the interface strict enough that the rest of the system can rely
on it.
