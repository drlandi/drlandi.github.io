# drlandi.dev: BACKLOG

Planned changes, highest priority first. Mark `[x]` when done and move the detail to LOG.md. Items tagged **[you]** came from Landi; **[suggested]** were found while documenting the site. **[verify]** means the git history suggests it is done or may be wrong, but the file was not read.

> The repo is public. Keep ideas here that are fine for anyone to read; private thoughts go in Obsidian.

## Landi's changes in mind

Fill this section in your own words. The three plans mentioned earlier have already happened:

- [x] **[you]** Bilingual site, English and Português (`4068ced`, 2026-09-29).
- [x] **[you]** AI positioning: tagline and bio rewritten around AI, uLLAMA and typed-decisions projects added (`4068ced`).
- [ ] **[you][verify]** Publish a blog post about uLLAMA. A *project page* for it exists (`ullama`), but the blog has only two posts: `why-im-transitioning` and `c42-factory`.
- [ ] **[you]** _Add your other ideas here._

## Check first (content accuracy, small)

- [ ] **[verify]** The résumé files exist (`static/resume.pdf`, `static/curriculo.pdf`). Open the About page in each language and click the résumé link to be sure it points to the right file. The original link in `about.md` was `/resume.pdf`.
- [ ] **[suggested]** The original About page said "six countries" and the README said "seven". Both pages were edited on 2026-09-29; confirm the number is now the same everywhere, in both languages.
- [ ] **[suggested]** The original `digital-resilience.md` said "Built independently" for what was a team hackathon (Equipe 5). Check the English and Portuguese text and say exactly who did what.
- [ ] **[suggested]** Re-read the older project pages for claims you cannot back up in an interview. They were first drafted from chat notes, not from the repos.
- [ ] **[suggested]** Link each project page to its real repository instead of the general `github.com/drlandi`.
- [ ] **[suggested]** Add a YouTube link when the channel exists. (The LinkedIn link is already set.)
- [ ] **[suggested]** Email address and WhatsApp number are public in the site and in git history. Confirm that is what you want (STATE.md, "Public contact data").

## Reliability

- [ ] **[suggested]** Pin the Hugo version in `.github/workflows/hugo.yml` (`hugo-version: "0.123.7"`, or upgrade the laptop and pin the newer one). The workflow has not changed since 2026-09-25 and still uses `latest`. Multilingual sites make version differences more likely to matter (D8).
- [ ] **[suggested]** Add the `www` record: `CNAME`, host `www`, answer `drlandi.github.io`, at Porkbun. Then confirm `https://www.drlandi.dev` redirects.
- [ ] **[suggested]** Turn on 2FA for the Porkbun account (it holds the domain). Install the Proton Pass browser extension so logins and passkeys can be saved and filled automatically.
- [ ] **[suggested]** Calendar reminder for 2027-09-01 to check the domain renewal and the payment method.
- [ ] **[suggested]** A tiny check before each push that every page has both language files (see RUNBOOK, section 2).

## Features

- [ ] **[suggested]** "Featured" flag for projects: there are now six projects, so the homepage list can grow long. Add `featured: true` in front matter and filter in `layouts/index.html`.
- [ ] **[suggested]** RSS link in the header or footer (Hugo already generates `index.xml`).
- [ ] **[suggested]** Favicon (the original template used an empty data URI) and a social-preview image.
- [ ] **[suggested]** Check the site on a phone and with a screen reader, in both languages.
- [ ] **[suggested]** `hreflang` tags so search engines connect each page to its translation (check what `baseof.html` already outputs first).
- [ ] **[suggested]** Page order control (`weight` in front matter) for projects.

## Content pipeline

- [ ] **[suggested]** Blog post: the Jekyll-versus-Hugo incident, written from LOG.md. It shows real debugging with `curl` headers.
- [ ] **[suggested]** Blog post: the campus-firewall SSH problem and the `gh auth login` fix (already drafted bilingually for LinkedIn).
- [ ] **[suggested]** A deep-dive post on the Digital Resilience architecture.
- [ ] **[suggested]** LinkedIn posts for each new blog post, bilingual, Portuguese first, with @42SP and #42, kept in AppFlowy.

## Done

- [x] Domain registered, DNS pointed at GitHub Pages (2026-09-25)
- [x] Site built and deployed with GitHub Actions (2026-09-25)
- [x] Custom domain verified, HTTPS enforced (2026-09-26)
- [x] Bilingual site, AI positioning, résumés, LinkedIn and WhatsApp contact (2026-09-29)
- [x] Projects added: typed-decisions-demo, ullama, self-correcting-activity-generator (2026-09-29)
- [x] Blog post: the C code factory, EN + PT (2026-10-11)
- [x] Documentation system created (2026-10-21)
