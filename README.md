# GitHub Repo Visibility Automation

This project provides a clean example of how to **automate making a private GitHub repository public** on a scheduled date using GitHub Actions and a Personal Access Token (PAT).

---

## 🔧 How It Works

1. **Create a Personal Access Token (PAT)**  
   - Go to: https://github.com/settings/tokens  
   - Check only the `repo` scope  
   - Copy the token after generating it

2. **Add the token as a secret** in your private repository:
   - Go to your repo → Settings → Secrets and variables → Actions → New repository secret
   - Name it: `GH_TOKEN`

3. **Create a `.github/workflows/make-public.yml` file** in that private repository with this content:

```yaml
name: Make Repository Public

on:
  schedule:
    - cron: '0 8 30 6 *'  # 8:00 UTC on June 30
  workflow_dispatch:

jobs:
  make-public:
    runs-on: ubuntu-latest
    steps:
      - name: Make the repo public
        run: gh repo edit $REPO_NAME --visibility=public
        env:
          GH_TOKEN: ${{ secrets.GH_TOKEN }}
          REPO_NAME: your-username/your-private-repo
