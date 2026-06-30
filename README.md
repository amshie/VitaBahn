# VitaBahn — HADP Investor Brief

A confidential, single-page investor brief. Static site (HTML/CSS/JS, self-hosted
IBM Plex fonts) with a lead-capture form backed by **Netlify Forms**.

The page content, copy, numbers, and legal disclaimers are the approved, signed-off
version and are unchanged. The only functional change versus the original single file:
the request form no longer opens the visitor's mail app — it submits to a real
backend that **captures every lead and emails the team**.

## Project structure

```
.
├── index.html        # markup (approved content, unchanged)
├── styles.css        # all styles (was inline <style>)
├── app.js            # nav, scroll-reveal, HADP animation, form submit
├── fonts/            # self-hosted IBM Plex woff2 (OFL-1.1), no external requests
│   ├── IBMPlexSans-Regular.woff2  …  -Medium / -SemiBold / -Bold
│   └── IBMPlexMono-Regular.woff2  …  -Medium / -SemiBold
├── netlify.toml      # publish dir + caching/security headers
└── README.md
```

## Run locally

Any static file server works for **viewing** the page:

```bash
npx serve .
# or:  python -m http.server 8000
```

To test the **form end-to-end** locally (so the Netlify Forms endpoint is emulated),
use the Netlify CLI instead:

```bash
npm i -g netlify-cli
netlify dev
```

> Note: a plain static server will *render* the page perfectly, but a form
> submission only reaches a real inbox/dashboard once the site is on Netlify
> (or running under `netlify dev`).

## Deploy (Netlify)

**Easiest — drag & drop**

1. Create a free account at <https://app.netlify.com>.
2. Go to <https://app.netlify.com/drop> and drag this whole folder onto the page.
3. Done — Netlify gives you a live URL.

**Or — connect Git** (auto-deploy on push)

1. Push this folder to a Git repo and "Add new site → Import an existing project".
2. Build command: *(leave empty)* — Publish directory: `.`
   (Both are already set in `netlify.toml`.)

### One-time setup after the first deploy — IMPORTANT

The form is detected automatically (it has `data-netlify="true"` + a hidden
`form-name`). Submissions are stored in the Netlify dashboard, but to also get
them **emailed to invest@vitabahn.com** you must add the notification once:

1. Netlify dashboard → your site → **Forms**.
2. **Form notifications** → **Add notification** → **Email notification**.
3. Enter **invest@vitabahn.com** and save.

(If you don't see the form after deploying, confirm **Forms** → form detection is
enabled, then redeploy.)

### Where the leads go

- **Stored:** Netlify dashboard → Forms → `investor-request` (viewable + CSV export).
- **Emailed:** to invest@vitabahn.com once the notification above is configured.

A built-in honeypot (`bot-field`) filters obvious spam. Client-side validation
(required fields + email format) runs before submit, exactly as before.

## Custom domain & email (vitabahn.com)

1. Netlify → **Domain settings** → add `vitabahn.com` and follow the DNS steps.
2. Set up a mailbox or forwarding for **invest@vitabahn.com** at your registrar
   (e.g. Porkbun) so the form notifications — and any investor who clicks the
   footer email link — actually reach you.

## Notes

- The brief is intentionally `noindex, nofollow` (meta tag + `X-Robots-Tag`
  header) — it should not appear in search engines.
- Fonts are fully self-hosted; the page makes **no third-party requests**.
