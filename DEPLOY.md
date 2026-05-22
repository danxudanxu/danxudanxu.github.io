# Deploy danxu.info to GitHub Pages

## Prerequisites

- [Hugo](https://gohugo.io/installation/) installed on your Mac: `brew install hugo`
- [GitHub Desktop](https://desktop.github.com) installed (you already have this)
- Your GitHub account (danxudanxu)

## Step 1 — Replace the placeholder photo

Replace `static/picture.jpeg` with your LinkedIn headshot.
- Rename your photo to `picture.jpeg`
- Drop it into the `static/` folder, overwriting the placeholder
- Recommended: crop to square, ~400x400px minimum

## Step 2 — Create the GitHub repo

1. Go to https://github.com/new
2. Repository name: `danxudanxu.github.io`
   - IMPORTANT: this exact name enables GitHub Pages at the root URL
3. Set to **Public**
4. Do NOT initialize with README, .gitignore, or license
5. Click "Create repository"

## Step 3 — Push the site to GitHub

Open Terminal and run:

```bash
cd ~/Documents/Academic/PhD\ Preping/danxu-website

# Remove the template's git history and start fresh
rm -rf .git
git init
git add .
git commit -m "Initial site build"

# Connect to your new repo
git remote add origin https://github.com/danxudanxu/danxudanxu.github.io.git
git branch -M main
git push -u origin main
```

## Step 4 — Enable GitHub Pages with Hugo workflow

1. Go to https://github.com/danxudanxu/danxudanxu.github.io/settings/pages
2. Under "Build and deployment" → Source → select **GitHub Actions**
3. GitHub will suggest "Hugo" — click **Configure** on the Hugo workflow
4. The workflow file (`.github/workflows/hugo.yml`) is already included in this repo. If GitHub asks you to create one, just use the one already in the repo.
5. Click "Commit changes" if prompted

The workflow will run automatically. Wait 2-3 minutes.

## Step 5 — Verify

Visit https://danxudanxu.github.io/ — your site should be live.

Check:
- [ ] Photo displays correctly
- [ ] Subtitle text is correct
- [ ] "Papers" button leads to paper list
- [ ] "CV" button downloads cv.pdf
- [ ] Both paper pages load with abstracts
- [ ] Social icons (CV, Email, GitHub, LinkedIn) work
- [ ] Dark teal link color appears

## Step 6 — Connect danxu.info (after domain transfer, ~late July 2026)

Once danxu.info is at Porkbun/Cloudflare:

### At your registrar (Porkbun or Cloudflare):
Add these DNS records:

| Type | Name | Value |
|---|---|---|
| CNAME | www | danxudanxu.github.io |
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |

### At GitHub:
1. Go to https://github.com/danxudanxu/danxudanxu.github.io/settings/pages
2. Under "Custom domain" → enter `danxu.info` → Save
3. Check "Enforce HTTPS" once DNS propagates (can take up to 24 hours)

### Update config.yml:
Change `baseURL` from `https://danxudanxu.github.io/` to `https://danxu.info/`
Commit and push. The site now serves from danxu.info.

## Step 7 — Set up dan@danxu.info email routing

At your registrar:
- **Cloudflare**: Dashboard → danxu.info → Email → Email Routing → dan@danxu.info → forwards to danxu.econ@gmail.com
- **Porkbun**: Domain → Email Forwarding → dan@danxu.info → danxu.econ@gmail.com

Then update config.yml: change `mailto:dan@danxu.info` (already set) — no change needed.

## Future updates

To update the site:
1. Edit files locally (add papers, update CV, change text)
2. In Terminal:
```bash
cd ~/Documents/Academic/PhD\ Preping/danxu-website
hugo server
```
3. Preview at http://localhost:1313
4. When satisfied:
```bash
git add .
git commit -m "Update description"
git push
```
5. GitHub Actions rebuilds and deploys automatically (2-3 minutes)

## File reference

| File | What to edit |
|---|---|
| `config.yml` | Site title, subtitle, social links, menu items |
| `assets/css/core/theme-vars.css` | Color scheme (darkcolor / lightcolor hex codes) |
| `static/picture.jpeg` | Profile photo |
| `static/cv.pdf` | CV download |
| `content/papers/paper1/index.md` | Paper 1 page |
| `content/papers/paper2/index.md` | Paper 2 page |

## Adding a new paper

```bash
cd ~/Documents/Academic/PhD\ Preping/danxu-website
mkdir content/papers/paper3
```

Copy `content/papers/paper1/index.md` to `content/papers/paper3/index.md`, update the front matter (title, date, tags, description, summary) and body content. Commit and push.

## Troubleshooting

- **Site shows 404**: Check GitHub Pages settings → Source must be "GitHub Actions", not "Deploy from a branch"
- **CSS looks wrong**: Clear browser cache (Cmd+Shift+R)
- **Hugo build fails**: Run `hugo version` — must be v0.147+ for this template
- **Photo not showing**: Must be named exactly `picture.jpeg` in the `static/` folder
