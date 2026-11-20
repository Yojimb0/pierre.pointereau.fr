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
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --no-pdf-header-footer \
  --print-to-pdf=resume/pierre-yves-pointereau-resume.pdf http://localhost:8765/resume/
kill %1
```

## Deploying

- GitHub repo: `Yojimb0/pierre.pointereau.fr` (to be created), branch `main`.
- Hosting: Cloudflare Pages, connected to the repo. Framework "None", no build command, output directory `/`.
- Domain: `pierre.pointereau.fr`. The `pointereau.fr` domain is registered at OVH. Add the custom domain in the Pages project first, then point `pierre` at `<project>.pages.dev` with a CNAME (at OVH, or in Cloudflare if the zone's nameservers are already Cloudflare's — check in OVH under Domain names → pointereau.fr → DNS servers). Remove any existing A/CNAME record for `pierre` first.

## First session to-do

1. `git init -b main`, commit, and create the GitHub repo `Yojimb0/pierre.pointereau.fr` (public), then push.
2. Help set up the Cloudflare Pages project and the custom domain, as above.
3. Once live, check https://pierre.pointereau.fr, `/resume/`, the PDF link, the old `/cv/cv.html` redirect, and the security headers.
