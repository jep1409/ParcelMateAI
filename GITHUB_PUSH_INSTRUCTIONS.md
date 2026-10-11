# GitHub Push Instructions

Your local repo is ready to push. Follow these steps to make it **PUBLIC** (required by the competition).

## 1. Create the GitHub repo (PUBLIC)

1. Go to https://github.com/new
2. **Repository name:** `parcelmate-ai`
3. **Description:** "Smart campus parcel management — Hackerdorm Track 2 by 404 Not Founders"
4. **Public** ← MUST be public
5. **Do NOT** initialize with README, .gitignore, or license (we already have them)
6. Click **Create repository**

## 2. Push the code

Copy the commands from GitHub's "push an existing repository" section. They will look like:

```bash
cd "C:\Users\user\WorkBuddy AI\2026-10-10-21-50-51\parcelmate-ai"
git remote add origin https://github.com/<YOUR-GITHUB-USERNAME>/parcelmate-ai.git
git branch -M main
git push -u origin main
```

Replace `<YOUR-GITHUB-USERNAME>` with your actual GitHub username.

If GitHub asks you to authenticate, use a **Personal Access Token** (PAT) as the password — not your GitHub account password:
- Create one at: https://github.com/settings/tokens/new
- Scopes needed: `repo` (full control of private repositories)

## 3. Verify it's PUBLIC

After pushing, visit:
`https://github.com/<YOUR-GITHUB-USERNAME>/parcelmate-ai`

You should be able to access it **without logging in** — that's how you'll know it's public.

## 4. Copy the link

The link to enter in the submission form is:
```
https://github.com/<YOUR-GITHUB-USERNAME>/parcelmate-ai
```

---

## Quick troubleshooting

**"Permission denied" error:** Your PAT may have expired or have the wrong scopes. Create a new one with `repo` scope.

**"Repository not found" error:** Check the URL — the username and repo name must match exactly.

**Need to verify the remote later:**
```bash
git remote -v
```

**Need to check what's committed:**
```bash
git log --oneline
```

---

**Already done?** Move on to the deployment step and PDF generation.