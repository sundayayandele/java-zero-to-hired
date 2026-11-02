## What you will be able to do

- Explain in plain words what Git and GitHub are, and how this course uses them.
- Use GitHub in a web browser: create an account and a repository, upload and edit files, read the history, download a project as a ZIP and fork one.
- Explain what version control is for, and what a commit, a repository and a remote are.
- Set up Git, make commits, see what changed and go back to an earlier version.
- Keep build output and secrets out of a repository with `.gitignore`.
- Put a project on GitHub, push changes, and clone it on another machine.
- Work on a branch, open a pull request on your own repository, review the diff and merge it.
- Add a GitHub Actions workflow that builds and tests every push, and show its badge in your README (by week 5, for your Module 1 project).

## Why this matters in a bank or fintech

Every line of code in a bank is in version control. Auditors ask "who changed this fee rule, when, and who approved it?", and the answer comes from Git history and pull requests. Teams deploy only what has passed CI on the main branch. For you, GitHub is also your portfolio: hiring managers open your repositories before they open your CV. This topic covers what you need from day one; Module 8 (topic 8.1) adds team workflows, rebasing and branch protection.

## 1. Git and GitHub in plain words

**Git** is a free program on your computer that saves **versions** of a project folder. Think of it as taking a photo of the whole folder whenever you reach a good point: you can look back at any photo, see exactly what changed, and go back if something breaks.

**GitHub** is a website that stores a copy of those projects online. It is a backup (a lost laptop doesn't lose your work), a shared space (others can read your code and suggest changes) and, for you, a **portfolio**: employers look at your GitHub profile to see what you've built.

Git works without GitHub, and you can use GitHub in a browser without installing Git. Professionals use both together. In this course you will:

- **read** the course's source on GitHub (the "Source on GitHub" link on every lesson);
- **download** starter projects from GitHub;
- **store** every project you build in your own GitHub repositories;
- **submit** your work by pasting a link to your repository into Moodle.

This lesson starts in the browser (sections 2 and 3), then moves to Git on your computer (sections 4 to 10). If the command line feels like a lot, do sections 2 and 3 today and the rest tomorrow.

## 2. Start in the browser: your account and your first repository

1. Go to [github.com](https://github.com) and click **Sign up**. Use an email you'll keep, and choose a **professional username** such as `ada-obi` (it appears on your CV, so avoid nicknames). Verify your email when GitHub asks.
2. Set up **two-factor authentication** when GitHub prompts you (Settings → Password and authentication), with an authenticator app on your phone. GitHub requires it for anyone who publishes code, and it protects your portfolio.
3. Click **+** (top right) → **New repository**. Name it `java-onramp`, choose **Public**, tick **Add a README file**, and click **Create repository**. A **repository** (or **repo**) is one project's folder on GitHub, with its history.
4. Click **Add file → Upload files**, drag in the `Main.java` you wrote in week 0, and click **Commit changes**. A **commit** is a saved version with a short message saying what changed.
5. Click `Main.java`, then the pencil icon, change a line, and click **Commit changes** again.
6. Click the **commits** link near the top of the file list (it shows a clock icon and a number). Each line is one saved version; click one to see exactly what changed, in red (removed) and green (added).

You've just done, in a browser, what the rest of this lesson does with commands. GitHub's own [Hello World guide](https://docs.github.com/en/get-started/start-your-journey/hello-world) walks through the same steps with screenshots.

## 3. Getting code from GitHub: download, fork and clone

There are three ways to get a project from GitHub, and this course uses all three:

| Way | What it gives you | When you use it here |
| --- | --- | --- |
| **Download ZIP** | A copy of the files on your computer, with no history and no link back | The starter projects, when you just want the code. On the course repository's main page, click **Code → Download ZIP** and unzip it; the starters are in its `templates` folder |
| **Fork** | Your own copy of someone else's repository, **on GitHub**, under your account | The coding practice track: you fork the course repository, solve problems in your copy and link it in Moodle. Open the repository, click **Fork → Create fork** ([GitHub's guide](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)) |
| **Clone** | A copy on your computer **with** its history, linked to GitHub so you can push changes back | Your own repositories and your forks, once Git is set up (section 8) |

## 4. What Git does on your computer

Git records **snapshots** of a project folder over time. Each snapshot is a **commit**: the full state of the tracked files, a message saying why it changed, the author and a timestamp. Commits form a history you can read, compare and return to.

| Term | Meaning |
| --- | --- |
| **Repository (repo)** | A project folder whose history Git tracks (in a hidden `.git` folder) |
| **Working tree** | The files as they are on your disk right now |
| **Staging area** | The changes you've chosen to include in the next commit |
| **Commit** | A saved snapshot with a message, identified by a hash such as `3f9c2ab` |
| **Branch** | A movable name for a line of commits; the default is `main` |
| **Remote** | A copy of the repository on a server, usually GitHub, named `origin` |

GitLab and Azure DevOps do the same job as GitHub; the Git commands are identical.

## 5. Set up once

Install Git:

- **Windows:** download **Git for Windows** from [git-scm.com/downloads/win](https://git-scm.com/downloads/win) and run it, keeping the default answers. It also installs **Git Credential Manager**, which signs you in to GitHub through your browser the first time you push.
- **macOS:** run `xcode-select --install` in Terminal.
- **Ubuntu:** `sudo apt install git`.

Open a **new** terminal, then tell Git who you are. This name and email go into every commit:

```bash
git config --global user.name "Ada Obi"
git config --global user.email "ada.obi@example.com"   # use the email on your GitHub account
git config --global init.defaultBranch main
git --version                                           # 2.40 or newer is fine
```

Prefer clicking to typing at first? [GitHub Desktop](https://desktop.github.com/download/) does commits, pushes and branches with buttons. It's a fine way to start, but learn the commands below too: every job interview and every server uses them.

## 6. Your first commits

Start from the [Java CLI starter](../../../../templates/java-cli-starter/README.md) (download it as a ZIP, section 3) or any Maven project from topic 1.1:

```bash
cd fee-calculator
git init                         # create the repository
git status                       # what has changed? (run this constantly)
```

`git status` lists **untracked** files. Before adding anything, create `.gitignore` so build output and personal files never enter the repository:

```
target/
.idea/
*.iml
.env
*.log
.DS_Store
```

Then stage and commit:

```bash
git add .gitignore pom.xml mvnw mvnw.cmd .mvn src
git commit -m "Create fee calculator project with Maven wrapper"
git log --oneline                # 3f9c2ab Create fee calculator project with Maven wrapper
```

The everyday loop is: **change files → `git status` → `git diff` → `git add` → `git commit`**.

```bash
# edit FeeCalculator.java, then:
git diff                         # exactly what changed, line by line (- removed, + added)
git add src/main/java/ng/itlearning/fees/FeeCalculator.java
git commit -m "Charge ₦25 for transfers above ₦5,000"
```

### Writing good commit messages

A commit message explains **why**, for the reader in six months (often you). Use the imperative mood ("Add", "Fix", "Charge"), keep the first line under about 72 characters, and make each commit one logical change.

| Weak | Better |
| --- | --- |
| `update` | `Reject transfers from frozen wallets` |
| `fix bug` | `Round VAT on fees half-even to the kobo` |
| `stuff` | `Add statement export as CSV` |

### Looking back and undoing

```bash
git log --oneline --graph        # history
git show 3f9c2ab                 # one commit in full
git diff HEAD~1                  # compare with the previous commit
git restore FeeCalculator.java   # throw away uncommitted changes to a file (careful: gone for good)
git restore --staged pom.xml     # unstage, keeping your edits
git revert 3f9c2ab               # a new commit that undoes an old one: safe on shared history
```

Topic 8.1 covers the more powerful (and more dangerous) tools such as `reset` and interactive rebase.

## 7. Secrets never go in Git

Passwords, API keys, tokens and private keys must **never** be committed, even to a private repository, and even "just for testing". Git keeps every version forever: deleting the file in a later commit doesn't remove it from history, and bots scan public GitHub for keys within minutes.

- Keep secrets in environment variables or a local `.env` file that is listed in `.gitignore` (topic 4.5 covers configuration properly).
- Commit a `.env.example` with the variable **names** and fake values, so others know what to set.
- Turn on GitHub's **secret scanning and push protection** (repository Settings → Code security). It blocks many known key formats before they leave your machine.
- **If you do commit a secret: rotate it first** (create a new key and revoke the old one), then remove it from the code. Rotating is the fix; cleaning history is secondary. In the capstones, a committed secret caps your *Delivery* score.

## 8. Push your project to GitHub, and clone

Your computer needs permission to push to your GitHub account. GitHub doesn't accept your account password for this:

- **Windows:** nothing to do now. The first `git push` opens a browser window where you sign in to GitHub (Git Credential Manager), and it remembers you afterwards.
- **macOS and Linux:** install the [GitHub CLI](https://cli.github.com/) (`brew install gh` on a Mac with Homebrew; the site has Linux instructions), then run once:

```bash
gh auth login                    # choose GitHub.com, HTTPS, and log in with a browser
```

(Alternatively, add an SSH key: `ssh-keygen -t ed25519 -C "you@example.com"`, then paste the `.pub` file into GitHub → Settings → SSH and GPG keys.)

Create an empty repository on GitHub (no README, so the histories don't clash), then connect and push:

```bash
git remote add origin https://github.com/example-user/fee-calculator.git
git push -u origin main          # -u remembers the link; afterwards just "git push"
```

On another computer, or to start from someone else's project:

```bash
git clone https://github.com/example-user/fee-calculator.git
cd fee-calculator
git pull                         # fetch and merge new commits from GitHub
```

Push at the end of every working session. A laptop can be stolen; GitHub is your backup and your portfolio.

## 9. Branches and pull requests

A **branch** lets you work on a change without disturbing `main`. Even working alone, use a branch per change and merge it through a **pull request (PR)**, because that's how every team works and it gives you a page to review your own diff.

```bash
git switch -c add-pos-fee        # create a branch and move to it
# edit, test, commit as usual
git commit -am "Add 0.5% POS fee capped at ₦100"
git push -u origin add-pos-fee
```

GitHub then offers **Compare & pull request**. A good PR description says what changed, why, and how you tested it. Read the **Files changed** tab line by line before merging: reviewers find a surprising number of bugs in their own diffs. Click **Merge pull request**, then update your local `main`:

```bash
git switch main
git pull
git branch -d add-pos-fee        # delete the merged branch
```

### Merge conflicts, briefly

If two branches change the same lines, Git can't choose and marks the file:

```
<<<<<<< HEAD
    return 2_500;
=======
    return 2_600;
>>>>>>> add-pos-fee
```

Edit the file to the correct final version, remove the three marker lines, then `git add` the file and `git commit`. Run the tests before you commit. Topic 8.1 practises conflicts in a team setting.

## 10. Your first GitHub Actions workflow (by week 5)

You don't need this in your first week: getting your code onto GitHub (sections 2 to 8) is the week 1 goal. Come back to this section when you start your Module 1 project, which must build on GitHub.

**Continuous integration (CI)** means a server builds and tests every push, so "it works on my machine" is checked on a clean machine. Add `.github/workflows/ci.yml`:

```yaml
name: ci

on:
  push:
  pull_request:

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5
      - uses: actions/setup-java@v5
        with:
          distribution: temurin
          java-version: "21"
          cache: maven
      - run: ./mvnw -B verify
```

Commit and push it, then open the **Actions** tab: a green tick means the project compiled and every test passed on a fresh Ubuntu machine with Java 21. If it's red, click into the failed step and read the log from the first error, just as you would locally. On Linux the wrapper must be executable; if CI says `Permission denied`, run `git update-index --chmod=+x mvnw` and commit.

Show the result on your README so a recruiter sees it at a glance:

```markdown
![ci](https://github.com/example-user/fee-calculator/actions/workflows/ci.yml/badge.svg)
```

From now on, **every project in this programme is pushed to GitHub with a green CI run**: the module projects, the capstones and the practice track. Topic 1.6 adds the tests that make that green tick mean something.

## Worked example: a fee change from branch to merge

1. `git switch -c raise-transfer-fee`.
2. Change the ₦25 band to ₦26.88 (₦25 plus 7.5% VAT), update the README's fee table, and run `./mvnw verify` locally.
3. `git diff` to check only the intended lines changed, then `git commit -am "Include VAT in the mid-band transfer fee"`.
4. `git push -u origin raise-transfer-fee`, open a PR, and write: *"Why: finance asked for VAT-inclusive fees. Tested: existing tests updated; boundary at ₦5,000.00 and ₦5,000.01 checked."*
5. Wait for the CI check on the PR to go green, read **Files changed** once more, merge, `git switch main && git pull`.

The history now shows who changed the fee, when, why, and that the build passed: exactly what an auditor asks for.

## Common mistakes

- Committing `target/`, `.idea/` or `.env` because `.gitignore` came after the first `git add .`.
- Committing a password or API key, then "fixing" it by deleting the file instead of rotating the key.
- One giant commit called `update` at the end of the week instead of small commits with reasons.
- Working directly on `main` and pushing untested code.
- Forgetting to push, so the work exists only on one laptop.
- Ignoring a red CI run, or a README badge that's red.

## Practice

1. In the browser (section 2): create a public repository, upload a program from week 0, edit it on GitHub and read its history. Then download it as a ZIP, and fork the course repository (section 3).
2. Put your on-ramp project (or any program from topic 1.1) under Git with a `.gitignore`, make at least five small commits, and push it to a public GitHub repository.
3. Make a change, look at it with `git diff`, then throw it away with `git restore`. Make another change, commit it, and undo it with `git revert`.
4. Create a branch, change one method, open a pull request, review your own diff in **Files changed**, and merge it.
5. Create a merge conflict on purpose: change the same line on `main` and on a branch, merge, resolve it, and commit.
6. Add the CI workflow above (by week 5) and the badge. Break the build on a branch (a compile error), push, see the red check on the pull request, fix it and merge only when green.
7. Turn on secret scanning and push protection for your repository.

## Key terms

Git, GitHub, fork, Download ZIP, version control, repository, working tree, staging area, commit, hash, `.gitignore`, remote, `origin`, push, pull, clone, branch, `main`, pull request, diff, merge, merge conflict, revert, continuous integration (CI), GitHub Actions, workflow, badge, secret scanning, rotation.

## Further reading

- [Pro Git (free book), chapters 1–3](https://git-scm.com/book/en/v2)
- [GitHub Docs: Get started with Git](https://docs.github.com/en/get-started/getting-started-with-git)
- [GitHub Docs: About pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
- [GitHub Docs: Building and testing Java with Maven](https://docs.github.com/en/actions/use-cases-and-examples/building-and-testing/building-and-testing-java-with-maven)
- [GitHub Docs: Secret scanning and push protection](https://docs.github.com/en/code-security/secret-scanning/introduction/about-push-protection)
- [GitHub CLI manual: `gh auth login`](https://cli.github.com/manual/gh_auth_login)
