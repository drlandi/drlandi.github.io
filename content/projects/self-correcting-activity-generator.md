---
title: "Self-Correcting AI Teaching Activity Generator"
summary: "A web app built in 48 hours for the IBM Bob 2.0 Hackathon that generates hands-on, teacher-editable classroom activities for any resource level — with a second AI pass that audits its own output against six quality checks before the teacher sees it."
date: 2026-09-27
weight: 1
---

## The problem

Teachers are asked to make lessons hands-on and locally relevant, but generic lesson plans
assume a resource level the classroom may not have: a lab with GPUs in one school, a room
with a chalkboard and no printed materials in another. I wanted a tool whose output adapts
to the classroom instead of the other way around — and whose output a teacher can trust
enough to use, because a lesson plan that looks right but contains a wrong fact or a broken
timeline is worse than none.

## What it does

A teacher enters a subject and a topic, plus optional cross-curricular links and context
(available materials, class setting). The app returns an activity with a fixed pedagogical
structure — narrative hook, flowchart, timeline, materials, step-by-step instructions,
reflection questions and an assessment rubric — plus customization zones marked for a photo,
a teacher note or a student worksheet, so the teacher can localize it.

Three design decisions shape the output:

- **Resource-aware materials.** For math, science and engineering topics the prompt prefers
  hands-on, tactile approaches and calibrates the materials to the stated context — kits where
  a school has them, straws, tape and cut paper where it doesn't — and requires the Materials
  section to justify the choice.
- **Premise check.** If a topic rests on a false or pseudoscientific premise, the app reframes
  the activity as a critical-thinking exercise instead of teaching the claim as true. Difficult
  but legitimate topics, such as documented atrocities or discrimination, are treated as
  normal subjects.
- **Printable flowchart.** The Mermaid flowchart is split onto its own page, so it can be
  printed or projected on its own.

## How it works

1. An Express server receives the form and builds a structured prompt.
2. Node spawns the IBM Bob CLI as a child process (`bob run` with JSON output and a five-turn
   limit). The response is newline-delimited JSON, parsed line by line to find the result
   message.
3. The draft goes through a second Bob call: the **self-critique pass**, described below.
4. The app extracts the final activity from between two marker lines, pulls the flowchart out
   for its own page, stores the result under a random UUID and redirects to it.

Failure handling was part of the design, not an afterthought. Generation and critique have
their own timeouts (60 and 90 seconds). If the critique step fails for any reason — timeout,
missing markers, malformed output — the app falls back to the original activity instead of
showing nothing. A response with no result message is treated as a content refusal and gets a
dedicated page instead of a stack trace.

## The self-critique pass

The second pass is not "make it better." It is a strict reviewer with six specific checks,
taken from a QA audit of earlier generated activities:

1. **Data sourcing** — contested figures stated with false precision get a qualifier or a
   "TEACHER: verify" flag.
2. **False binaries** — a rubric or role structure that splits a real-world question into two
   opposing camps is flagged where it is enforced, not in a general note elsewhere.
3. **Scope versus timeline** — five substantial phases in a single 50-minute period get
   trimmed, or split across days.
4. **Rubrics without exemplars** — every level needs an observable indicator.
5. **No opt-out for public performance** — the alternative must be built into the actual task
   assignment, not buried in an appendix.
6. **Internal consistency** — the model first extracts every claim from every section, in
   writing, and only then compares them, so contradictions are caught by explicit comparison
   rather than by a general impression.

Two rules cut across all six. First, **never fabricate a citation**: a made-up reference that
looks authoritative is a worse defect than an unsourced number, because a teacher will trust
and repeat it. Where a fix would call for a source, the reviewer adds a qualifier or a verify
flag instead. Second, **fixes must be complete**: if a change touches a timed phase, every
other place that mentions it, and the total time, must still add up.

The repository keeps the successive versions of the critique prompt and the test drafts I used
to tune it.

## Integration and deployment

Bob has no public HTTP inference endpoint, so the app shells out to the CLI. My first version
used `execFile` and hung, waiting on output pipes that never closed; switching to `spawn`
with stdin ignored and the output streams collected fixed it.

The CLI is installed through IBM's installer script rather than the npm registry, so a
standard `npm install` on a cloud host cannot provide it. The Dockerfile starts from
`node:22-slim`, runs the installer non-interactively, and then runs `bob --version` as a build
step so that a broken install fails the build instead of failing at runtime. The container was
deployed on Railway for the hackathon, with the API key supplied as an environment variable.

## Limits

- Activities live in memory, keyed by UUID, which was enough for a 48-hour demo. They vanish
  on restart; a real deployment needs a persistent store.
- The critique is another model call. It reduces specific classes of defects but does not
  prove correctness, which is why it leaves "TEACHER: verify" flags instead of guessing. The
  teacher remains the final reviewer.
- A standalone script, `validate-activity.js`, checks that generated activities contain the
  required sections. I used it during QA, but it is not wired into the web interface.
- The interface is currently English only.

---

*[Code](https://github.com/drlandi/ibm-bob-activity-generator)*

The public demo ran on Railway during and after the hackathon. It depended on the hackathon's API key, and it is no longer running.
