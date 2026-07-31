# DEPLOY

Everything is built and committed. The only step left needs your GitHub account.

## Why the history is in a bundle

The folder this was built in is a mounted share that does not permit deleting files. Git needs to
delete its own `.git/*.lock` files between commits, so it could only make one commit here before
jamming. The full six-commit history was therefore built in the sandbox and exported as
`git-history.bundle` — a standard Git format that carries complete history.

The `.git` folder sitting in this directory is the jammed one-commit attempt. **Ignore it and use the
bundle**, which has everything.

## Get the repo onto your machine

```bash
git clone git-history.bundle bill-marriott-cuoc-doi
cd bill-marriott-cuoc-doi
git remote remove origin          # points at the bundle file; remove it
```

You now have the project with all six commits:

```
docs: README, image provenance and build log
feat: build the fifteen-section longform page
feat: Vietnamese-first design system and original vector assets
content: timeline, brands, quotes and stats as editable JSON
docs: numbered bibliography with source-conflict table
chore: scaffold repo, licence and static-hosting config
```

## Push to GitHub and turn on Pages

1. Create a new **public, empty** repo named `bill-marriott-cuoc-doi` (no README, no .gitignore,
   no licence — the history already has them).

2. Push:

```bash
git remote add origin https://github.com/<your-username>/bill-marriott-cuoc-doi.git
git push -u origin main
```

3. **Settings → Pages → Source: Deploy from a branch → Branch: `main`, folder: `/ (root)` → Save.**

No GitHub Actions workflow is needed — there is no build step. `.nojekyll` is already committed so
Jekyll won't interfere with the `assets/` folder.

4. Wait a minute, then open `https://<your-username>.github.io/bill-marriott-cuoc-doi/`.

5. Once live, update the placeholder URL in three files and push again:

| File | What to change |
|---|---|
| `index.html` | `<link rel="canonical">` and both `og:image` / `twitter:image` if you want absolute URLs |
| `sitemap.xml` | `<loc>` |
| `robots.txt` | `Sitemap:` |

## Viewing it before you deploy

**`review-build.html`** in this folder is a single self-contained file with the CSS, JavaScript and all
JSON inlined. Double-click it — it opens straight in a browser with no server. Use it for a quick look.

For the real thing, serve the folder:

```bash
python3 -m http.server 8000
# http://localhost:8000
```

A server is required for `index.html` because it loads `data/*.json` via `fetch()`, which browsers
block on `file://` URLs.

## After it's live — worth doing

- Run Lighthouse on the deployed URL (see BUILD-LOG §5 — scores were not measured here, no browser
  was available in the build environment).
- Open the timeline on an actual phone. Horizontal timelines are the most common mobile failure point.
- Consider self-hosting the two fonts to drop the Google Fonts request.
