# aspiredoctor.com

The marketing site for **Aspire**, the pre-med planner. A static, dependency-free
replacement for the old Framer site: one page, one stylesheet, a handful of
images. No build step, no framework, nothing to keep up to date.

This repo holds **only** the marketing site. Three other things live elsewhere:

| | Where |
|---|---|
| The Flutter app (iOS, Android, web) | `Aspire-premed-app/main-app` |
| The web app, `app.aspiredoctor.com` | deployed from that repo to Vercel |
| The iOS app | [App Store](https://apps.apple.com/us/app/aspire-the-premed-app/id6744653365) |

    index.html        the whole page
    styles.css        the whole design
    img/              screenshots, icons, OG image
    CNAME             tells GitHub Pages the custom domain
    robots.txt, sitemap.xml
    .nojekyll         skip Jekyll processing

## Local preview

```bash
python3 -m http.server 4173 --directory .
```

Then open http://localhost:4173.

## Deploying

`.github/workflows/deploy.yml` publishes the repo root to GitHub Pages on every
push to `main`. It can also be run by hand from the Actions tab (Run workflow).

One-time setup in the repo settings:

1. **Settings → Pages → Build and deployment → Source: GitHub Actions.**
2. **Settings → Pages → Custom domain:** `aspiredoctor.com`, then save.
3. Leave **Enforce HTTPS** unchecked until GitHub finishes issuing the
   certificate, then turn it on.

The repo must stay public for Pages to work on the free plan.

## DNS

The domain is on Cloudflare (`lia` / `pedro.ns.cloudflare.com`). The apex and
`www` still point at Framer and need repointing; **leave the `app` record
alone** — that is the live web app on Vercel.

| Type  | Name  | Value                          | Proxy  |
|-------|-------|--------------------------------|--------|
| A     | `@`   | `185.199.108.153`              | DNS only |
| A     | `@`   | `185.199.109.153`              | DNS only |
| A     | `@`   | `185.199.110.153`              | DNS only |
| A     | `@`   | `185.199.111.153`              | DNS only |
| CNAME | `www` | `Aspire-premed-app.github.io`  | DNS only |
| CNAME | `app` | *unchanged — points to Vercel* | — |

Records to delete first: the two apex A records at `31.43.161.6` and
`31.43.160.6`, and the `www` CNAME to `sites.framer.app`.

Set the new records to **DNS only** (grey cloud) so GitHub can validate the
domain and issue a certificate. Once HTTPS is working you can switch the proxy
back on, but only with Cloudflare SSL mode set to **Full (strict)** — the
default Flexible mode will send the site into a redirect loop.

Propagation is usually minutes; GitHub's certificate can take up to an hour.

## Editing the copy

Plain HTML — edit `index.html` directly. A few things are stated as fact and
were checked against the app's source when the site was written. If the app
changes, re-check them here:

- the five categories and their default targets (300 / 300 / 400 / 50 / 500),
  which come from `lib/app/core/services/firestore_category_settings.dart` in
  the app repo, where they are per-user editable
- "iOS 13 or later", from the App Store listing
- sign-in methods: Apple, Google, email
- the feature list deliberately omits **Schedule** and **Prerequisites**, which
  are archived in the app (see `guides/ARCHIVED_FEATURES.md` there)

## Screenshots

`img/*.webp` were cropped from the live App Store screenshots, with the
marketing headline trimmed off so only the device remains.

The home-dashboard screenshot from that set was deliberately left out: it still
shows a **Schedule** tile for an archived feature. Worth refreshing the App
Store screenshots at some point, and the crops here along with them.
