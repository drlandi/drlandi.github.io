# drlandi.dev: DECISIONS

Why things are the way they are. One entry per decision: context, choice, reasoning, and what would make us revisit it. Add new entries at the bottom; never rewrite old ones (mark them superseded instead).

> The repo is public. Keep rationale here, secrets elsewhere.

---

## D1. Domain: `drlandi.dev`, not `drlandi.ai`

- **Date:** 2026-09-25
- **Context:** `drlandi.ai` was the first choice (short, matches the AI-engineering direction). Porkbun listed it at $82.70/year, renewing at $82.70, with no first-year discount.
- **Decision:** register `drlandi.dev` instead: $8.75 for the first year, renewing at about $12.87.
- **Reasoning:** the job search is the priority and income is not there yet; a recurring cost nine times higher than needed for the same professional signal is not justified. `.dev` is also a respected developer TLD and enforces HTTPS by design.
- **Revisit when:** a job is landed and the cost is trivial. Then `.ai` can be added as a second domain that redirects, and `.dev` kept.

## D2. Registrar: Porkbun

- **Date:** 2026-09-25
- **Reasoning:** transparent pricing, no aggressive upselling (compared with GoDaddy), clean DNS editor. WHOIS privacy is included. Registration requires ID verification, which was completed.
- **Note:** the login is stored in Proton Pass with a generated password. The passkey dialog on Porkbun listed only Google Password Manager and a USB security key. Proton Pass's browser extension was not installed at the time (its web app showed an "Install extension" banner), which is the likely reason it was not offered. Installing the extension is a BACKLOG item, as is 2FA on the Porkbun account.

## D3. Static site generator: Hugo (not Jekyll)

- **Date:** 2026-09-25
- **Context:** Hugo and Jekyll were both proposed. Jekyll is GitHub Pages' native builder; Hugo is not.
- **Decision:** Hugo, confirmed explicitly by Landi ("Stick with Hugo").
- **Reasoning:** Hugo is a single binary with very fast builds, no Ruby toolchain to maintain, and a simple content model (Markdown files in folders). Because GitHub Pages does not build Hugo itself, the site is built in GitHub Actions and the output is published (D6).
- **Cost:** a build step exists that Jekyll would not need, and Hugo's version needs managing (see D8).

## D4. Templates written by hand, no theme

- **Date:** 2026-09-25
- **Decision:** `layouts/` contains a handful of small templates (six at the start, nine files plus a shortcode after the bilingual change); there is no `themes/` folder and no theme submodule.
- **Reasoning:** the target look is deliberately minimal (Karpathy/Torvalds): text first, no JavaScript, almost no CSS. A theme would bring features and upgrade work that this site does not need. With hand-written templates every line is understood and maintainable by the owner.
- **Cost:** features such as RSS links, tag pages, or search must be added by hand.

## D5. Hosting: GitHub Pages, repo named `drlandi.github.io`

- **Date:** 2026-09-25
- **Reasoning:** free, versioned with the code, HTTPS included, and the same GitHub account that already holds the other repos. The repository name `drlandi.github.io` is a GitHub convention that marks a *user site* served from the root; renaming the repo would turn it into a project site with a path prefix. The repo is public because the free plan requires it for Pages.
- **Local folder:** the clone lives at `~/dev/site`. A local folder name is cosmetic and does not affect git.

## D6. Deploy through GitHub Actions, Pages source = "GitHub Actions"

- **Date:** 2026-09-25
- **Decision:** `.github/workflows/hugo.yml` builds with `hugo --minify` and publishes via `actions/upload-pages-artifact` and `actions/deploy-pages`.
- **Reasoning:** the "deploy from a branch" mode only understands plain HTML or Jekyll. With Actions, `public/` never has to be committed, and a push is the only deploy step.
- **Evidence it matters:** on first deploy GitHub served a Jekyll page built from `README.md` instead of the Hugo site (see LOG, 2026-09-25). It was fixed with a second push after the source was set to GitHub Actions.

## D7. DNS: four `A` records on the root, no `ALIAS`/`CNAME` on the apex

- **Date:** 2026-09-25
- **Reasoning:** a root (apex) domain cannot carry a standard `CNAME`, so GitHub's four documented IPs are used as `A` records (four for redundancy). Porkbun's default `ALIAS` and wildcard `CNAME` to its parking page were deleted so that they cannot compete with the new records.
- **Pending:** a `www` `CNAME` to `drlandi.github.io` (BACKLOG).

## D8. Hugo version is *not* pinned yet (known risk)

- **Date:** 2026-10-21 (recorded; not a decision to keep it)
- **State:** the local machine runs 0.123.7, CI uses `latest`, and the first live build used 0.166.0.
- **Risk:** a future Hugo release could change behaviour so that the site builds on the laptop but fails (or renders differently) on GitHub.
- **Proposed fix:** set `hugo-version` in `hugo.yml` to the version used locally, and upgrade both together on purpose. Tracked in BACKLOG.

## D9. Documentation lives in the repo (`docs/`), with Obsidian as a map

- **Date:** 2026-10-21
- **Context:** the site's history existed only in chat threads, and a later chat found almost none of it in memory.
- **Decision:** five files in `docs/` (STATE, RUNBOOK, DECISIONS, LOG, BACKLOG). The GitHub repo is the source of truth. Obsidian holds one short map note that links to them plus private thinking. AppFlowy holds the content pipeline (post ideas, LinkedIn posts).
- **Reasoning:** one copy of each fact. Git gives the history and the backup for free, and a public engineering log doubles as portfolio material.
- **Constraint:** the repo is public, so nothing private goes in `docs/`.

---

## D10. Bilingual with Hugo's multilingual mode: English at the root, Português under `/pt-br/`

- **Date:** 2026-09-29 (commit `4068ced`). Recorded 2026-10-21 from the commit and `hugo.toml`; the reasons below are inferred from the configuration, so edit them if your own reasons were different.
- **Decision:** `defaultContentLanguage = "en"` with `defaultContentLanguageInSubdir = false`, a `[languages.pt-br]` block, translated pages as `name.pt-br.md` next to `name.md`, and interface strings in `i18n/en.toml` and `i18n/pt-br.toml`. A language switcher sits in the header.
- **Reasoning:** the audience is both Brazilian recruiters and international ones, and the LinkedIn posts are already bilingual. Hugo's own multilingual mode links each page to its translation and keeps one codebase, rather than two sites. Keeping English at the root means the URLs that already existed (and any links shared before the change) did not move.
- **Cost:** every new page needs two files, and a missing translation means the page is absent in one language. This is the reason RUNBOOK section 2 insists on pairs.
- **Revisit when:** a third language is wanted (the structure supports it) or the double maintenance becomes a burden.

## D11. Positioning: "AI & Infrastructure Engineer"

- **Date:** 2026-09-29 (commit `4068ced`).
- **Decision:** the tagline changed from "Cloud & Infrastructure Engineer" to "AI & Infrastructure Engineer"; the bio now presents 25+ years of mission-critical systems as the foundation for AI applications and local-LLM tooling "from a C inference runtime to cloud-native pipelines". Projects on uLLAMA, typed decisions and the activity generator were added the same day.
- **Reasoning:** the site is the anchor of the job search, and the target roles named earlier were broad (cloud, DevOps/SRE, backend). The recent work that is distinctive is the AI and local-model tooling, so the first line a recruiter reads now says so, while the infrastructure experience is kept as the credibility behind it.
- **Revisit when:** the job search narrows to one target role, or the AI projects stop being the most recent and strongest work.
