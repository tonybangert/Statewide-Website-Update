# Statewide Publishing — Website

Marketing site for Statewide Publishing (legal-notice fulfillment for Illinois law firms, courts, and local governments).

**Live reference / current production:** https://statewide-publishing.com
**This repo:** the updated site, ready to deploy to a test subdomain for review.

---

## What this is

A plain **static site**. Three HTML pages plus a shared CSS/JS/asset bundle. There is **no build step**, no framework, no `npm install`, no server-side code. Open `index.html` in a browser and it runs.

```
.
├── index.html          Home
├── what-we-do.html     Services / platform
├── contact.html        Contact + inquiry form
└── assets/
    ├── favicon.svg     Site favicon
    ├── css/styles.css  All styling (single file)
    └── js/main.js      Nav, scroll effects, animations
```

All internal links are **relative**, so the site works unchanged whether it lives at a domain root (`https://test.statewide-publishing.com/`) or in a subfolder (`.../staging/`).

### External dependencies
- **Google Fonts** (Inter + Plus Jakarta Sans) loaded from `fonts.googleapis.com`. Needs public internet; nothing to self-host. If the host is locked down, self-host the two fonts and update the `<link>` tags in each page `<head>`.
- Nothing else. No CDN scripts, no jQuery, no trackers.

---

## Deploying to a test subdomain on statewide-publishing.com

The goal is a review URL such as `https://test.statewide-publishing.com` (or `staging.`).

1. **Create the subdomain** in DNS (wherever statewide-publishing.com is hosted): add an `A`/`CNAME` record for `test` pointing at the web server or static host.
2. **Upload the files.** Copy the full contents of this repo (all HTML + the `assets/` folder, preserving structure) to the web root for that subdomain. Any static host works: the existing web server, cPanel/FTP, Netlify, Vercel, Cloudflare Pages, S3 + CloudFront, GitHub Pages, etc.
3. **Set the default document** to `index.html` (default on nearly every host).
4. **Verify** the three pages load, the nav works on mobile, the favicon appears, and fonts render. That confirms relative paths and assets resolved correctly.

No build, environment variables, or runtime are required.

---

## ⚠️ TODO — needs to be completed before go-live

These are intentionally left for the web developer because they depend on Statewide's own accounts/services.

### 1. Wire up the contact form
`contact.html` contains a working, styled form, but it does **not submit anywhere yet**:

```html
<form id="contact-form" action="#" method="post">
```

The form fields (names) are:

| Field | `name` | Required |
|-------|--------|----------|
| First Name | `firstName` | yes |
| Last Name | `lastName` | yes |
| Email | `email` | yes |
| Phone | `phone` | no |
| Organization | `organization` | no |
| I am a… (select) | `type` | no |
| Message | `message` | yes |

**To make it functional, do one of:**
- Point `action` at a form endpoint / backend that emails **info@statewide-publishing.com**, **or**
- Use a hosted form service (Formspree, Basin, Netlify Forms, HubSpot, etc.), **or**
- Add a `fetch()` submit handler in `assets/js/main.js` posting to an API.

Also add client-side success/error feedback (the form has no submit handler in `main.js` today) and basic spam protection (honeypot or captcha).

### 2. Add Google Analytics (GA4)
The site has **no analytics installed**. Add Statewide's GA4 tag to **all three pages**, immediately before the closing `</head>` in each of `index.html`, `what-we-do.html`, and `contact.html`. Replace `G-XXXXXXXXXX` with the real Measurement ID:

```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

(If a shared tag manager is preferred, drop the GTM container snippet instead.) Consider firing a GA event on successful contact-form submit once the form is wired.

### 3. Footer legal links are placeholders
In all three pages the footer **Privacy Policy** and **Terms of Service** links are `href="#"`. Point them at real pages (or remove them) before go-live.

---

## Site contact details (as coded)
- Phone: **(708) 620-8338** (`tel:` links throughout)
- Email: **info@statewide-publishing.com** (`mailto:` in footer)

If any of these change, they appear in the footer and CTA sections of each page.

---

## Handoff summary

| Item | Status |
|------|--------|
| HTML / CSS / JS | ✅ Complete, responsive, no build step |
| Assets (favicon, fonts) | ✅ Intact; fonts via Google CDN |
| Relative paths (subdomain-ready) | ✅ Verified |
| Contact form submission | ⬜ Needs backend/service wiring |
| Google Analytics | ⬜ Needs GA4 Measurement ID installed |
| Privacy / Terms links | ⬜ Placeholder `#` links |

Questions on the source or structure can go back to the Statewide team.
