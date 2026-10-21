# drlandi.dev: STATE

**The 2-minute catch-up.** Read this first. Paste it at the start of any new chat about the site.

- Last updated: 2026-10-21, matching git commit `acc150a` ("Blog: add 5-attempt result", 2026-10-11)
- Rebuilt from: the build session of 2026-09-25/26, plus `git log`, `git show --stat` and `hugo.toml` as of 2026-10-21. What was *said* in later sessions (the reasons) is only recorded where the files make it obvious. If something here is wrong, fix it and log the fix.
- This repository is **public**. Everything in `docs/` can be read by anyone. Passwords, keys and private notes never go here; they live in Proton Pass and Obsidian.

## One-paragraph summary

drlandi.dev is Daniel Landi's bilingual (English and Brazilian Portuguese) personal site and technical portfolio: a minimal, text-first Hugo site (Karpathy/Torvalds style) with hand-written templates and no external theme, positioned as "AI & Infrastructure Engineer". The source lives in the GitHub repo `drlandi/drlandi.github.io`. Every push to `main` triggers a GitHub Actions workflow that builds the site with Hugo and publishes it to GitHub Pages, served at https://drlandi.dev with HTTPS enforced. English is at the root, Portuguese is under `/pt-br/`. The domain is registered at Porkbun and its DNS points at GitHub Pages.

## Where everything lives

| Thing | Where |
|---|---|
| Live site | https://drlandi.dev (English), https://drlandi.dev/pt-br/ (Português) |
| Source repo (public) | https://github.com/drlandi/drlandi.github.io |
| Local working copy | `~/dev/site` (Pop!_OS machine; the folder is named `site` on purpose, the repo name is dictated by GitHub) |
| Deploy workflow | `.github/workflows/hugo.yml` ("Deploy Hugo site to GitHub Pages"), unchanged since 2026-09-25 |
| Domain registrar | Porkbun, `drlandi.dev` (registered 2026-09-25, expires 2027-09-25, auto-renew ON, domain lock ON) |
| DNS | Porkbun nameservers; four `A` records on the root domain (see RUNBOOK) |
| Hosting | GitHub Pages, source = **GitHub Actions** (not "Deploy from a branch") |
| Credentials | Proton Pass (never in the repo) |
| Private thinking / map note | Obsidian, The-Forge vault, `07 Landi Projects` |
| Post ideas and pipeline | AppFlowy, LinkedIn Content Vault board |

## Stack

- **Generator:** Hugo, extended edition, multilingual mode. Local machine: 0.123.7 (apt). CI: `hugo-version: latest`. The first live build used 0.166.0. This mismatch is a known risk, see BACKLOG and D8.
- **Languages:** `en` (default, at the site root) and `pt-br` (under `/pt-br/`). Set in `hugo.toml` with `defaultContentLanguage = "en"` and `defaultContentLanguageInSubdir = false`. Each language has its own `tagline` and `bio` there.
- **Translations of pages:** same file name with a `.pt-br.md` suffix next to the English file (for example `about.md` and `about.pt-br.md`).
- **Interface strings:** `i18n/en.toml` and `i18n/pt-br.toml`.
- **Templates (hand-written, no theme):** `layouts/_default/` (`baseof.html`, `list.html`, `single.html`), `layouts/index.html`, `layouts/partials/` (`header.html`, `footer.html`, `lang-switcher.html`, `email.html`), `layouts/shortcodes/email.html`.
- **Styling:** one file, `static/css/style.css` (Georgia serif, narrow single column, accent `#a33`, responsive).
- **Static files:** `static/CNAME` (`drlandi.dev`), `static/resume.pdf` (English), `static/curriculo.pdf` (Português), `static/images/` (blog images such as `factory-en.png`, `fabrica-pt.png`).
- **Site settings in `hugo.toml` `[params]`:** name, email, GitHub URL, LinkedIn URL, WhatsApp number (digits only).

## Content map

Every page below exists in two files: English, and a `.pt-br.md` twin.

```
content/
├── about(.pt-br).md
├── projects/
│   ├── _index(.pt-br).md
│   ├── digital-resilience         AWS dual-origin failover (EDN Hackathon, Case B)
│   ├── gems-and-landi             AWS Cloud Labs documentation series
│   ├── 42-systems-programming     libft, get_next_line, push_swap, ft_printf at 42 São Paulo
│   ├── typed-decisions-demo       typed-decision AI demo (added 2026-09-29)
│   ├── ullama                     uLLAMA, C inference runtime / local-LLM tooling (added 2026-09-29)
│   └── self-correcting-activity-generator   "Bob activity generator" (added 2026-09-29)
└── blog/
    ├── _index(.pt-br).md
    ├── why-im-transitioning       "What 25 Years of Telecom Ops Taught Me About Cloud Infrastructure"
    └── c42-factory                the C code factory post, with the 5-attempt result (added 2026-10-11)
```

## Health check (30 seconds)

```bash
curl -s https://drlandi.dev | grep -i "generator\|<title>"
```

Expected: `<meta name=generator content="Hugo …">` and a title containing "Daniel Landi". If you see **Jekyll**, see RUNBOOK, "Troubleshooting". Also open `https://drlandi.dev/pt-br/` and check that the language switcher works both ways.

## Known open items

Everything unfinished is in `BACKLOG.md`. The ones that matter most:

1. CI uses Hugo `latest` while local uses 0.123.7 (not pinned). Pages were also tested in Claude's sandbox with 0.152.2.
2. The résumé PDFs on the site may lack the AWS Cloud Practitioner line; replace them with the newest versions.
3. Check the claims on the older pages: "built independently" on Digital Resilience, and the wording of the country count ("six outside Brazil" / "seven counting Brazil").
4. The `www` subdomain is not set up.
5. Porkbun 2FA is off.

## Public contact data

The site and repo publish the email address (shown as text with a copy button), a WhatsApp number (in `hugo.toml`) and the LinkedIn URL, by choice (commit `4068ced`, "WhatsApp contact"). The two résumé PDFs also carry a phone/WhatsApp number on purpose. All of this is in git history, so removing it later does not erase it from older commits.

## Ordering and templates worth knowing

- Pages with a future `date` are not built. To control the order of projects use `weight` in front matter (the Bob project has `weight: 1`, so it is first).
- `baseof.html` already outputs `hreflang` alternate links and sets `<html lang>` per language.
- The language switcher falls back to the other language's home page when a page has no translation.

## Docs in this folder

| File | Purpose |
|---|---|
| `STATE.md` | This file. Current snapshot. |
| `RUNBOOK.md` | How to maintain the site yourself. |
| `DECISIONS.md` | Why things are the way they are. |
| `LOG.md` | Dated, append-only history. |
| `BACKLOG.md` | Changes planned, in priority order. |
