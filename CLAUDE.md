# pierre.pointereau.fr

Personal home page and resume of Pierre-Yves Pointereau (senior software engineer, front-end / UI architect, Sydney). Replaces a decade-old site at the same address.

## What's here

- `index.html` — home page: short intro, selected work, links.
- `resume/index.html` — the resume as a web page; prints cleanly to A4.
- `resume/pierre-yves-pointereau-resume.pdf` — PDF generated from the resume page (see below).
- `style.css` — the only stylesheet. Light and dark via `prefers-color-scheme`, plus a print stylesheet.
- `favicon.svg`, `404.html`.
- `_headers` — security headers for Cloudflare Pages (strict CSP: no scripts, styles from self only, so no inline `style=` attributes).
- `_redirects` — sends the old site's `/cv/…` URLs to `/resume/`.

## Principles

- Plain static HTML and CSS. No JavaScript, no build step, no web fonts, no tracking.
- The site is part of the pitch: keep it accessible (WCAG 2.2 AA), fast, and free of layout shift.
- Minimal, text-first style in the spirit of deadsimplesites.com.
- Never put the phone number or other private details on the public site.
- The source of truth for the resume content is the "Pierre-Yves Pointereau — Resume" doc in the "Job search" Claude project. When it changes, update `resume/index.html` to match, then regenerate the PDF.

## Regenerating the PDF

Serve the folder (root-relative links need a server), then print the resume page with headless Chrome:

```bash
python3 -m http.server 8765 &
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --no-pdf-header-footer --generate-pdf-document-outline \
  --print-to-pdf=resume/pierre-yves-pointereau-resume.pdf http://localhost:8765/resume/
kill %1
```

## Deploying

Push to `main` and Cloudflare Pages deploys it automatically. Other branches and PRs get preview URLs on `pierre-pointereau-fr.pages.dev`.

- GitHub repo: `Yojimb0/pierre.pointereau.fr` (public), branch `main`. Commits use `py.pointereau@gmail.com` (set in the repo's git config), not the work email.
- Hosting: Cloudflare Pages project `pierre-pointereau-fr` (a Pages project, not a Worker). Framework "None", no build command, output directory `/`.
- Domain: `pierre.pointereau.fr`, a custom domain on the Pages project. `pointereau.fr` is registered at OVH, but its DNS is on Cloudflare, so the `pierre` CNAME lives in the Cloudflare zone.
- Email Address Obfuscation is off for the `pointereau.fr` zone. Leave it off: its script is blocked by the CSP, so the email links would break.

## After a deploy, check

https://pierre.pointereau.fr, `/resume/`, the PDF link, the old `/cv/cv.html` redirect (301 to `/resume/`), and the security headers (`curl -sI`).
