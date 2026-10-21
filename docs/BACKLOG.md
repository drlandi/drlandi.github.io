# drlandi.dev: BACKLOG

Planned changes, highest priority first. Mark `[x]` when done and move the detail to LOG.md. Items tagged **[you]** came from Landi; **[suggested]** were found while documenting the site. **[verify]** means the git history suggests it is done or may be wrong, but the file was not read.

> The repo is public. Keep ideas here that are fine for anyone to read; private thoughts go in Obsidian.

## Landi's changes in mind

Fill this section in your own words. The three plans mentioned earlier have already happened:

- [x] **[you]** Bilingual site, English and Português (`4068ced`, 2026-09-29).
- [x] **[you]** AI positioning: tagline and bio rewritten around AI, uLLAMA and typed-decisions projects added (`4068ced`).
- [ ] **[you][verify]** Publish a blog post about uLLAMA. A *project page* for it exists (`ullama`), but the blog has only two posts: `why-im-transitioning` and `c42-factory`.
- [ ] **[you]** LinkedIn post announcing the new bilingual site: Portuguese first, then English, flag emojis, always @42SP and #42. Draft it in AppFlowy.
- [ ] **[you]** _Add your other ideas here._

## Check first (content accuracy, small)

- [ ] **[verify]** The résumé links: the English About page links `/resume.pdf` and the Portuguese one `/curriculo.pdf`. Click both on the live site. (The old link was broken before 2026-09-29.)
- [ ] **[suggested]** Countries: the résumé work found that "six" and "seven" are both true (six countries outside Brazil, seven counting Brazil). Make the wording say which, on the About pages and README, in both languages.
- [ ] **[you]** The two résumé PDFs on the site may be older than your latest versions. A later version added "AWS Certified Cloud Practitioner (2024)". Replace `static/resume.pdf` and `static/curriculo.pdf` with the newest ones, then commit and push. The editable sources are the `.docx` files, which live on your computer, not in the repo.
- [ ] **[verify]** Click the live-demo link on the Bob project page (Railway) before relying on it: it depends on the hackathon key and may stop working. If it does, say so on the page.
- [ ] **[suggested]** The original `digital-resilience.md` said "Built independently" for what was a team hackathon (Equipe 5). Check the English and Portuguese text and say exactly who did what.
- [ ] **[suggested]** Re-read the older project pages for claims you cannot back up in an interview. They were first drafted from chat notes, not from the repos.
- [ ] **[suggested]** Link each project page to its real repository instead of the general `github.com/drlandi`.
- [ ] **[suggested]** Add a YouTube link when the channel exists. (The LinkedIn link is already set.)
- [ ] **[suggested]** Email address and WhatsApp number are public in the site and in git history. Confirm that is what you want (STATE.md, "Public contact data").

## Project repos behind the site (found in the earlier chat's handoff)

- [ ] **[suggested]** The Bob repo (`drlandi/ibm-bob-activity-generator`) has no `README.md`. Write one; the project page already has most of the content.
- [ ] **[suggested]** The `ullama` repo has clutter (`src/aso`, `src/eroou`, `src/erros.txt`) and the Python `factory/` folder was not in the public repo. The project page says the fine-tuning and offline factory are *planned*, so keep the page and the repo telling the same story.
- [ ] **[suggested]** The Netlify demo URLs have random names (for example `singular-kashata-…`). Consider renaming them and updating the project page.
- [ ] **[suggested]** Check that the LinkedIn headline and the CIEBP dates ("Professor Especialista", Feb 2023 to present) match the résumé.
- [ ] **[suggested]** Optional: a "Write in Gmail" compose link next to the email copy button (offered, not added).

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
- [x] Email shown as text with a copy button, replacing the `mailto:` link that opened a mail-app prompt (2026-09-29)
- [x] `hreflang` alternate links, `<html lang>` per language and localized dates are already in the templates (2026-09-29)
- [x] Bob project page and LinkedIn footer link are in `main` (`e1f4dc1`) and its deploy run was green
- [x] Documentation system created and committed (`a0ad578`, 2026-10-21)
