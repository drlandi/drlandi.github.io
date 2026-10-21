# drlandi.dev: LOG

Dated, append-only history. Newest entry at the bottom. One entry per working session. Times are local (America/Sao_Paulo). Facts only: decisions and reasons go in DECISIONS.md, plans in BACKLOG.md.

> The repo is public. No secrets in this file.

Source note: entries from 2026-09-25 to 2026-09-26 come from the build session. Entries from 2026-09-29 and 2026-10-11 were reconstructed on 2026-10-21 from `git log` and `git show --stat`, so they list *what changed*, not what was discussed.

---

## 2026-09-25: Domain, DNS, repo, first deploy

**Domain**
- Tried `drlandi.ai` at Porkbun: available at $82.70/year. Decided against it (D1).
- Registered `drlandi.dev` at Porkbun, $8.75 for the first year. Created the Porkbun account with a Proton Pass-generated password and completed ID verification. Registry create date 2026-09-25, expiry 2027-09-25.
- Porkbun showed a suggestion to set up 2FA. Not done yet.

**DNS (around 13:22–13:29)**
- Opened DNS Records for drlandi.dev. It had two Porkbun defaults: `ALIAS` drlandi.dev → pixie.porkbun.com and `CNAME` *.drlandi.dev → pixie.porkbun.com. Deleted both.
- Added four `A` records on the root domain (host left blank) pointing to 185.199.108.153, .109.153, .110.153, .111.153, TTL 600.
- Mistake noted for next time: the first plan said to point `www` at `dlandi.github.io`. The GitHub username is `drlandi`, so the correct target is `drlandi.github.io`. The final DNS screenshot showed only the four `A` records, so the `www` record was not added during this session.

**Repo and site**
- Created the public repo `drlandi/drlandi.github.io`, empty (no README initialised).
- Built the Hugo site: `hugo.toml`, six hand-written templates, one CSS file, about page, three project pages, one blog post, `static/CNAME`, the Actions workflow and a README. Verified with `hugo --minify`: 16 pages, no errors.
- On the laptop: cloned the repo to `~/dev/` and renamed the folder to `~/dev/site`. Unzipping the site archive produced a stray folder literally named `{content` (an artefact of a shell brace pattern when the archive was created). Removed it with `find . -maxdepth 1 -name '{*' -exec rm -rf {} \;`, then copied the clean files into `~/dev/site`.
- Installed Hugo with `sudo apt install hugo` (0.123.7). Local build: 16 pages, clean.
- First push: commit `7fb1fde`, "Initial site: Hugo scaffold with about, projects, and first post".

**Hosting settings (around 23:42–23:47)**
- Settings → Pages: source GitHub Actions.
- Custom domain `drlandi.dev` saved; "DNS check successful". "Enforce HTTPS" was greyed out while the certificate was being issued.

**The Jekyll incident (around 23:48–00:02)**
- `drlandi.dev` showed the repo README rendered as a plain page, even in a private window.
- Both runs in the Actions tab were green: "Deploy Hugo site to GitHub Pages" (25s) and "pages build and deployment" (46s). The second one is GitHub's classic builder.
- `curl -s https://drlandi.dev | head -40` showed `<meta name="generator" content="Jekyll v3.10.0" />`. So the page was GitHub's Jekyll fallback, not the Hugo build.
- `ls -la .github/workflows/` showed only `hugo.yml`, so there was no second workflow file. `ls -la` also showed `public/` and `.hugo_build.lock` in the working folder (from the local test build).
- Fix: added a `.gitignore` containing `public/` and `.hugo_build.lock`, ran `git rm -r --cached public .hugo_build.lock`, committed (`04853dc`, "Add gitignore, remove committed build artifacts") and pushed. The new Actions run was green.
- Likely cause (not proven): the first deployment came from the classic builder before the Pages source was switched to GitHub Actions; the second push deployed under the right source. Treat as "probable" in future debugging.

## 2026-09-26: Confirmed live

- `curl -s https://drlandi.dev | grep -i generator` → `<meta name=generator content="Hugo 0.166.0">`, title "Daniel Landi — Cloud & Infrastructure Engineer".
- Settings → Pages (00:04): "Your site is live at https://drlandi.dev/", custom domain verified, "Enforce HTTPS" checked.
- Browser check (00:10): homepage with hero, featured projects and recent writing renders correctly.

## 2026-09-29: Bilingual site and AI positioning (2 commits)

- `4068ced` "Bilingual site, AI positioning, résumés, WhatsApp contact" (27 files, +595/−43):
  - Switched to Hugo multilingual mode: English at the root, Português under `/pt-br/`. Added `.pt-br.md` translations of the about page, the projects index, the blog index, the existing blog post and the three existing project pages. Added `i18n/en.toml` and `i18n/pt-br.toml`.
  - New templates: `layouts/partials/lang-switcher.html`, `layouts/partials/email.html`, `layouts/shortcodes/email.html`. Edited `baseof.html`, `single.html`, `index.html`, `header.html`, `footer.html` and `style.css`.
  - `hugo.toml` rewritten (37 lines changed): per-language `tagline` and `bio`, new `linkedin` and `whatsapp` parameters. The tagline went from "Cloud & Infrastructure Engineer" to "AI & Infrastructure Engineer".
  - Added `static/resume.pdf` and `static/curriculo.pdf` (about 79 KB each).
  - Added two new projects in both languages: `typed-decisions-demo` and `ullama`.
  - Updated `README.md` and `content/about.md`.
- Context from the earlier chat's handoff (not visible in git):
  - Email: the `mailto:` link opened a mail-app setup prompt because Pop!_OS has no mail client and Gmail is used in the browser. It was replaced by the visible address with a copy button (partial `email.html`, shortcode `{{< email >}}`, a small script in the footer).
  - The site changes were prepared in Claude's sandbox, built with Hugo 0.152.2 for testing, and delivered as patch files that were applied locally with `git apply`, previewed, committed and pushed by Landi. Claude cannot push to GitHub.
  - Bilingual details: header links built with `relLangURL`, the language switcher falls back to the other language's home page when a page has no translation, `<html lang>` follows the language, `hreflang` alternate links are in `baseof.html`, dates are localized.
  - Project pages: `ullama` states that fine-tuning and the offline factory are planned, not done; `typed-decisions-demo` states the typed model is simulated; the Bob project has `weight: 1` so it sorts first, and a "Limits" section (in-memory store, validator not wired into the UI, English-only interface).
  - New résumés were reconciled from Lattes and older CVs (six countries outside Brazil, seven counting Brazil; unsupported claims removed; phone/WhatsApp included on purpose).
- `e1f4dc1` "Add Bob activity generator project and LinkedIn link" (4 files): added `self-correcting-activity-generator` (EN + PT), a one-line `hugo.toml` change and a README update.

## 2026-10-11: Blog post on the C code factory (2 commits)

- `119def3` "Add blog post: the C code factory (EN + PT)": added `content/blog/c42-factory.md` and `c42-factory.pt-br.md`, two images (`static/images/factory-en.png`, `fabrica-pt.png`) and two CSS lines for them.
- `acc150a` "Blog: add 5-attempt result": edited both language versions of the post (about 19 lines each).
- `main` and `origin/main` both point at `acc150a`: everything is pushed.

## 2026-10-21: Documentation system

- Decided to keep the site's history in the repo instead of in chat threads (D9).
- Created `docs/STATE.md`, `RUNBOOK.md`, `DECISIONS.md`, `LOG.md`, `BACKLOG.md`, plus an Obsidian map note for The-Forge. Reconciled them with the git history (the two entries above).
- Committed and pushed as `a0ad578` ("Add project documentation system (docs/)"). The Actions run for it is the first docs-only deploy.
- Read the handoff summary of the earlier chat where the 2026-09-29 work was done, and merged its facts (see the 2026-09-29 notes above and BACKLOG).

<!-- Add the next entry below. Template:

## YYYY-MM-DD: short title
- What changed (files, settings)
- What broke and what fixed it
- Commit hashes
-->
