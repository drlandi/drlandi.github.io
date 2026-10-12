---
title: "The C code factory: local first, cloud only when needed"
summary: "A 7B model on a laptop with no GPU solved 20 of 41 C tasks under the 42 Norm; a cloud cascade solved all 41 for USD 1.01. Method, results, and the six mistakes I caught along the way."
date: 2026-10-11
---

![The cascade: 20 tasks solved by the local 7B, 13 by Haiku, 8 by Sonnet](/images/factory-en.png)

*Technical report by Daniel Landi (42 São Paulo, login dlandi), 2026-10-11.*

## Summary

I built a pipeline that writes small C functions that follow the 42 Norm, using a small model on a laptop first and a paid cloud model only when the local one fails. Every answer is judged automatically, with no human edits.

On a ladder of 41 tasks (easy to hard):

| System | Easy + medium (22 tasks) | All 41 tasks | Cloud cost |
|---|---|---|---|
| Qwen2.5-Coder 3B, local | 12 (55%) | 13 of 38 scored | 0 |
| Qwen2.5-Coder 7B, local | 15 (68%) | 20 of 41 (49%) | 0 |
| 7B, then Claude Haiku 5.5, then Claude Sonnet 5.5 | not split out | **41 of 41** | **USD 1.01** |

- The cascade met its target (95% or more with the cloud) and the local tier did not meet its own (80% on easy and medium tasks; the 7B reached 68%, with 3 attempts instead of the 5 the criterion allows).
- 21 of the 41 tasks needed the cloud. Haiku solved 13 of them for about 0.7 cent each. Sonnet solved the last 8 and took 91% of the money.
- Five tuning experiments on the local model gave no clear gain. One of them looked like a success until a control run showed otherwise (section 7).
- I retracted six claims along the way. They are listed in section 8, because I think they are the most useful part of the report.

## 1. The problem and the setup

At 42, all C code follows the Norm: functions of at most 25 lines, at most 5 functions per file, at most 4 parameters, no `for`, no `switch`, no ternary operator, and more. A tool called norminette checks it, and a single error fails a project. Code also has to be correct, which in practice means tests plus sanitizers (memory bugs are the usual way a "working" C function turns out to be wrong).

The question: can a small model on my own laptop write code that passes all of that, and if not, how little cloud do I need to cover the rest?

Hardware: Intel i3-N305, 7.4 GiB of RAM, no GPU, Pop!_OS. Local models run on llama.cpp (`llama-server`), 4-bit quantized: Qwen2.5-Coder 3B (Q4_K_M) and Qwen2.5-Coder 7B (Q4_K_M, 4.7 GB). Cloud models are called through Anthropic's Messages API.

## 2. How "verified" is defined

A task is **verified** when one attempt satisfies all of these, with no human edits:

1. The code compiles with warnings as errors.
2. It passes every case of the task's judge, with AddressSanitizer and UndefinedBehaviorSanitizer on.
3. After the code is run through `c_formatter_42`, norminette reports 0 errors.

Each task has a spec (a `.h` file), a judge (a test program), a Norm-clean reference solution, and 3 to 5 deliberately broken versions that the judge must catch. The judges for the tasks that allocate memory also make `malloc`, `calloc` and `realloc` fail on purpose (linked with `--wrap`), because many solutions that looked right crashed when an allocation failed.

An attempt is up to 3 **rounds**: the model writes code, the harness runs the judge and norminette, and the model gets the errors back (compiler output, failing cases, sanitizer reports, Norm errors) and tries again. The best round of an attempt is the one that counts. Local results use the first 3 attempts per task. Everything is logged to a `run.json` plus the code of each round, so every number in this report can be recomputed.

## 3. The ladder: 41 tasks

| Level | Tasks | Examples |
|---|---|---|
| Easy (10) | `abs_diff`, `add`, `clamp`, `count_digits`, `count_words`, `gcd`, `is_palindrome`, `is_power_of_two`, `min_max`, `sum_array` | one loop or one expression |
| Medium (12) | `array_reverse`, `str_count_substr`, `array_dup`, `parse_port`, `str_interleave`, `uint_to_binary`, `str_repeat`, `str_squeeze`, `array_rotate`, `hex_encode`, and two more | allocation, arrays, string building |
| Hard (16) | `slist`, `list_reverse`, `heap`, `list_remove_if`, `queue`, `vec`, `mat_mul`, `map`, `str_replace_all`, `str_wrap`, `base64_encode`, `base64_decode`, `format_duration`, `parse_duration`, `csv_line_split`, `calc` | data structures and parsers |
| Other (3) | `trim`, `rle`, `stack` | the first tasks I wrote |

All tasks were written by me. Some hard tasks were simplified to fit the Norm (for example, `map` has a fixed number of buckets and `heap` a fixed capacity).

## 4. Results of the local models

| Level | Tasks | 3B | 7B |
|---|---|---|---|
| Easy | 10 | 8 | **9** |
| Medium | 12 | 4 | **6** |
| Hard | 16 | 1 | **4** (`slist`, `list_reverse`, `heap`, `vec`) |
| Other | 3 | not in this merge | 1 (`stack`) |
| Easy + medium | 22 | 12 (55%) | **15 (68%)** |
| All | 41 | 13 of 38 scored | **20 of 41 (49%)** |

Verified attempts: 55 of 123 for the 7B against 29 of 114 for the 3B. The 7B solved 6 tasks the 3B did not, and the 3B solved none that the 7B missed. All 8 hard parsing tasks are unsolved by both.

What the 7B's attempts end in (best round of each of the 123 attempts): 62 pass, 35 sanitizer error, 14 wrong output, 12 did not compile. So the main loss is **memory and undefined-behavior bugs**, not the Norm. Of 104 sanitizer reports, 44 were heap-buffer-overflows and 25 were signed integer overflows.

The 7B also repeats itself: in 48% of repair rounds (56 of 116) it sent back the same code as before, the same rate as the 3B. The feedback often changes nothing.

## 5. The cloud cascade

The 7B failed 21 tasks. A script sent exactly those to the cloud in two stages, cheap model first.

| Stage | Settings | Tasks | Solved | Cost |
|---|---|---|---|---|
| Haiku 5.5 | thinking off, 3 attempts, 3 rounds, cap USD 0.40 | 21 | 13 | USD 0.0866 |
| Sonnet 5.5 | low effort, 2 attempts, 3 rounds, cap USD 1.00 | 8 (the ones Haiku missed) | 8 | USD 0.92 |
| Total | | 21 | **21** | **USD 1.0066** |

- Haiku solved 13 tasks at about 0.7 cent each (`array_rotate`, `array_unique_sorted`, `base64_encode`, `count_digits`, `list_remove_if`, `mat_mul`, `parse_port`, `queue`, `str_interleave`, `str_repeat`, `str_replace_all`, `str_squeeze`, `trim`).
- Sonnet solved `base64_decode`, `calc`, `csv_line_split`, `format_duration`, `map`, `parse_duration`, `rle` and `str_wrap`, at about 11.5 cents per task solved.
- The money follows difficulty: the hardest 8 tasks cost 91% of the cloud spend.
- The harness's own cost count matched the Anthropic Console to the cent. Development and exploration runs before the final cascade cost roughly another USD 1.8.

Some behavior of the API I had to learn from error messages: Haiku 5.5 and Sonnet 5.5 reject the `temperature` parameter, so the cloud runs are not like-for-like with the 0.2 used locally; thinking tokens count toward `max_tokens`, so a low limit can produce an empty reply that looks like a model failure but is a limit; and Sonnet 5.5 refuses `thinking: disabled` and wants `between_tools` instead.

## 6. Where the local model loses, and where it does not

- **The Norm is not the main loss.** I tested this directly: of the 7B's 123 attempts, only 2 passed the judge and had only mechanical Norm errors a script could fix. 7 passed the judge and failed the Norm, 5 of those on `TOO_MANY_LINES`.
- **The 7B does not have Haiku's habit.** Haiku writes correct logic and then splits it into too many functions (`TOO_MANY_FUNCS`). That error never occurs in the 7B's code.
- **The 3B can't restructure code to satisfy the Norm,** even with specific hints: when told to split a long function it kept the old one and added a second copy, so the file defined it twice.
- **Specific feedback matters, and only for what the model can see.** When the feedback quoted the offending line and a rewrite, the 3B fixed Norm errors within 1 to 2 rounds. A crash report alone mostly got the same code back.

## 7. Tuning experiments and the control run

All on the 7B with the same settings as the ladder run; one flag changed at a time, and the success bar written before the run.

| Experiment | Tasks | Verified attempts, reference then flag | Result |
|---|---|---|---|
| `--func-budget` (name the function limits with real numbers) | 12 | 9 of 36, then 9 of 36 | no effect; the premise was wrong |
| `--norm-hints2` (quote the line, give a `for` to `while` rewrite) | 12 | 9 of 36, then 5 of 36 | slightly worse, within run-to-run noise |
| `--sanitizer-hints2` (debug info, quote the failing line, per-kind hint) | 15 | 0 of 45, then 3 of 45 | looked like a success; see below |

For the sanitizer experiment I used the 15 tasks whose attempts had ended in sanitizer errors, and set three bars in advance (at least 3 verified attempts, or 2 newly solved tasks, or sanitizer endings down to 25 or fewer). The first run met all three. I wrote that down as a success, then ran a **control**: the same settings, no flag, same tasks.

| 15 tasks, 3 attempts each | reference | control (rerun, no flag) | with the flag |
|---|---|---|---|
| Verified attempts | 0 | 0 | 3 |
| Ended in a sanitizer error | 35 | 28 | 25 |
| Judge pass | 1 | 3 | 8 |
| Repair rounds that repeated the same code | not computed | 51 of 90 | 36 of 90 |

The control alone moved sanitizer endings from 35 to 28 and judge passes from 1 to 3. I had picked these tasks because they had failed, and a rerun of failures looks better by chance: regression to the mean. Against the control the flag still looks better, but with attempts treated as independent (which flatters the result) the differences are not significant: Fisher exact p is about 0.24 for verified, 0.20 for judge passes, 0.67 for sanitizer endings and 0.04 for repeated replies (one of five comparisons under 0.05). My conclusion: a small positive trend, not a demonstrated effect. The flag only changes feedback after a sanitizer failure, so it is cheap to keep on. Proving an effect would take roughly three times the attempts.

## 8. Mistakes I caught and retracted

1. **"Verified" `stack` code was correct.** An early version of the judge never made `malloc` or `realloc` fail. After I added allocation-failure cases, none of the earlier "verified" `stack` attempts survived (the usual cause was `s->data = realloc(s->data, n)` and a leak when a later allocation failed). A passing judge is a lower bound.
2. **"The Norm is the main blocker; a script could fix 40% of the failures."** Wrong. The measurement showed 2 of 123 attempts. I retracted it.
3. **Difficulty labels.** I first labeled tasks by hand and reported 21% on medium tasks. The ladder's own batches give 50%. Always use the definition that was fixed before the run.
4. **"The 7B has Haiku's too-many-functions problem."** An assumption I built an experiment on. The error never occurs in the 7B's code.
5. **"All three bars were met" for the sanitizer flag.** Mostly regression to the mean, as the control showed.
6. **"Thinking made Sonnet expensive."** The API's own message showed thinking was already off. What lowered the cost was `--effort low`.

Also not proven: the cause of a freeze that killed the first 7B run. The model server had started with 4 slots and a prompt cache of up to 8 GiB on a machine with 7.4 GiB of RAM. Fixing it with `LLAMA_ARG_CACHE_RAM=0 LLAMA_ARG_N_PARALLEL=1` and closing the browser removed the freeze, but I changed three things at once, so the cache is the leading hypothesis, not a proven cause.

## 9. Limits of these results

- The tasks are mine and small. Nothing here measures code outside the ladder.
- The judges are a lower bound: a passing judge does not prove the code is correct.
- With 3 attempts per task, one task of difference is within noise.
- The cloud rate (21 of 21) is a rate on the hard cases only, because those 21 were chosen as the 7B's failures. Sonnet solved `calc` and `rle` on one attempt of two, and its `rle` varied between runs earlier (3 of 5, 2 of 5, 0 of 2). Read "all 41" as "all 41 reached once", not as reliability.
- The cascade was run by a script in two stages; the harness's `--escalate` mode still has one cloud model.
- Cloud models run at their own temperature, so they are not a like-for-like comparison with the local runs.
- No 42 exam subjects or graded-project code were sent to any API. Only tasks I wrote.

## 10. What's next

In order of expected value per hour:

1. Give the 7B attempts 4 and 5 on its unsolved easy and medium tasks (about 3 hours on the laptop). This measures the 80% target as defined.
2. Search for a better local model of 8B or less that fits in about 5 GB, and run the same ladder.
3. Show the judge's test cases in the prompt, and change the first prompt. All experiments so far changed only the repair message.
4. More samples: repair temperature, best-of-N.
5. A cheaper hybrid: Haiku writes a skeleton, the local model writes the bodies.
6. Fine-tuning on verified solutions (needs a GPU, hundreds of tasks, and a check of the providers' terms).
7. Larger local models on a planned 64 GB desktop.

For every new experiment: run a control on the same tasks, write the success bar before the run.

## 11. Reproducing this

The code, the judges, the ladder and the logs are in the `c42-factory` repository (`harness.py`, `compare.py`, `exp.sh`, `cloud21.sh`, `ceiling.py`, `normkinds.py`, and ROADMAP_v9.md with every table above and the full history). The cost tracking and the spending caps are part of the harness, and the API key is read only from an environment variable.
