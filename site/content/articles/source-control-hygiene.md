---
title: "Source Control Hygiene"
date: 2023-03-23T00:00:00-06:00
draft: false
description: My thoughts on being a good steward of source control.
tags: ["programming", "git"]
---

Contributing to open-source is analogous to using someone's home. You don’t want to come in and make a mess of the place. Similarly, you don’t want to muddle a concise and well-maintained commit history with uninformative and messy commits. The following provides strategies for maintaining a clean, hygienic commit history, so that your code and your pull requests are professional.

## Commit Messages

`WIP` is not a suitable commit message. That goes for duplicate messages as you troubleshoot an issue as well. Your commit message or messages should be cohesive and informational. For example,

```
<issue> - <issue description>

  - Added exception handling in function 'foo'
```

Here, issue can correspond to the GitHub issue number. Alternatively, issue can be an issue type (e.g. `docs`, `feature`, `bug`).

## Squashing Commits

You should not submit a pull request with more than one commit unless each commit contains a logical grouping of changes. A pull request should be an atomic package of work that can be summarized by a single commit message. If you commit often during development, use:

```
$ git rebase -i <commit-SHA>
```

## Don’t Merge Master
Do not merge master into a branch in order to integrate upstream changes. Instead, use `git rebase`. Given that your local reference is up-to-date with upstream remotes, executing `git rebase master` will integrate upstream changes and place your commits on top.
