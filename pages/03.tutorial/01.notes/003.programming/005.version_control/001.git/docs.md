---
title: Git
taxonomy:
    category: docs
---

* If you want to know Git root directory, the one that has `.git` directory, run the following command:
```
git rev-parse --show-toplevel
```

* If you want to change the email address of the last commit which you didn't push, use the following command:
```
git commit --amend --author="Author Name <email@address.com>"
```

* If you want to pull a pull request from [GitHub](https://github.com), you can run one of the two following commands. The first one which specify a local branch is recommended (replace pr-id with the id of pr):

```
git fetch origin pull/pr-id/head:BRANCHNAME
git fetch origin pull/pr-id/head
```
Then you can switch to new branch using `git checkout BRANCHNAME`. then run the following command to update the branch in the future:
```
git pull origin pull/pr-id/head
```
* `git merge-base` finds best common ancestor(s) between two commits to use in a three-way merge. One common ancestor is better than another common ancestor if the latter is an ancestor of the former. For more information visit this [site](https://git-scm.com/docs/git-merge-base).
* You can use `gitk` for interactively select from a file listing to see the differences. Here are some examples:
```
gitk --all
gitk master..topic
```
* See only commits which are submitted to a topic branch:
```
git log master..topic
```
* See all changes in a topic branch:
```
git difftool master...topic
```

* For GUI you can use `gitk, git gui, qgit, gitg`.
* For viewing a file in another branch without changing branch, use the following command:
```
git show branch:file
```
In Windows change `\` to `/`

