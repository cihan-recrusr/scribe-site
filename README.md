# Scribe site (privacy policy + support)

Static site for the two public URLs App Store Connect requires. Plain HTML + one CSS file,
no build step, no scripts, no external requests.

| File | Purpose |
|---|---|
| `index.html` | Landing page (optional as a store URL, but gives the host a root page) |
| `privacy.html` | Privacy Policy (English + Turkish) -> App Store Connect "Privacy Policy URL" |
| `support.html` | Support (English + Turkish) -> App Store Connect "Support URL" |
| `style.css`, `icon.png` | Shared styling and app icon (also the favicon) |

## Placeholders to replace before publishing

Every placeholder is rendered as a highlighted `[NAME]` (`<span class="placeholder">`) and
listed in a comment at the top of each HTML file. Find them all with
`grep -rn 'class="placeholder"' .`

| Placeholder | Where | Replace with |
|---|---|---|
| `<APP_STORE_URL>` | `index.html`, HTML comment above the "Coming soon" badge | Swap the badge for the download link once the app is live |

Replace the whole `<span class="placeholder">[...]</span>` element, not only the text, so the
highlight disappears. Keep `privacy.html` in step with `docs/app-store/privacy-policy.md`.

## Hosting option A: new public GitHub repo with GitHub Pages (recommended)

The Scribe repo is private and Pages is off, so use a separate public repo.
The GitHub account is `cihan-recrusr` (checked with `gh api user`).

1. Fill in the placeholders above.
2. Create the repo and push the *contents* of `site/` (so `index.html` is at the repo root):

   ```sh
   cp -R site /tmp/scribe-site && cd /tmp/scribe-site
   git init -b main && git add . && git commit -m "Initial site"
   gh repo create scribe-site --public --source=. --push
   ```
3. Turn on Pages from the root:

   ```sh
   gh api -X POST repos/cihan-recrusr/scribe-site/pages \
     -f 'source[branch]=main' -f 'source[path]=/'
   ```
   (or: GitHub -> repo -> Settings -> Pages -> Source "Deploy from a branch", branch `main`, folder `/ (root)`).
4. Wait a minute or two for the first build, then check the URLs.

Resulting URLs:

- Privacy Policy URL: `https://cihan-recrusr.github.io/scribe-site/privacy.html`
- Support URL: `https://cihan-recrusr.github.io/scribe-site/support.html`
- Home: `https://cihan-recrusr.github.io/scribe-site/`

Enter them in App Store Connect: App Information -> Privacy Policy URL, and Version -> Support URL.
All links in the site are relative, so the `/scribe-site/` sub-path works.

## Hosting option B: an existing domain

Upload the contents of `site/` to any static host or folder on the domain (for example
`https://example.com/scribe/`). Because links are relative, any sub-path works. Use
`https://example.com/scribe/privacy.html` and `.../support.html` as the two URLs. Both must be
reachable without login and over HTTPS.
