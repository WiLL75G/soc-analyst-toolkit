# Git: Learning Doc

Written to be understood, not just followed. The workflow section is the part you'll use daily. The mental model section is the part that stops you being stuck the first time reality deviates from the script.

---

## Part 1: The mental model

Everything in Git follows from one idea. There are **three places a file can be**.

```
  working directory  →   staging area   →   repository
   (your folder)        (the rehearsal)     (the history)
```

**Working directory.** The actual folder on your machine. What you see in Finder. Edit a file here and Git notices, but does nothing about it.

**Staging area.** A holding pen. You put things here to say "this belongs in the next snapshot." Nothing is recorded yet. You can add and remove freely.

**Repository.** The `.git` folder. Permanent history. Once something lands here, it stays, even if you delete the file later.

Three commands move things between them:

```
git add      working directory  →  staging area
git commit   staging area       →  repository
git push     repository (local) →  GitHub
```

That's it. Every other Git command is either **looking at these three states** or **moving something backwards between them**.

Once this clicks, Git stops being a list of incantations.

---

## Part 2: Seeing where you are

The three commands that show you the three states:

```bash
git status
```
What's changed, what's staged. Run it constantly. Before every command, after every command. It's not a beginner crutch; people who are good at Git run it more, not less.

```bash
git diff
```
What changed in the working directory, not yet staged.

```bash
git diff --staged
```
What's staged and about to be committed. **Run this before every commit.** It's the last checkpoint before something enters permanent history. On a forensics repo, this is where you catch the unredacted hostname.

```bash
git log --oneline
```
The history. Read it as the story of the work.

---

## Part 3: The workflow

### First time setup

```bash
git config --global user.name "Julian Derry"
git config --global user.email "your@email.com"
```

This stamps every commit you make. Do it once.

### Starting a repo

On GitHub: New repository. Name it, describe it, **public**, do **not** tick "Add a README" (you have one; an auto-generated one causes a conflict on first push).

Locally:

```bash
cd ~/Desktop
mkdir windows-insider-threat-investigation
cd windows-insider-threat-investigation
```

Build the tree:

```bash
mkdir -p 00-case-summary 01-acquisition 02-artifacts/{registry,filesystem,memory,network} 03-analysis 04-findings 05-report screenshots
```

`mkdir -p` creates parents as needed. The braces expand into four subfolders in one go.

**Git does not track empty folders.** It tracks files. An empty `02-artifacts/registry/` will not appear on GitHub at all. So:

```bash
touch 02-artifacts/registry/.gitkeep
```

`.gitkeep` is not a Git feature. It's a convention: an empty file that gives Git something to track so the folder survives.

### Set .gitignore before anything else

```bash
cat > .gitignore << 'EOF'
*.E01
*.dd
*.raw
*.mem
*.bin
.DS_Store
EOF
```

Do this **first**, before your first `git add`. See Part 5 for why this ordering matters more on a forensics repo than anywhere else.

### First commit

```bash
git init
```

Creates the hidden `.git` folder. That folder **is** the repository. Nothing has touched GitHub yet.

```bash
git status          # look
git add .           # stage everything
git diff --staged   # look again, carefully
git commit -m "Add case study structure and README"
```

### Connect and push

```bash
git remote add origin https://github.com/jderrydev-collab/windows-insider-threat-investigation.git
git branch -M main
git push -u origin main
```

`origin` is a nickname for the remote URL. Convention, not magic.
`-u` links local `main` to remote `main`, so future pushes are just `git push`.

### Authentication

HTTPS prompts for a password. **Your GitHub password will not work.** Disabled for Git operations since 2021.

GitHub, then Settings, then Developer settings, then Personal access tokens, then Tokens (classic), then Generate new token. Tick `repo` scope. Copy it immediately, it cannot be viewed again. Paste it where it asks for a password.

### The daily loop

```bash
git status
git add .
git diff --staged
git commit -m "Add registry analysis and MFT timeline"
git push
```

Five commands. That's the rhythm.

---

## Part 4: Undo

You will make mistakes. Every one of these is recoverable. Knowing that is most of what makes Git usable.

**Discard a change you haven't staged:**
```bash
git restore filename.md
```
The file goes back to how it was at the last commit. The edit is gone. This one is genuinely destructive, so be sure.

**Unstage something you staged by mistake:**
```bash
git restore --staged filename.md
```
Moves it back from staging to working directory. The edit survives, it's just no longer queued for commit.

**Fix the last commit message:**
```bash
git commit --amend -m "Better message"
```

**Add a forgotten file to the last commit:**
```bash
git add forgotten-file.md
git commit --amend --no-edit
```
`--no-edit` keeps the existing message.

**Only amend commits you haven't pushed.** Amending rewrites history. If it's already on GitHub, rewriting causes problems for anyone who pulled it. On a solo repo you can get away with it, but build the habit now.

**Undo a commit you already pushed:**
```bash
git revert <commit-hash>
```
Creates a *new* commit that undoes the old one. History stays intact, nothing is rewritten. This is the safe option and the right one for a public repo.

Get the hash from `git log --oneline`.

---

## Part 5: The forensics-specific problem

**Once something is committed and pushed, it's in history permanently.** Deleting the file in a later commit does not remove it. It stays recoverable from the log forever, by anyone.

For a normal portfolio repo that's a nuisance. For a forensics repo it's a professional problem. Evidence files, unredacted extractions, real hostnames, client names, personal data from a device image. A DFIR analyst who leaks evidence through their portfolio has demonstrated the exact opposite of the skill they're advertising.

**So: `.gitignore` first, always.** Before `git init` if you can manage it. Certainly before the first `git add`.

**If it does happen:**

If you haven't pushed yet:
```bash
git reset --soft HEAD~1
```
Undoes the commit, keeps your files. Fix the `.gitignore`, then re-commit.

If you have pushed:

Stop. Don't try to fix it with a delete commit, that doesn't work. The file needs removing from history entirely, which means `git filter-repo` or BFG Repo-Cleaner, plus a force push, plus rotating anything that was exposed.

Realistically, for a small portfolio repo: **delete the repo on GitHub and start over.** It's faster and more certain than surgery on the history. Nothing of value is lost when the repo is three commits old.

The point of knowing this is to make `.gitignore` feel non-negotiable rather than optional.

---

## Part 6: Branches

You don't need branches for a solo portfolio repo. `main` is enough.

You do need them the moment you contribute to anything, or collaborate with anyone. Worth learning before that day, not during it.

The idea: a branch is a parallel line of work. You split off `main`, do something, and merge it back when it's done. `main` stays clean while you experiment.

```bash
git branch                        # list branches
git checkout -b add-memory-analysis   # create and switch to a new one
# ...work, commit...
git checkout main                 # go back
git merge add-memory-analysis     # bring the work in
git branch -d add-memory-analysis # clean up
```

That's the shape of it. **Learn Git Branching** (link below) teaches this visually and it's the fastest way in. Reading about branches is much worse than seeing them move.

---

## Part 7: Where to actually learn this

This doc is a reference. It'll get you working and it'll get you unstuck. It is not a course, and I'd rather point you at the good stuff than write a worse version of it.

**Pro Git**, chapters 1 to 3. Free, online, written by the people who build Git. Chapter 2 is the one that matters: it covers the three-state model properly. An afternoon.
https://git-scm.com/book

**Learn Git Branching.** Interactive, in the browser, visual. You watch commits move as you type commands. The best available way to build intuition for branching and merging.
https://learngitbranching.js.org

**Oh Shit, Git!?!** Short, plain-language recovery recipes for common disasters. Bookmark it before you need it.
https://ohshitgit.com

Do Pro Git chapters 1 to 3 first. Then this doc becomes a cheat sheet for something you already understand, which is what a reference should be.

---

## Command summary

| Command | What it does |
|---|---|
| `git status` | What's changed, what's staged |
| `git diff` | Changes not yet staged |
| `git diff --staged` | Changes about to be committed |
| `git add .` | Stage everything |
| `git commit -m "..."` | Record staged changes |
| `git push` | Upload to GitHub |
| `git log --oneline` | History, one line per commit |
| `git restore <file>` | Discard unstaged changes |
| `git restore --staged <file>` | Unstage, keep the edit |
| `git commit --amend` | Fix the last commit |
| `git revert <hash>` | Safely undo a pushed commit |
| `git remote -v` | Which remote is connected |
