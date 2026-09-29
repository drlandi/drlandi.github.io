---
title: "uLLAMA: A Minimal C Runtime for Local LLM Inference"
summary: "A small C wrapper around llama.cpp that loads GGUF models and runs an interactive session with live tokens-per-second reporting — built to fit an 8 GB, CPU-only laptop and written under the 42 Norm."
date: 2026-09-29
---

## Why build it

I wanted to own the inference loop instead of treating it as a black box. The constraint
was deliberate: an 8 GB, CPU-only laptop, no GPU. When memory and compute are that tight,
every decision — context size, thread count, sampling strategy, model quantization — shows
up immediately in how the model behaves, which makes it a good way to learn what actually
happens between a prompt and a response.

It is also written in the same C discipline I practice at 42 São Paulo: the 42 Norm, manual
resource management, and no framework between me and the model.

## What it does today

- Loads a local GGUF model through the llama.cpp C API and initializes a 2048-token context
  running on four threads.
- Builds a sampler chain — top-k 40, top-p 0.9, temperature 0.7 — so generation is
  controlled and reproducible rather than left to defaults.
- Runs an interactive session: tokenizes the prompt, decodes one token at a time, streams
  the output as it is produced, and prints tokens per second after each response.
- Frees the sampler, context, and model in order on exit, and silences the backend's
  logging so the terminal shows only the conversation.

The core is roughly a hundred lines of C. That is intentional: small enough to read in one
sitting and reason about completely.

## Where it is going

The repository is scaffolded for the next stages, which are planned rather than finished:

- **Domain-specific fine-tuning** — a dataset folder for JSONL training data (42 C norms,
  Python, AWS workflows) and a notebook slot for training small models with Unsloth and
  exporting them to GGUF.
- **An offline code factory** — a harness that uses the compiled binary to generate and test
  code with no network dependency, so a small local model does the cheap, high-volume work
  and larger models are reserved for the hard problems.

I'll document each stage here as it becomes real, including the numbers, so the write-ups
show what worked and what did not.

---

*Code: [github.com/drlandi/ullama](https://github.com/drlandi/ullama)*
