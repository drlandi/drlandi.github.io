# drlandi.dev

Personal website and technical portfolio of Daniel Landi — Cloud & Infrastructure Engineer.

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

- `content/projects/` — Featured technical projects (AWS Digital Resilience architecture,
  Gems & Landi AWS Cloud Labs documentation series, 42 systems programming work)
- `content/blog/` — Technical writing on infrastructure, cloud architecture, and the
  ops-to-cloud-engineering transition
- `content/about.md` — Background and current focus
- `layouts/` — Hand-written Hugo templates (no external theme dependency)
- `static/CNAME` — Custom domain configuration for GitHub Pages

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
