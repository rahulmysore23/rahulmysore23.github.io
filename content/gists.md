---
title: "Gists"
showDate: false
showReadingTime: false
showBreadCrumbs: false
showAuthor: true
sharingLinks: false
showWordCount : false
draft: true
layout: "page"
description: "Collection of commands for developers"
---

This is a personal collection of commands for day to day use. If you are a developer who works with tools below, you can use these commands to save time and increase your productivity.

### Git

- Rebase from main with merge strategy

```bash
git rebase -m main
```

- Cherry pick changes from another branch

```bash
git cherry-pick <commit_hash>
```

- Clean local branches except current branch

```bash
git branch --merged | grep -v 'master\|main' | xargs git branch -d
```

- Another powerful command is git worktree, which allows you to create multiple working trees for the same repository.

```bash
git worktree add <path> <branch>
```

- Stash your changes and apply them later

```bash
git stash save "WIP"
```

- Apply stashed changes and drop the stash

```bash
git stash pop WIP
```


### Neo4j

- Batch delete all nodes and relationships

```bash
MATCH (n) DETACH DELETE n;
```

### Kubernetes

-

### Installations

- Install mysql

```bash
sudo yum install mysql-server
```

-
