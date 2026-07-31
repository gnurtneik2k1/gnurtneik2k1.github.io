# PUBLISH — getting this live

Everything is ready. A GitHub Actions workflow is already committed, so **Pages turns itself on and
deploys on the first push** — nobody has to touch Settings → Pages.

Pick whichever route matches what you have open.

---

## Route A — Git Bash (fastest, keeps the 7-commit history)

You have Git Bash installed. Open it, then paste this **one block at a time**.

```bash
cd "$APPDATA/Claude/local-agent-mode-sessions/f373f732-2422-42a5-87dc-424dbd979af5/d985d0f5-9d53-404f-b1a9-5cc6cc7578e0/local_c59eba3b-91f4-433c-88a9-d2bc3aa3e51e/outputs"

git clone bill-marriott-cuoc-doi/git-history.bundle ~/bill-marriott-cuoc-doi
cd ~/bill-marriott-cuoc-doi
git remote remove origin
```

Now create an **empty public repo** on github.com named `bill-marriott-cuoc-doi` —
no README, no .gitignore, no licence. Then:

```bash
git remote add origin https://github.com/<your-username>/bill-marriott-cuoc-doi.git
git push -u origin main
```

Git Bash will pop a browser window to authorise. That's the only sign-in step, and it's handled by
GitHub's own credential helper — no token to copy anywhere.

---

## Route B — GitHub Desktop (all GUI, no commands)

1. **GitHub Desktop → File → New repository**
   - Name: `bill-marriott-cuoc-doi`
   - Local path: anywhere you like (Documents is fine)
   - Leave README / .gitignore / licence blank
   - **Create repository**

2. Open the new folder it made, and **copy everything from this project folder into it** —
   `index.html`, `assets/`, `data/`, `.github/`, and all the `.md` files.
   Skip `git-history.bundle`, `review-build.html`, and the `.git` folder.

   > Windows hides folders starting with a dot. In File Explorer turn on
   > **View → Show → Hidden items** so you can see `.github` and `.nojekyll`.

3. Back in GitHub Desktop it will list all the files as changes.
   Summary: `feat: Bill Marriott biographical site` → **Commit to main**

4. **Publish repository** → **untick "Keep this code private"** → Publish.

This route gives you one commit instead of seven. Everything else is identical.

---

## Then

Watch the **Actions** tab on the repo. The `Deploy to GitHub Pages` workflow runs in about a minute
and prints the live URL. It'll be:

```
https://<your-username>.github.io/bill-marriott-cuoc-doi/
```

Once it's live, update the placeholder URL in three places and push again:

| File | Line |
|---|---|
| `index.html` | `<link rel="canonical" ...>` |
| `sitemap.xml` | `<loc>` |
| `robots.txt` | `Sitemap:` |

---

## Why I couldn't do this part myself

Three routes were available and all three were closed:

- **No GitHub connector** exists in the MCP registry — I checked.
- **The Claude in Chrome extension isn't connected**, so I can't drive github.com in the browser.
  (Install: https://chromewebstore.google.com/detail/fcoeoabgfenejglbffodgkkbkcdhcgfn — then sign into
  the side panel with this same account, and I can do Route B end to end.)
- **The desktop-control approval dialog timed out twice** with no response, so I never got control of
  GitHub Desktop.

Separately, and regardless of the above: I don't handle passwords or access tokens. Even with full
desktop control I'd have driven an already-signed-in GitHub Desktop rather than typing a credential.
The sign-in step is yours either way.
