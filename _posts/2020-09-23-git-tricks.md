---
layout: post
title: Git CLI tricks
categories:
  - git
---

If you use Git as source code management tool, you probably use a plugin for your favorite IDE or a desktop application.
Anyway, sometimes you need to use the CLI to perform some tricky commands.

Here you could find my personal tricks for Git CLI I learned during my work (or too late to be used in my day-by-day job).

## Undo last commit
Sometimes you need to undo your last commit, because you need to remove a file from that or to avoid commit some secrets.
In that case, `git reset` is the command you need to run.

If you would like to preserve the change, so you could update it, then you need to run

```bash
git reset --soft HEAD~1
```

Otherwise, if you would delete completely the changes, you need to run
```bash
git reset --hard HEAD~1
```

If you already push your commit on remote repository, then you need to force the push in order to remove also from the history of the remote.
```bash
git reset --hard HEAD~1
git push --force
```

## Exclude merge commits checking logs
Merge commits could be useful, but they get dirty the history of the repository.
If you would like to analyze the history without those commits, you could use this.
```bash
git log --oneline --graph --no-merges
```

## Easy changelog
Create changelogs is annoying, but it is important before release a new version of your application.
The easiest way to do that in Git is
```bash
git shortlog XXXXXX..HEAD --no-merges
```
where `XXXXXX` is the SHA code for the previous commit.
This works also with tags, following the rule `OLD_TAG..NEW_TAG`.

## Clean up deleted branches and tags
If you use branches for each feature you create, you will have tons of branches in your repo, and you will need to clean up stolen one.
You could clean up your local repo while you are pulling the updates.
```bash
git pull --all --prune
```