---
title: "Systems Programming at 42 São Paulo"
summary: "Reimplementing the C standard library, building a line-by-line file reader with static memory, and co-developing a stack-sorting algorithm — the foundations most cloud engineers skip."
date: 2026-08-21
---

## Why this matters for infrastructure work

Most people move into cloud engineering from application development or bootcamps.
I came in from the opposite direction — 25 years of operating the infrastructure other
people's code runs on — and 42's curriculum forces exactly the low-level understanding
that background didn't require until now: manual memory management, no garbage collector
to hide behind, no libraries to hand-wave over.

## Selected work

**libft** — reimplemented 43 core C standard-library functions from scratch, then applied
them building a command-line product-catalog project with full documentation. This is the
foundation everything else in the curriculum builds on.

**get_next_line** — a line-by-line file reader using static memory across function calls.
The interesting part wasn't reading the file — it was resolving malloc-failure edge cases
and two memory leaks that only appeared under specific buffer-boundary conditions.

**push_swap** — co-developed with a partner ([Luciano](https://github.com/lfurtado)):
a stack-sorting algorithm project with adaptive sorting strategies tuned for large data
sets, where the naive approach runs out of budget fast.

**ft_printf** — a custom `printf` implementation from scratch, including full variadic
argument (`va_list`) handling.

## The connection to cloud work

None of this is abstract for me. Debugging a memory leak in `get_next_line` and debugging
why a Lambda function's cold-start memory allocation is behaving unexpectedly are the same
kind of problem at different altitudes. That's the argument I'd make for why systems-level
grounding matters even in a serverless world: the abstractions leak, and when they do, you
want to have built the thing they're abstracting.

---

*Code: [GitHub](https://github.com/drlandi)*
