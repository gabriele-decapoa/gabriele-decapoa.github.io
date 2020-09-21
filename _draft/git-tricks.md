---
layout: post
title: Git tricks
categories:
  - git
---

### Undo last commit preserving changes
```bash
git reset --soft HEAD~1
```

### Undo last commit deleting changes
```bash
git reset --hard HEAD~1
```

### Undo last commit deleting changes also in remote repository
```bash
git reset --hard HEAD~1
git push --force
```