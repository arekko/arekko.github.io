# Liftyx website (landing + privacy policy + support)

Static pages used for the App Store **Privacy Policy URL** and **Support URL**.
No build step, no cookies. The landing and support pages load one external
script: the TelemetryDeck web analytics beacon (cookieless, anonymous).

The source of truth lives here, in `docs/`, in the **private** app repository.
The site is published from a separate **public** repository,
[`arekko/arekko.github.io`](https://github.com/arekko/arekko.github.io)
(a GitHub *user site*, served at the account root).

## Files

| File | Purpose |
|---|---|
| `index.html` | Landing page (marketing URL, optional) |
| `privacy.html` | Privacy policy — required by App Store Connect |
| `support.html` | Support page — required by App Store Connect |
| `styles.css` | Shared styling (light/dark) |
| `favicon.ico`, `assets/icon-*.png` | App icon and favicon used by browsers and search results |
| `assets/app-store-badge-{black,white}.svg` | Official Apple "Download on the App Store" badge (light/dark) |
| `CNAME` | Keeps the custom domain configured; never omit it from a deploy |

## URLs

| Page | URL |
|---|---|
| Landing | https://liftyx.app/ |
| Privacy policy | https://liftyx.app/privacy.html |
| Support | https://liftyx.app/support.html |

`https://arekko.github.io/...` redirects to the custom domain, so links in
already-shipped builds keep working.

## Web analytics

`index.html` and `support.html` load TelemetryDeck's Web SDK from
`cdn.telemetrydeck.com` with `data-page-engagement="false"`, so each visit sends
a single `pageview` signal (path, referrer, UTM parameters, country, browser and
device). Clicking the App Store badge on `index.html` sends one
`Actions.AppStoreLink.clicked` signal to TelemetryDeck's v3 web endpoint, using
the same cookieless visitor hash. `privacy.html` is deliberately excluded: the
policy page runs nothing.

- The site reports to its own TelemetryDeck app, **"Liftyx Website"**, in the
  same account as the iOS app. Do NOT reuse the iOS app ID — website traffic
  would pollute the app's DAU/retention.
- The site app ID lives in each page's `data-app-id` attribute. It is a public
  client identifier — it only permits sending signals, not a secret — but keep
  it out of this README, which ships to the public site repository. The inline
  click script reads it from the existing script tag; do not duplicate it.
- Web events are counted against the account's monthly quota, shared with the
  app. Page engagement is off to halve the burn; enable it per page only if
  there is headroom.
- Localhost visits are tagged as test signals automatically; watch them in
  TelemetryDeck **Test Mode** → *Recent Signals*.
- The beacon and `privacy.html` §11 must ship in the same deploy.

## Deploying

GitHub publishes the site repository automatically from `main`.

`git subtree push` is no longer usable because the site repository has commits
of its own (notably `CNAME`). Mirror the files instead:

```sh
rsync -a --delete --exclude .git docs/ ../arekko.github.io/
cd ../arekko.github.io
git add -A
git commit -m "docs: update site"
git push
```

Deployment takes about a minute; then check https://liftyx.app/. The
`--delete` is safe because the site repo holds nothing except a copy of `docs/`.

## Before submission

- [x] Custom domain and support email configured.
- [x] App Store link and badge on `index.html`; the `apple-itunes-app` Smart App
  Banner meta tag carries app ID `6827282064`.
- [x] TelemetryDeck website App ID set in `index.html` and `support.html`.

## SEO

- `index.html` carries the canonical URL, Open Graph/Twitter tags, and
  `MobileApplication` JSON-LD; `support.html` and `privacy.html` carry canonical
  and Open Graph tags.
- `robots.txt` and `sitemap.xml` ship with the site; the sitemap lists the three
  pages.
- Favicons: `/favicon.ico` (16/32/48) plus 48/96/192/512 px PNGs, declared on
  every page. Google caches favicons separately from page content, so after a
  change use Search Console's URL Inspection → Request indexing and allow days
  to weeks. Brave usually refreshes sooner; resubmit via
  `https://search.brave.com/submit-url` if it doesn't.
- Verify `liftyx.app` in Google Search Console (DNS TXT or HTML file) and submit
  `https://liftyx.app/sitemap.xml`; repeat in Bing Webmaster Tools.
- The App Store link is `https://apps.apple.com/app/liftyx/id6827282064`; keep
  it in sync with the `apple-itunes-app` meta tag and `installUrl` JSON-LD.
  The home page badge uses the `site-home` campaign link
  `https://apps.apple.com/app/apple-store/id6827282064?pt=129602377&ct=site-home&mt=8`
  so App Store Connect attributes downloads to the site; keep `installUrl`
  canonical (no campaign parameters).
- Keep `/README.md` out of search results via the robots disallow.

## App Store Connect field mapping

| ASC field | Value |
|---|---|
| Privacy Policy URL | `https://liftyx.app/privacy.html` |
| Support URL | `https://liftyx.app/support.html` |
| Marketing URL (optional) | `https://liftyx.app/` |
