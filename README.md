# Site offline notice

A short static placeholder page telling visitors the site is offline while the
host fixes a PHP security issue. It runs no PHP or JavaScript.

## Files

- `index.html` – the notice. The United Fitness logo is embedded in it as a data URI, so there is no separate image file to keep in sync.
- `404.html` – identical copy, so deep links (`/blog/post`, `/shop`) show the notice too. Keep it in sync with `index.html` (`cp index.html 404.html`).
- `.nojekyll` – tells GitHub Pages to serve the files as-is.

## Before publishing

Search `index.html` for `EDIT:` comments and fill in:

1. Contact email.
2. "Last updated" in the footer.

The page carries `<meta name="robots" content="noindex">` so search engines do not
index the notice in place of your real pages.

## Deploy to GitHub Pages

```sh
cd site-down
git init -b main
git add .
git commit -m "Outage notice"
git remote add origin git@github.com:<you>/site-down.git
git push -u origin main
```

Then on GitHub: **Settings → Pages → Build and deployment → Source: Deploy from a
branch**, branch `main`, folder `/ (root)`. The page is live at
`https://<you>.github.io/site-down/` within a minute or two.

## Point your domain at it

1. **Settings → Pages → Custom domain**: enter your domain (e.g. `example.com`) and
   save. GitHub adds a `CNAME` file to the repo.
2. At your DNS provider, point the apex at GitHub Pages:

   ```
   A     @    185.199.108.153
   A     @    185.199.109.153
   A     @    185.199.110.153
   A     @    185.199.111.153
   CNAME www  <you>.github.io
   ```

   Check GitHub's docs for the current IPs before relying on these.
3. Once the DNS check passes, tick **Enforce HTTPS**.

GitHub Pages cannot return a 503 status. If you can put a reverse proxy or CDN in
front instead, prefer serving this file with `503` and a `Retry-After` header.

## When the site is back

Point DNS back at your server, then delete or archive this repo.
