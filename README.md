# 📚 BMS Functional Knowledge Vault — Onboarding Guide

Welcome to the team! This guide walks you through everything you need to set up your computer,
join our shared knowledge vault, and contribute notes every day, even if you have never used
Git or GitHub before.

Read it from top to bottom the first time. After that, keep the
[Daily Routine](#-your-daily-routine) section bookmarked.

---

## Table of Contents

1. [How Our System Works](#1-how-our-system-works)
2. [Install the Required Tools](#2-install-the-required-tools)
3. [Create a GitHub Account](#3-create-a-github-account)
4. [Configure Git on Your Computer](#4-configure-git-on-your-computer)
5. [Connect Your Computer to GitHub Securely](#5-connect-your-computer-to-github-securely)
6. [Clone the Vault to Your Computer](#6-clone-the-vault-to-your-computer)
7. [Open the Vault in Obsidian](#7-open-the-vault-in-obsidian)
8. [Create Your Personal Branch](#8-create-your-personal-branch)
9. [Your Daily Routine](#9-your-daily-routine)
10. [How the Team Lead Reviews and Merges](#11-how-the-team-lead-reviews-and-merges)
11. [Common Problems and Fixes](#11-common-problems-and-fixes)
12. [Quick Command Reference](#12-quick-command-reference)

---

## 1. How Our System Works

Before touching any tool, understand the big picture. Our documentation system has three parts
that work together:

| Tool | What It Does | Think of It As |
|------|-------------|----------------|
| **Git** | Tracks every change ever made to every file | A full history of all edits, forever |
| **GitHub** | Stores the vault online so the whole team can access it | A shared Google Drive, but smarter |
| **Obsidian** | The app you use to read and write notes | Microsoft Word, but for a shared folder |

### How the Team Works Together

Our team of ten each has their **own personal branch** — a private workspace inside the shared
vault. Here is the full flow:

```
Every team member has their own branch: user/firstname-lastname

Morning routine:
  Your branch ← pull latest updates from main   (you receive teammates' approved notes)

During the day:
  Write notes in Obsidian → save → commit → push to your branch

When ready to share:
  Open a Pull Request on GitHub

Team Lead:
  Reviews your Pull Request → merges it into main → notifies the team
```

**The three rules every team member must know:**

> **Rule 1 — You always stay on your own branch. Never work on `main`.**
>
> **Rule 2 — Every morning, pull the latest updates from `main` into your branch.**
>
> **Rule 3 — When you are ready to share your notes, push your branch and the Team Lead will merge it.**

---

## 2. Install the Required Tools

You need three tools installed before anything else.

### 2.1 Git

Git is the engine that tracks changes. It runs in the background — you will mostly not see it
directly after setup.

| Your System | What to Do |
|-------------|-----------|
| **macOS** | Open **Terminal** and run: `brew install git` *(if you do not have Homebrew, install it first at [brew.sh](https://brew.sh))* |
| **Windows** | Download and run the installer from **[git-scm.com/download/win](https://git-scm.com/download/win)** — accept all default settings |

After installing, check it worked — open your terminal and run:

```bash
git --version
```

You should see something like `git version 2.43.0`. Any version above 2.30 is fine.

---

### 2.2 Obsidian

Obsidian is the app where you will read and write all team notes.

1. Go to **[obsidian.md](https://obsidian.md)** and download the installer for your system.
2. Run the installer.
3. When it opens and asks you to create or open a vault — **stop here for now**.
   You will open the correct folder in Step 7 after cloning the repository.

> ⚠️ **Do not set up Obsidian Sync.** We use Git to sync — it is free and already included.

---

### 2.3 Visual Studio Code *(Recommended)*

VS Code is a free text editor that makes it much easier to deal with any Git issues that come up.

1. Download from **[code.visualstudio.com](https://code.visualstudio.com)** and install with default settings.
2. After installing, open VS Code and install the **GitLens** extension
   *(Extensions panel on the left → search "GitLens" → Install)*.

---

## 3. Create a GitHub Account

GitHub is where the vault lives online.

1. Go to **[github.com](https://github.com)** and click **Sign up**.
2. Use your **work email address**.
3. Choose a username that is easy to recognise — ideally `firstname-lastname`
   (e.g. `singa-maurice`).
4. Complete the verification and confirm your email.
5. **Send your GitHub username to the Team Lead** so they can give you access to the repository.

> The Team Lead will add you as a collaborator. You will receive an email invitation from GitHub —
> accept it before moving to the next step.

---

## 4. Configure Git on Your Computer

Git needs to know who you are so your name appears correctly on every change you make.
Run these commands once — open your terminal and copy them in one by one,
replacing the example values with your real name and work email.

```bash
git config --global user.name  "Your Full Name"
git config --global user.email "yourname@company.com"
```

Then run these two extra settings that make the team workflow smoother:

```bash
# Sets the default branch name to "main" (the modern standard)
git config --global init.defaultBranch main

# Keeps history clean when pulling updates from teammates
git config --global pull.rebase false
```

Check everything looks right:

```bash
git config --global --list
```

You should see your name and email in the output.

> ⚠️ **The email here must match the email on your GitHub account.**
> If they are different, your contributions will appear anonymous.

---

## 5. Connect Your Computer to GitHub Securely

Instead of typing a password every time you interact with GitHub, we use an
**SSH key** — a digital pass that identifies your computer automatically.
You do this once and never think about it again.

### Step 1 — Generate your key

Open your terminal and run *(replace the email with your GitHub email)*:

```bash
ssh-keygen -t ed25519 -C "yourname@company.com"
```

When it asks:
- **"Enter file in which to save the key"** → press **Enter** to accept the default
- **"Enter passphrase"** → type a strong password and remember it

### Step 2 — Start the SSH agent and register your key

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### Step 3 — Copy your public key to GitHub

Run this to display your public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the entire output (it starts with `ssh-ed25519` and ends with your email).

Then on GitHub:
1. Click your profile picture (top-right) → **Settings**
2. Left sidebar → **SSH and GPG keys**
3. Click **New SSH key**
4. Title: type anything, e.g. `Work Laptop`
5. Paste the key into the **Key** field
6. Click **Add SSH key**

### Step 4 — Test the connection

```bash
ssh -T git@github.com
```

Expected response:
```
Hi YourUsername! You've successfully authenticated, but GitHub does not provide shell access.
```

If you see that message, you are connected. ✅

> 🔒 **Security note:** The file `~/.ssh/id_ed25519` is your **private key** — never share it,
> never commit it to any repository. The `.pub` file is the public one — that is the one you gave to GitHub.

---

## 6. Clone the Vault to Your Computer

**Cloning** means downloading a copy of the repository from GitHub to your computer.
You do this **once**. After that, Git keeps everything in sync.

### Choose where to save it

Pick a stable folder — avoid Desktop, Downloads, or any folder synced by
Dropbox / OneDrive / Google Drive *(those tools conflict with Git)*.

Good choices:
- macOS / Linux: `~/Documents/work/`
- Windows: `C:\Users\YourName\Documents\work\`

### Run the clone command

Open your terminal, navigate to your chosen folder, and clone the repository.
The Team Lead will give you the exact URL — it looks like this:

```bash
# Go to your chosen folder:
cd ~/Documents/work

# Clone the repository (replace with the URL the Team Lead gives you):
git clone git@github.com:YOUR-ORG/REPO-NAME.git

# Enter the folder:
cd REPO-NAME

# Confirm the content is there:
ls
```

You should see the team folders listed (`01-PROJECTS`, `02-WORKFLOWS`, `README.md`, etc.).

---

## 7. Open the Vault in Obsidian

1. Open **Obsidian**.
2. On the welcome screen, click **"Open folder as vault"**.
3. Navigate to the folder you just cloned
   (e.g. `~/Documents/work/REPO-NAME`) and click **Open**.

Obsidian will load and you will see all the team folders in the left sidebar. That is your vault.

> 💡 **The vault and the repository are the same folder.**
> Every note you create in Obsidian is a file that Git is already watching.
> There is nothing special to do — just write in Obsidian as you normally would.

---

## 8. Create Your Personal Branch

Your personal branch is **permanent** — you create it once and use it for as long as you are
on the team. It is your private workspace inside the shared vault.

**Branch naming format:** `user/firstname-lastname`

Examples:

| Team Member | Their Branch |
|------------|-------------|
| Meresia | `user/meresia` |
| Margaret | `user/margaret` |
| Janet | `user/janet` |
| Boluwatife | `user/boluwatife` |

### Create your branch

Open your terminal, make sure you are inside the repository folder, and run:

```bash
# 1. Make sure you start from the latest version of main:
git checkout main
git pull origin main

# 2. Create your personal branch (replace with your actual name):
git checkout -b user/your-firstname-lastname

# 3. Push your branch to GitHub so the Team Lead can see it:
git push -u origin user/your-firstname-lastname
```

After the `-u` flag is used once, future pushes are simply `git push`.

Confirm you are on the right branch:

```bash
git branch
```

You should see a `*` next to your branch name. That asterisk means
"this is where I am right now."

```
  main
* user/your-firstname-lastname   ← you are here
```

> ⚠️ **You never need to switch to `main`.** If you accidentally end up there,
> run `git checkout user/your-firstname-lastname` to come back.

---

## 9. Your Daily Routine

This is the routine every team member follows every working day.
It takes about 2 minutes of Git activity — the rest of the time you simply write in Obsidian.

---

### 🌅 Morning — Pull the Latest Updates

Before writing anything, get the latest approved notes from your teammates.

**In the terminal:**

```bash
# 1. Make sure you are on your branch:
git checkout user/your-firstname-lastname

# 2. Pull the latest content from main INTO your branch:
git pull origin main
```

**Or in Obsidian (no terminal needed):**

```
Press Ctrl+P (Cmd+P on Mac)
Type: Git: Pull
Press Enter
```

Your vault now shows everything that the Team Lead has merged since your last session.

> 💡 **"Pulling from main" means:** take all the approved notes from the shared vault and
> bring them into your local copy — while you stay on your own branch.
> You are not switching to main. You are just receiving its content.

---

### ✍️ During the Day — Write Notes in Obsidian

Open Obsidian and work as normal. Use the correct folder for what you are writing:

| What You Are Writing | Folder | File Name Format |
|---------------------|--------|-----------------|
| Workflow notes | `02-WORKFLOWS/` | `YYYY-MM-DD-workflow-name.md` |
| Project documents | `01-PROJECTS/` | `project-name/document.md` |
| How-to guides / procedures | `04-RUNBOOKS/` | `how-to-topic.md` |
| Team decisions | `03-DECISIONS/` | `ADR-00X-short-title.md` |
| Personal drafts (private) | `06-INBOX/` | any name — these stay on your computer only |

---

### 💾 Save Your Work — Commit and Push

When you have finished writing, or reached a good stopping point, save your work to GitHub.
This backs it up and makes it available for the Team Lead to review.

**In the terminal:**

```bash
# 1. Stage all your new and modified files:
git add -A

# 2. Write a short description of what you added:
git commit -m "docs: add Kazi notes 24 Feb"

# 3. Push to your branch on GitHub:
git push origin user/your-firstname-lastname
# Or just: git push  (after the first push with -u)
```

**Or in Obsidian:**

```
Ctrl+P → "Git: Commit all changes" → type your message → Enter
Ctrl+P → "Git: Push" → Enter
```

**Commit message examples:**

| What You Did | Message to Write |
|-------------|-----------------|
| Added meeting notes | `docs: add 24 Feb sprint review notes` |
| Updated a project document | `docs: update alpha project timeline` |
| Fixed a mistake | `fix: correct wrong date in standup notes` |
| Added a new runbook | `docs: add deployment runbook` |

---

### 🔔 Open a Pull Request When Ready to Share

A **Pull Request** tells the Team Lead: *"My notes are ready — please review and add them
to the shared vault."*

1. Go to **[github.com](https://github.com)** and open the team repository.
2. You will see a yellow banner: **"Your branch had recent pushes — Compare & pull request"**. Click it.
3. Check that:
   - **Base branch** = `main`
   - **Compare branch** = your personal branch (`user/your-name`)
4. Write a clear title, e.g. `Kazi notes — review 24 Feb 2026`.
5. In the description box, briefly note what you are contributing.
6. Click **"Create pull request"**.

The Team Lead will receive a notification and review your notes. You may receive a comment
asking you to clarify something — just update the note in Obsidian, commit, and push again.
The Pull Request updates automatically.

---

### 🔄 After the Team Lead Merges — Sync Your Vault

Once the Team Lead merges a Pull Request — yours or a teammate's — those notes become part
of `main`. To get them into your vault:

```bash
# Stay on your branch and pull the latest from main:
git pull origin main
```

---

### 📋 Daily Routine at a Glance

```
MORNING
  git checkout user/your-name          confirm you are on your branch
  git pull origin main                 get the latest approved notes

DURING THE DAY
  write notes in Obsidian

WHEN READY TO SHARE
  git add -A
  git commit -m "docs: describe what you added"
  git push origin user/your-name
  open Pull Request on GitHub

AFTER TEAM LEAD MERGES
  git pull origin main                 sync your vault again
```

---

## 10. How the Team Lead Reviews and Merges

*This section is primarily for the Team Lead, but every team member should understand the process.*

### What the Team Lead Does

When a team member opens a Pull Request, the Team Lead:

1. Goes to **GitHub → Pull Requests** and clicks on the new PR.
2. Clicks **"Files changed"** to see exactly what the team member added.
3. Checks for issues (see checklist below).
4. Either **approves and merges** or **requests changes**.

### Review Checklist

Before merging, the Team Lead checks:

- [ ] No personal Obsidian files included (`.obsidian/workspace.json`, `plugins/`)
- [ ] No OS junk files (`.DS_Store`, `desktop.ini`, `Thumbs.db`)
- [ ] Notes are in the correct folder
- [ ] Meeting note files are named with a date prefix (`YYYY-MM-DD-`)
- [ ] Content is legible and makes sense in the shared vault

### How to Merge

1. Click **"Review changes"** → select **"Approve"** → **"Submit review"**.
2. Scroll to the bottom of the PR → click **"Squash and merge"**.
3. Confirm the commit message (e.g. `docs(meetings): add sprint review notes – 24 Feb`).
4. Click **"Confirm squash and merge"**.
5. **Do NOT delete the branch** — each team member's branch is permanent.

### After Merging

Post a message in the team channel so everyone knows to sync:

```
✅ Notes merged: [brief description of what was added]
Everyone please run:  git pull origin main
```

### How to Request Changes

If a PR has an issue (wrong folder, personal files included, etc.):

1. Click **"Review changes"** → **"Request changes"**.
2. Write a clear comment explaining what to fix and how.
3. Click **"Submit review"**.

The team member makes the correction in Obsidian, commits, and pushes.
The PR updates automatically — review it again when the notification arrives.

---

## 11. Common Problems and Fixes

### "Permission denied (publickey)"

Your SSH key is not linked to GitHub correctly.

```bash
# Check that your key is loaded:
ssh-add -l

# If it says "no identities", add your key:
ssh-add ~/.ssh/id_ed25519

# Test the connection again:
ssh -T git@github.com
```

If it still fails, go back to [Step 5](#5-connect-your-computer-to-github-securely) and
re-add your public key to GitHub.

---

### "I accidentally wrote notes on main"

Do not panic. Run:

```bash
# Switch back to your personal branch:
git checkout user/your-firstname-lastname
```

Your notes written in Obsidian are saved as files on your disk — they are not lost.
Stage and commit them from your personal branch as normal.

---

### "I have a merge conflict when pulling"

This happens when you and a teammate both edited the same file.
Git pauses and marks the conflict inside the file like this:

```
<<<<<<< HEAD  (your version)
Action item: Papa to follow up by Friday.
=======
Action item: Papa to follow up by Friday. John to send the budget update.
>>>>>>> origin/main  (your teammate's version)
```

To fix it:

1. Open the file in Obsidian or VS Code.
2. Delete the `<<<<<<<`, `=======`, and `>>>>>>>` lines.
3. Edit the section so it contains the correct final text.
4. Save, then run:

```bash
git add -A
git commit -m "fix: resolve conflict in standup notes"
git push origin user/your-firstname-lastname
```

> 💡 **Prevention:** Pulling `origin/main` every morning is the best way to avoid conflicts.
> The more regularly you sync, the less likely conflicts become.

---

### "I forgot to pull this morning and now I have errors when pushing"

```bash
# Pull first, then push:
git pull origin main
git push origin user/your-firstname-lastname
```

---

### "I committed the wrong file"

```bash
# Undo the last commit but keep your file changes:
git reset --soft HEAD~1

# Then re-stage only the correct files and commit again
```

---

### "I cannot remember which branch I am on"

```bash
git branch
# The * shows your current branch
```

---

## 12. Quick Command Reference

### Team Member — Commands Used Every Day

| When | Command |
|------|---------|
| Start of session — confirm your branch | `git checkout user/your-name` |
| Start of session — get latest updates | `git pull origin main` |
| Stage all changes | `git add -A` |
| Commit with a message | `git commit -m "docs: what you added"` |
| Push your branch | `git push origin user/your-name` |
| Check which branch you are on | `git branch` |
| Check what files have changed | `git status` |
| Undo last commit (keep files) | `git reset --soft HEAD~1` |
| Discard changes to one file | `git restore filename.md` |
| Abort a conflict during pull | `git merge --abort` |

---


### Things to Never Do

| Action | Why |
|--------|-----|
| `git push origin main` (as a team member) | Bypasses review — blocked by branch protection |
| Write notes while on the `main` branch | Notes end up in the wrong place |
| Delete your `user/firstname-lastname` branch | Destroys your workspace and history |
| Put the vault inside Dropbox / OneDrive | Conflicts with Git and corrupts files |
| Commit `.obsidian/workspace.json` | Overwrites teammates' personal Obsidian settings |

---


*Questions? Reach out to the Team Lead or post on team hangout.*

*Vault Onboarding Guide · Version 1.0 · February 2026*
