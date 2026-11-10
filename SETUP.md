# Setup, verification and troubleshooting

## 1. Create the repository
The repo name must exactly match your username: `playingkinggame/playingkinggame`, public, with a `README.md` on `main`.

## 2. Upload (run inside the unzipped folder)
```bash
git init -b main
git add .
git commit -m "feat: add animated profile README"
git remote add origin https://github.com/playingkinggame/playingkinggame.git
git push -u origin main
```
If the repo already has commits: `git pull origin main --rebase` before pushing.

## 3. Permissions and secrets
- **Snake (required):** Settings → Actions → General → Workflow permissions → *Read and write permissions* → Save. No secret needed; the built-in `GITHUB_TOKEN` is enough.
- **Summary cards:** uses the built-in `GITHUB_TOKEN`. If the run fails with "Resource not accessible by integration", create a PAT (`read:user`, `repo`), save it as a secret named `SUMMARY_GITHUB_TOKEN`, and use that name in `.github/workflows/profile-summary-cards.yml`.
- The streak card and visitor counter need no keys.

## 4. Run the workflows once
Actions tab → *Generate contribution snake* → Run workflow. It creates the `output` branch containing `github-snake.svg` and `github-snake-dark.svg`. Then run **Profile summary cards**. It commits a `profile-summary-card-output/` folder to `main`; the activity cards in the README read from it.

## 5. Verify
1. Open github.com/playingkinggame in a private window and hard-refresh.
2. The banner, typing line, terminal and separators are local SVGs; they should animate immediately. Check the `assets/` links resolve in the repo.
3. Open `https://raw.githubusercontent.com/playingkinggame/playingkinggame/output/github-snake-dark.svg` directly. It should show the snake.
4. Open any card URL from the README in a browser tab. If it renders there, it will render on the profile.
5. Switch GitHub's appearance between dark and light to check the snake variants.

## 6. Troubleshooting
| Symptom | Fix |
|---|---|
| Snake shows alt text | Workflow hasn't run, or write permission is off (step 3). Run it and check the log. |
| Snake workflow fails on push | Confirm *Read and write permissions*; check that `output` isn't a protected branch. |
| Stats card says "rate limit" or shows an error | The public Vercel instance is busy. Retry later, or self-host: fork anuraghazra/github-readme-stats, deploy to Vercel with a `PAT_1` env var, and replace the domain in the README. |
| Streak card blank | Retry; the service caches. Self-hosting is possible via DenverCoder1/github-readme-streak-stats. |
| Stats show 0 or low numbers | Expected for a new account. Private contributions appear only if enabled in profile settings and PAT-backed. |
| Visitor counter stuck at 1 | It counts per image request and caches; it grows over time. |
| Animations don't move | Some mobile apps and email previews render SVGs statically. Browsers on github.com support them. |
| Edits don't appear | GitHub proxies images through camo and caches them. Hard-refresh; add `&v=2` to a card URL to bust the cache. |
| Metrics workflow fails | Check `METRICS_TOKEN` exists and has the scopes above. |

## 7. Editing
- Search `TODO` in `README.md` for every placeholder (project links, status, LinkedIn, portfolio, email).
- Change tech icons by editing the `i=` list (names at skillicons.dev).
- Colors used everywhere: `#0a0e1a` background, `#a855f7` purple, `#22d3ee` cyan.
