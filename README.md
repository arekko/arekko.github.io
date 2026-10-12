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

## URLs

| Page | URL |
|---|---|
| Landing | https://arekko.github.io/ |
| Privacy policy | https://arekko.github.io/privacy.html |
| Support | https://arekko.github.io/support.html |

## Deploying

GitHub publishes user-site repositories automatically from the default branch
(`main`), so there is nothing to configure in Settings. After editing the pages
here, push `docs/` to the site repository:

```sh
git subtree push --prefix docs git@github.com:arekko/arekko.github.io.git main
```

Deployment takes about a minute; then check https://arekko.github.io/.

## Before submission — replace these placeholders

- `support@liftyx.app` → your real support email (appears in `index.html`,
  `privacy.html`, and `support.html`).
- If you add a custom domain later, update the privacy policy URL in
  `gym_tracker/views/pages/settings/SettingsPage.swift` and the tables here.
- "Coming soon to the App Store" note on `index.html` → replace with an App
  Store link or badge once the app is live.

## App Store Connect field mapping

| ASC field | Value |
|---|---|
| Privacy Policy URL | `https://arekko.github.io/privacy.html` |
| Support URL | `https://arekko.github.io/support.html` |
| Marketing URL (optional) | `https://arekko.github.io/` |
