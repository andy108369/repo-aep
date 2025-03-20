# Repo AEP – Workflow Trigger for Repo Website

This repository contains a GitHub Actions workflow that triggers a **repository dispatch event** in [`repo-website`](https://github.com/andy108369/repo-website) whenever a push occurs on the `main` branch.

## 🚀 Workflow Overview

- **Trigger:** Push to `main`.
- **Action:** Sends a **repository dispatch** event to `repo-website` to trigger its workflow.
- **Failure Handling:** The workflow fails if the response is not `2xx`.

---

## 🔑 Setup: Fine-Grained PAT

To enable the workflow, create a **fine-grained Personal Access Token (PAT)** with the following **repository permissions** (for `repo-website` only):

- ✅ **Actions: Read & Write** (Trigger workflows)
- ✅ **Workflows: Read & Write** (Dispatch events)
- ✅ **Contents: Read & Write** (Required for repository dispatch)
- ✅ **Metadata: Read** (Mandatory)

### **Step 1: Generate the PAT**
1. Go to **GitHub → Settings → Developer settings → Personal Access Tokens → Fine-grained tokens**.
2. Click **"Generate new token"**.
3. Set **Repository access** to **repo-website only**.
4. Enable the permissions listed above.
5. Name it **`repo-website-trigger-pat`** and add this description:  
   - *"Allows repo-aep to trigger workflows in repo-website via repository dispatch."*
6. Set **Expiration** to **No expiration**
7. Click **Generate Token** and **copy it**.

### **Step 2: Add the PAT to `repo-aep`**
1. Go to **repo-aep → Settings → Secrets and variables → Actions**.
2. Click **New Repository Secret**.
3. Add:
   - **Name:** `REPO_WEBSITE_ACCESS_TOKEN`
   - **Value:** Paste the **PAT** generated in Step 1.
4. Save the secret.

---

## ✅ Testing the Setup

1. Push a commit to `repo-aep`.
2. Go to [`repo-website` Actions](https://github.com/andy108369/repo-website/actions) and check if the `Dispatch Listener` workflow was triggered.
3. If the workflow fails:
   - Check **repo-aep → Actions logs**.
   - Ensure the **PAT has the correct permissions**.

---

### ⚠️ Notes
- **GITHUB_TOKEN won't work** for triggering workflows in another repo.
- **Fine-grained PATs require `Contents: Read & Write` for repository dispatch to work.**
- If the workflow still fails, check if the token is scoped to `repo-website` only.
