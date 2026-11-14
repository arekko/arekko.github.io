# Liftyx website

Static marketing, privacy policy and support pages for
[Liftyx](https://liftyx.app/), a private workout tracker for iPhone. The site
supplies the App Store Connect **Privacy Policy URL** and **Support URL**.

- **Live site:** https://liftyx.app/
- **Stack:** plain HTML and CSS, no build step, no cookies
- **Hosting:** GitHub Pages user site, published from `main`

> [!IMPORTANT]
> This repository is a **published mirror**. The source of truth is `docs/` in
> the private app repository. Make changes there and deploy them (see
> [Deploying](#deploying)). Anything committed only here is overwritten by the
> next deploy's `rsync --delete`.

## Contents

- [Pages](#pages)
- [Repository layout](#repository-layout)
- [Local preview](#local-preview)
- [Deploying](#deploying)
- [App Store links](#app-store-links)
- [Web analytics](#web-analytics)
- [SEO](#seo)
- [App Store Connect field mapping](#app-store-connect-field-mapping)

## Pages

| Page | URL | Notes |
|---|---|---|
| Landing | https://liftyx.app/ | Marketing URL; App Store badge and Smart App Banner |
| Privacy policy | https://liftyx.app/privacy.html | Required by App Store Connect; runs no scripts |
| Support | https://liftyx.app/support.html | Required by App Store Connect |

`https://arekko.github.io/...` redirects to the custom domain, so links in
builds that have already shipped keep working.

## Repository layout

| Path | Purpose |
|---|---|
| `index.html` | Landing page |
| `privacy.html` | Privacy policy |
| `support.html` | Support page |
| `styles.css` | Shared styles (light and dark) |
| `favicon.ico`, `assets/icon-*.png` | App icon and favicons for browsers and search results |
| `assets/og-image-iphone.png` | Open Graph and Twitter share image |
| `assets/app-store-badge-{black,white}.svg` | Official Apple "Download on the App Store" badge (light and dark) |
| `robots.txt`, `sitemap.xml` | Crawler rules and the sitemap of the three pages |
| `CNAME` | Custom domain (`liftyx.app`). **Never omit it from a deploy.** |
| `README.md` | This file, kept out of search results by `robots.txt` |

## Local preview

The pages are static, so any local web server works:

```sh
python3 -m http.server 8000
```

Then open http://localhost:8000/. TelemetryDeck tags localhost visits as test
signals, so they don't count as real traffic (see [Web analytics](#web-analytics)).

## Deploying

GitHub Pages publishes `main` automatically, usually within a minute.

`git subtree push` doesn't work because this repository has commits of its own
(notably `CNAME`). From the private app repository, mirror `docs/` instead:

```sh
rsync -a --delete --exclude .git docs/ ../arekko.github.io/
cd ../arekko.github.io
git add -A
git commit -m "docs: update site"
git push
```

`--delete` is safe because this repository holds nothing except a copy of
`docs/`. After the push, check https://liftyx.app/.

Before you deploy:

- [ ] `CNAME` is present.
- [ ] Analytics changes and the matching update to `privacy.html` §11 ("This
  website") ship in the same deploy.
- [ ] The App Store links still agree (see [App Store links](#app-store-links)).

## App Store links

App ID: `6827282064`.

| Where | Link | Why |
|---|---|---|
| `index.html` badge | `https://apps.apple.com/app/apple-store/id6827282064?pt=129602377&ct=site-home&mt=8` | `site-home` campaign link, so App Store Connect attributes downloads to the site |
| `installUrl` (JSON-LD) | `https://apps.apple.com/app/liftyx/id6827282064` | Canonical link. Keep campaign parameters out of it, or search traffic is counted as site traffic. |
| `apple-itunes-app` meta tag | `app-id=6827282064` | Smart App Banner in Safari |

In HTML attributes, write `&` in the campaign link as `&amp;`.

## Web analytics

`index.html` and `support.html` load TelemetryDeck's Web SDK from
`cdn.telemetrydeck.com`. The SDK is cookieless and anonymous.

- **Page views:** `data-page-engagement="false"` means each visit sends one
  `pageview` signal (path, referrer, UTM parameters, country, browser and
  device).
- **Badge clicks:** clicking the App Store badge on `index.html` sends one
  `Actions.AppStoreLink.clicked` signal to TelemetryDeck's v3 web endpoint, with
  the same cookieless visitor hash.
- **Privacy page:** `privacy.html` is deliberately excluded and runs no scripts.

Rules:

- The site reports to its own TelemetryDeck app, **"Liftyx Website"**, in the
  same account as the iOS app. **Do not reuse the iOS app ID**, because website
  traffic would distort the app's DAU and retention numbers.
- The site app ID lives in each page's `data-app-id` attribute. It's a public
  client identifier that only allows sending signals, so it isn't a secret, but
  keep it out of this README. The inline click script reads it from the existing
  script tag, so don't duplicate it.
- Web events count against the account's monthly quota, which the app shares.
  Page engagement is off to halve usage; turn it on per page only if the quota
  allows.
- Watch localhost test signals in TelemetryDeck **Test Mode** → *Recent
  Signals*.

## SEO

- `index.html` carries the canonical URL, Open Graph and Twitter tags, and
  `MobileApplication` JSON-LD. `support.html` and `privacy.html` carry canonical
  URLs and Open Graph tags.
- `robots.txt` allows everything except `/README.md` and `/README.html`, and
  points to `sitemap.xml`.
- Favicons are `/favicon.ico` (16, 32 and 48 px) plus 48, 96, 192 and 512 px
  PNGs, declared on every page. Google caches favicons separately from page
  content. After you change one, use Search Console's URL Inspection → *Request
  indexing* and allow days to weeks. Brave usually refreshes sooner; if it
  doesn't, resubmit at https://search.brave.com/submit-url.
- Verify `liftyx.app` in Google Search Console (DNS TXT record or HTML file) and
  submit `https://liftyx.app/sitemap.xml`. Repeat both in Bing Webmaster Tools.

## App Store Connect field mapping

| App Store Connect field | Value |
|---|---|
| Privacy Policy URL | `https://liftyx.app/privacy.html` |
| Support URL | `https://liftyx.app/support.html` |
| Marketing URL (optional) | `https://liftyx.app/` |
