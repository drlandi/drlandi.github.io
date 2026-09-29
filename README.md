# drlandi.dev

Personal website and technical portfolio of Daniel Landi — AI & Infrastructure Engineer.

## About

This repository hosts the source for [drlandi.dev](https://drlandi.dev), built with Hugo
and deployed via GitHub Pages.

**Background:** 25+ years architecting and operating mission-critical distributed systems
and high-availability wireless networks across seven countries (Ericsson, Nextel, and
carrier-grade telecom infrastructure). Currently retooling into cloud-native and modern
software engineering practice through:

- **42 São Paulo** — intensive, project-based systems programming curriculum (C, algorithms,
  Unix/Linux internals)
- **Hands-on AWS architecture** — WAF, CloudFront failover, API Gateway, Lambda, DynamoDB,
  SQS, CloudWatch observability
- **AI-assisted engineering** — production AI application patterns, local LLM deployment,
  and pedagogical AI tooling

## Site Structure

- `content/projects/` — Featured technical projects (self-correcting activity generator, uLLAMA
  local-LLM runtime, typed-decision triage demo, AWS Digital Resilience architecture, Gems & Landi AWS Cloud Labs
  documentation series, 42 systems programming work)
- `content/blog/` — Technical writing on infrastructure, cloud architecture, and the
  ops-to-cloud-engineering transition
- `content/about.md` — Background and current focus
- `i18n/` — UI strings per language (`en.toml`, `pt-br.toml`)
- `layouts/` — Hand-written Hugo templates (no external theme dependency)
- `static/resume.pdf` / `static/curriculo.pdf` — English and Portuguese résumés linked from About
- `static/CNAME` — Custom domain configuration for GitHub Pages

## Languages

The site is bilingual. English is the default and lives at `/`; Brazilian Portuguese lives
at `/pt-br/`. Translations are paired by filename: `about.md` (English) and `about.pt-br.md`
(Portuguese). A page with no translation simply doesn't appear in the other language, and the
header language switcher falls back to that language's home page. To add a translated page,
create the `.pt-br.md` twin next to the English file with the same base name.

## Stack

- **Static site generator:** [Hugo](https://gohugo.io/) (extended, v0.123+)
- **Hosting:** GitHub Pages
- **Domain:** drlandi.dev (DNS via Porkbun)

## Local Development

```bash
hugo server -D
```

Then visit `http://localhost:1313/`.

## Deployment

Pushed to the `main` branch and built via GitHub Actions (see `.github/workflows/hugo.yml`),
which publishes the generated `public/` directory to GitHub Pages automatically.

---

**Contact:** drlandi@gmail.com | [GitHub](https://github.com/drlandi)
