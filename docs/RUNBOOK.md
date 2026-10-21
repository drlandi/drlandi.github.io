# drlandi.dev: RUNBOOK

How to maintain the site yourself, without needing a chat. Every command here was used during the build unless marked *(new)*.

> The repo is public. Never paste passwords or tokens into these docs.

## 0. Mental model (read once)

```
you edit files in ~/dev/site
   └─ git push to main
        └─ GitHub Actions runs "Deploy Hugo site to GitHub Pages"
             ├─ builds the site with Hugo into public/
             └─ publishes public/ to GitHub Pages
                  └─ drlandi.dev (DNS A records) → GitHub's servers → your build
```

Three independent layers, three independent failure modes: **content/build** (your repo and Actions), **hosting** (GitHub Pages settings), **domain** (Porkbun DNS). When something breaks, work out which layer it is first.

## 1. Preview locally

```bash
cd ~/dev/site
hugo server -D
```

Open http://localhost:1313 for English and http://localhost:1313/pt-br/ for Português. It reloads when you save. Stop it with `Ctrl+C`.
`-D` includes drafts, so the local preview can show pages that **will not** appear on the live site (see gotchas).

## 2. Add a blog post (always two files)

The site is bilingual. Every page is a **pair of files**: the English one, and the Portuguese one with the same name plus `.pt-br`:

```
content/blog/my-post-title.md
content/blog/my-post-title.pt-br.md
```

Easiest way to start: copy an existing pair (`c42-factory.md` and `c42-factory.pt-br.md`) and change the text, so you keep the front matter fields the templates expect. A minimal version looks like this:

```markdown
---
title: "The title"
summary: "One sentence shown in lists and on the homepage."
date: 2026-10-21
---

Body in Markdown.
```

Rules that bite:

- **Both languages, or the page is missing in one of them.** A post with only the English file normally does not appear under `/pt-br/`, and the language switcher has nothing to link to.
- **No `draft: true`** unless you really want it hidden. The CI build does not use `-D`, so drafts never go live.
- **The date must not be in the future.** Hugo skips future-dated pages by default. If a post is missing after a deploy, check the date first.
- The file name becomes the URL: `my-post-title.md` → `/blog/my-post-title/` and `my-post-title.pt-br.md` → `/pt-br/blog/my-post-title/`.
- Images go in `static/images/` and are referenced as `/images/name.png`. Use one image per language when the picture contains text (as in the C factory post).

## 3. Add a project page

Same pair-of-files rule, in `content/projects/`. There are six projects now and the homepage lists them, so a seventh makes the list longer. See BACKLOG for the "featured" idea.

## 3b. Change the résumé

Replace `static/resume.pdf` (English) and `static/curriculo.pdf` (Português), keeping the same file names so the links keep working. Check both links after the deploy.

## 4. Edit the About page or site info

- About text: `content/about.md` (English) and `content/about.pt-br.md` (Português)
- Tagline and bio: `hugo.toml`, one block per language (`[languages.en.params]` and `[languages.pt-br.params]`)
- Name, email, GitHub, LinkedIn, WhatsApp number (digits only): `hugo.toml`, `[params]`
- Button and menu text in each language: `i18n/en.toml` and `i18n/pt-br.toml`
- Look and feel: `static/css/style.css`
- Page structure: `layouts/` (language switcher: `layouts/partials/lang-switcher.html`)

## 5. Publish

```bash
cd ~/dev/site
git status
git add .
git commit -m "Short description of the change"
git push
```

Then watch https://github.com/drlandi/drlandi.github.io/actions. A green check usually appears in under a minute. Then:

```bash
curl -s https://drlandi.dev | grep -i "generator\|<title>"
```

GitHub's CDN caches pages for up to 10 minutes (`cache-control: max-age=600`). If the check looks stale, wait and retry before assuming a fault.

## 6. Roll back a bad change

```bash
cd ~/dev/site
git log --oneline          # find the commit that broke things
git revert <commit-sha>    # creates a new commit that undoes it
git push
```

`git revert` is safer than rewriting history: nothing is lost and the deploy runs again by itself. You can also re-run an old successful workflow from the Actions tab, but the next push will override it.

## 7. Domain and DNS reference

At Porkbun → drlandi.dev → DNS Records, the root domain has four `A` records, TTL 600:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

These are GitHub Pages' public addresses, the same for every GitHub Pages site. Which site is served is decided by the custom domain set in the repo (Settings → Pages → Custom domain = `drlandi.dev`), not by the IP. The Porkbun default records (`ALIAS` and `*` `CNAME` to `pixie.porkbun.com`) were deleted so they would not compete with these.

The `www` subdomain was **not set up** in the build session (check Porkbun to confirm; see BACKLOG). When you add it: type `CNAME`, host `www`, answer `drlandi.github.io`.

Domain expires 2027-09-25 and auto-renews at about $12.87/year. Keep a payment method valid.

## 8. Troubleshooting

### drlandi.dev shows my README (a plain styled page) instead of the site

This happened on 2026-09-25. `curl -s https://drlandi.dev | grep -i generator` showed `Jekyll v3.10.0`. GitHub Pages' classic builder had built a page from `README.md` because no `index.html` existed at the repo root.

Check, in order:

1. Settings → Pages → **Source** must be **GitHub Actions**.
2. Actions tab: the latest "Deploy Hugo site to GitHub Pages" run must be green.
3. If both are right and the site is still Jekyll, push any small commit (or run the workflow manually from the Actions tab: Run workflow). The 2026-09-25 incident was resolved this way.
4. Re-test with the `curl` command. Allow up to 10 minutes for the CDN.

### The Actions run is red

Open the run → click the failed step → read the last 20 lines of the log. The usual suspects:

- A Hugo version mismatch (the workflow uses `latest`; a new Hugo release can change behaviour). See BACKLOG about pinning.
- A typo in `hugo.toml` or in a page's front matter (quotes, `---` markers).
- Reproduce locally with `hugo --minify`; if it fails locally too, it is your content, not GitHub.

### "DNS check failed" in Settings → Pages

- Compare the four `A` records at Porkbun against section 7, character by character.
- Make sure there are no leftover `ALIAS`/`CNAME` records on the root domain.
- DNS can take minutes to hours to propagate; the check succeeded within a few minutes in September.

### HTTPS is unavailable or the browser warns about the certificate

After a custom domain is added, GitHub issues the certificate on its own, usually within 15–30 minutes. "Enforce HTTPS" is greyed out until then. If it stays greyed out for more than a day, remove and re-add the custom domain.

### A new page does not appear

In this order: draft flag, future date, missing `summary` (the page exists but has no blurb), wrong folder, and a stale CDN.

## 8b. Working with Claude on site changes

Claude cannot push to GitHub. The workflow used so far:

1. Claude clones the public repo in its own sandbox, makes the change and builds it with Hugo to test.
2. Claude delivers a `.patch` file.
3. You apply it: `cd ~/dev/site`, then `git apply <patch-file>`.
4. Preview with `hugo server -D`, check both `/` and `/pt-br/`.
5. Commit and push (section 5).

Start any new chat about the site by pasting `docs/STATE.md` (and `BACKLOG.md` if the change is planned work). Tell Claude the last commit hash from `git log --oneline -1` so it clones the same version you have.

## 9. Things that are *not* in git

- The built site (`public/`) and `.hugo_build.lock`: both are in `.gitignore`. They are regenerated by every build.
- Secrets: none are needed. The workflow uses GitHub's built-in permissions only.
- The résumé source files (`.docx`) and the scripts that generated the PDFs. The PDFs in `static/` are the output only; keep the `.docx` files somewhere safe (Obsidian vault or backup) and re-upload them to Claude when the résumé changes.
