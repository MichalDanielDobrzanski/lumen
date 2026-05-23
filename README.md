# Lumen — marketing site

Static site for Lumen, designed to be hosted on GitHub Pages with no
domain. The **main app repo is private**; this folder is intended to
live in a **separate public repo** so GitHub Pages can serve it for
free.

## Deploy as a separate public repo

The recommended setup keeps the app private and the site public.

1. **Create a new public GitHub repo** — name it `lumen` (or anything
   you like — the URL becomes `<username>.github.io/<repo>/`).
2. **Copy this folder out** of the private app repo:

   ```bash
   # from your home directory
   git clone https://github.com/<username>/lumen.git lumen-site
   cp -R /path/to/private/repo/website/. lumen-site/
   cd lumen-site
   git add -A
   git commit -m "Initial site"
   git push -u origin main
   ```

3. **Enable GitHub Pages**: in the new repo's Settings → Pages →
   Source: "Deploy from branch", branch `main`, folder `/` (root).
4. After a minute the site lives at
   `https://<username>.github.io/lumen/`.

That's it — no build, no domain, no DNS.

## When you update the site

Because this folder lives in two repos, treat the private one as the
working copy and `rsync` / `cp` over to the public repo when you're
ready to publish:

```bash
rsync -av --delete --exclude .git \
  /path/to/private/repo/website/ \
  /path/to/public/lumen-site/
cd /path/to/public/lumen-site
git add -A && git commit -m "Update" && git push
```

You could also make the private `website/` folder a git submodule of
the public repo — that's tidier but adds the usual submodule friction.
For a static four-page site, the rsync approach is enough.

## Pages

- `index.html` — landing
- `privacy.html` — privacy policy (linked from in-app Settings)
- `terms.html` — terms of use (linked from in-app Settings)
- `support.html` — contact + crisis hotlines + FAQ
- `assets/style.css` — single stylesheet, no JS, no framework
- `assets/icon.png`, `og.png`, `favicon.svg` — visual assets

## Styling

- Mirrors the in-app palette (`#6B5DD8` violet, `#FBF8F3` cream,
  `#0F0820` indigo-deep).
- Loads Lora + Inter + UnifrakturMaguntia from Google Fonts.
- Dark mode is `prefers-color-scheme`-driven.

## Update the in-app + fastlane links if the URL differs

Three places need the same site URL:

- `fastlane/metadata/{ios,android}/lumen/<locale>/{privacy_url,marketing_url,support_url}.txt`
- `tools/icon_gen/*.py` (not a URL — but the brand assets feed both)
- App Store / Play Console listing fields (uploaded via
  `fastlane sync_metadata`)

The defaults assume `michaldobrzanski.github.io/lumen`. If your public
repo lives somewhere else, sed-replace before pushing fastlane.
