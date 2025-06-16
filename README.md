# Automating Repository Visibility on GitHub

Sometimes we want to keep our project private while working on it, and only make it public later — maybe when it's ready, or after a deadline. In this guide, I explain how to **automate the process of changing a private GitHub repository to public** on a specific date using **GitHub Actions**.

I used this method when I was preparing a project for an application and didn’t want to release it too early. So instead of setting a reminder or doing it manually, I automated the process.

---

## What This Repository Contains

This repo does **not** run the automation itself — it's just a **template and explanation** you can reuse. You’ll find:
- An example workflow file (`make-public.yml`)
- A full step-by-step guide (below)
- Notes and tips based on my experience

---

## What You’ll Need

Before you start, make sure:
- You have a **private repository** you want to make public later
- You can **create a Personal Access Token (PAT)** on GitHub
- You know how to navigate to your repository’s **Settings** → **Secrets and variables**

---

## Step-by-Step Guide

### **Step 1 – Generate a Personal Access Token**

1. Go to: [https://github.com/settings/tokens](https://github.com/settings/tokens)
2. Click **“Generate new token (classic)”**
3. Give it a name like `repo-public-automation`
4. Set an expiration date (e.g., 90 days)
5. Under **Scopes**, only select:  
    `repo`
6. Click **Generate token** and **copy it immediately** — you won't see it again!

---

### **Step 2 – Add Your Token to GitHub Secrets**

1. Go to your **private repository** (the one you want to make public)
2. Click on **Settings**
3. Navigate to **Secrets and variables → Actions**
4. Click **New repository secret**
   - **Name:** `GH_TOKEN`
   - **Value:** Paste the token you just copied
5. Save the secret

---

### **Step 3 – Add the Workflow File**

In your private repository, create a file at this path:  
`.github/workflows/make-public.yml`

Paste the following content inside:

```yaml
name: Make Repository Public

on:
  schedule:
    - cron: '0 8 30 6 *'  # This runs at 08:00 UTC on June 30
#   workflow_dispatch:       # Optional: you can also trigger it manually

jobs:
  make-public:
    runs-on: ubuntu-latest
    steps:
      - name: Make the repo public
        run: gh repo edit $REPO_NAME --visibility=public
        env:
          GH_TOKEN: ${{ secrets.GH_TOKEN }}
          REPO_NAME: your-username/your-private-repo
```

---

### **Step 4 – That’s It!**

Once this workflow file is added to your private repo:
- It will **automatically run on the date you set in the `cron` line**
- The repository will become **public without you needing to do anything manually**
- You’ll also have the option to run it anytime using the **“Run workflow”** button on GitHub

This is super helpful when you're preparing for submissions, applications, or simply want to delay the release until you’re ready.

---

## Customizing the Schedule (CRON)

The line below controls **when** the workflow runs:

```yaml
cron: '0 8 30 6 *'
```

This means: **at 08:00 UTC on June 30**.

You can modify the schedule to fit your needs. Here are some examples:

| CRON Expression     | Runs At                    |
|---------------------|----------------------------|
| `0 8 30 6 *`        | June 30 at 08:00 UTC       |
| `0 0 * * 1`         | Every Monday at 00:00 UTC  |
| `0 12 * * 5`        | Every Friday at 12:00 UTC  |
| `0 9 * * *`         | Every day at 09:00 UTC     |

🔗 Use [crontab.guru](https://crontab.guru) to test and understand different CRON expressions.

---

## A Few Extra Tips

- The workflow uses **GitHub CLI (`gh`)** — no need to install it manually; it's already available in GitHub Actions runners.
- Make sure your **Personal Access Token (`GH_TOKEN`)** is still active and has the correct `repo` permission.
- Replace the following line:

```yaml
REPO_NAME: your-username/your-private-repo
```

with your actual repository name.


---

## Why I Shared This

I used this method when I was preparing a project for an application and needed to keep it private until a certain date. Instead of manually making it public later — or risking forgetting it — I decided to automate the process.

It worked really well for me, and I thought it might help others too — especially students, developers, and researchers managing sensitive or timed releases.

---

## 🧠 Summary

- ✅ Create a Personal Access Token
- ✅ Save it in your repo as a secret (`GH_TOKEN`)
- ✅ Add the GitHub Actions workflow
- 🕒 Let the automation handle the visibility change

No stress, no reminders — just clean automation.

---
