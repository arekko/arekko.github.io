# Liftyx website (landing + privacy policy + support)

Static pages used for the App Store **Privacy Policy URL** and **Support URL**.
No build step, no JavaScript, no external assets, no cookies.

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
| `assets/icon-*.png` | App icon used on the pages |
| `CNAME` | Keeps the custom domain configured; never omit it from a deploy |

## URLs

| Page | URL |
|---|---|
| Landing | https://liftyx.app/ |
| Privacy policy | https://liftyx.app/privacy.html |
| Support | https://liftyx.app/support.html |

`https://arekko.github.io/...` redirects to the custom domain, so links in
already-shipped builds keep working.

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
- [ ] Replace the "Coming soon to the App Store" note on `index.html` with an
  App Store link or badge once the app is live.

## SEO

- `index.html` carries the canonical URL, Open Graph/Twitter tags, and
  `MobileApplication` JSON-LD; `support.html` and `privacy.html` carry canonical
  and Open Graph tags.
- `robots.txt` and `sitemap.xml` ship with the site; the sitemap lists the three
  pages.
- Verify `liftyx.app` in Google Search Console (DNS TXT or HTML file) and submit
  `https://liftyx.app/sitemap.xml`; repeat in Bing Webmaster Tools.
- After launch, add the App Store link and the `apple-itunes-app` meta tag with
  the numeric app ID.
- Keep `/README.md` out of search results via the robots disallow.

## App Store Connect field mapping

| ASC field | Value |
|---|---|
| Privacy Policy URL | `https://liftyx.app/privacy.html` |
| Support URL | `https://liftyx.app/support.html` |
| Marketing URL (optional) | `https://liftyx.app/` |
