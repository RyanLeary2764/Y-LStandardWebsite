# Deploying ylstandard.com to GitHub Pages

## What's in this folder
- `index.html` — homepage
- `about.html` — about page
- `css/style.css` — all styling (single file, no build step, no external fonts/images)
- `CNAME` — tells GitHub Pages to serve this repo at `ylstandard.com`

No hero photo is included on purpose — the original was a generic stock image hosted on Squarespace's own CDN, and hotlinking it would likely break once you cancel Squarespace. The hero now uses a plain CSS gradient instead, so there's nothing to break and nothing to re-host.

## 1. Push this to a GitHub repo
```
git init
git add .
git commit -m "Initial static rebuild of ylstandard.com"
git branch -M main
git remote add origin https://github.com/<your-username>/ylstandard.git
git push -u origin main
```

## 2. Turn on GitHub Pages
1. In the repo, go to **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to "Deploy from a branch".
3. Branch: `main`, folder: `/ (root)`. Save.
4. Wait a minute, then confirm the site loads at `https://<your-username>.github.io/ylstandard/`.

Do this step BEFORE touching DNS, so you know the site works before you point your real domain at it.

## 3. Point ylstandard.com at GitHub Pages (domain is on Squarespace Domains)
Squarespace Domains has its own DNS panel, separate from your Squarespace *website* — you keep the domain even if you cancel the website plan.

In the Squarespace Domains DNS settings for ylstandard.com, add/replace records so you end up with:

**Apex domain (ylstandard.com) — four A records pointing to GitHub Pages:**
```
A    @    185.199.108.153
A    @    185.199.109.153
A    @    185.199.110.153
A    @    185.199.111.153
```

**www subdomain — one CNAME record:**
```
CNAME   www   <your-username>.github.io.
```

Remove any existing A/CNAME records Squarespace put there for hosting the old site — leave only the ones needed for domain verification/email (MX records, if you use email at this domain, are untouched by any of this).

## 4. Confirm and enforce HTTPS
1. Back in **Settings → Pages** on GitHub, add `ylstandard.com` as the custom domain (this writes the CNAME file for you if it's missing — it's already in this repo).
2. Wait for DNS to propagate (can take a few minutes to a few hours).
3. Once GitHub shows the domain as verified, check **Enforce HTTPS**.

## 5. Verify before you cancel anything
- Load `https://ylstandard.com` and `https://www.ylstandard.com` in an incognito window.
- Check both pages, check the padlock/HTTPS is working, check on mobile.
- Only after this all checks out, downgrade or cancel the Squarespace website plan. Keep the domain itself on Squarespace Domains (or transfer it later if you want) — you are only dropping the website hosting piece, not the domain registration.

## Ongoing edits
There's no CMS here — to change copy, edit the HTML files directly and push:
```
git add .
git commit -m "Update copy"
git push
```
GitHub Pages redeploys automatically within a minute or two of a push to `main`.
