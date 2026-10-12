# Liftyx website (landing + privacy policy + support)

Static pages used for the App Store **Privacy Policy URL** and **Support URL**.
No build step, no JavaScript, no external assets, no cookies.

## Files

| File | Purpose |
|---|---|
| `index.html` | Landing page (marketing URL, optional) |
| `privacy.html` | Privacy policy — required by App Store Connect |
| `support.html` | Support page — required by App Store Connect |
| `styles.css` | Shared styling (light/dark) |
| `assets/icon-*.png` | App icon used on the pages |

## Deploying with GitHub Pages

1. Push this `docs/` folder to `main`.
2. On GitHub: **Settings → Pages → Source: Deploy from a branch**,
   branch `main`, folder `/docs`, then **Save**.
3. Wait a minute, then check:
   - `https://arekko.github.io/gyloo/`
   - `https://arekko.github.io/gyloo/privacy.html`
   - `https://arekko.github.io/gyloo/support.html`

> Note: GitHub Pages on **private** repositories requires a paid GitHub plan.
> If the repository is private and Pages is unavailable, either make the repo
> public (it contains no secrets — analytics IDs are public by design) or host
> this folder on any static host (Cloudflare Pages, Netlify, Vercel) and use
> that URL instead.

## Before submission — replace these placeholders

- `support@liftyx.app` → your real support email (appears in `index.html`,
  `privacy.html`, `support.html`, and in `SettingsPage.swift` is unaffected).
- If the repository name changes or you add a custom domain, update the privacy
  policy URL in `gym_tracker/views/pages/settings/SettingsPage.swift`.
- "Coming soon to the App Store" note on `index.html` → replace with an App
  Store link or badge once the app is live.

## App Store Connect field mapping

| ASC field | Value |
|---|---|
| Privacy Policy URL | `https://arekko.github.io/gyloo/privacy.html` |
| Support URL | `https://arekko.github.io/gyloo/support.html` |
| Marketing URL (optional) | `https://arekko.github.io/gyloo/` |
