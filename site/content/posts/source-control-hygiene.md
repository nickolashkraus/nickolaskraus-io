+++
title = 'Source Control Hygiene'
date = 2023-03-23T00:00:00-00:00
lastmod = 2025-08-22T00:00:00-00:00
draft = false
description = '''
My thoughts on being a good steward of source control.
'''
tags = ['programming', 'git']
aliases = ['/articles/source-control-hygiene/']
+++

Contributing to open-source is analogous to using someone's home. You don't
want to come in and make a mess of the place. Similarly, you don't want to
muddle a concise and well-maintained commit history with uninformative and
messy commits. The following provides strategies for maintaining a clean,
hygienic commit history, so that your code and your pull requests are
professional.

## Commit Messages

`WIP` is not an appropriate commit message. That also applies to duplicate
messages when you troubleshoot an issue. Your commit messages should follow
a standard format and be informative. For example,

```gitcommit
<issue> - <issue description>

- Added exception handling in function 'foo'
```

Here, `<issue>` can correspond to the GitHub issue number. Alternatively,
`<issue>` can be an issue type (e.g., `docs`, `feature`, `bug`).

The Git commit message should adhere to the following conventions:

1. **Subject line** (first line):
    - **Line length**: 50 characters
    - **Style**: Capitalized, imperative mood (e.g., *Fix bug in user login
      flow*)
    - **No punctuation** at the end (e.g., no period)

2. **Blank line** between subject and body

3. **Body** (optional):
    - **Line length**: 72 characters (wrap lines)
    - Explain *what* and *why*, not *how*

A project may use [Conventional Commits][Conventional Commits], in which case
you should adhere to the specification.

## Squashing Commits

You should not submit a pull request with more than one commit unless each
commit contains a logical grouping of changes. A pull request should be an
atomic package of work that can be summarized by a single commit message. If
you commit often during development, use:

```bash
$ git rebase -i <commit-SHA>
```

## Don't Merge Master

Do not merge master into a branch in order to integrate upstream changes.
Instead, use `git rebase`. Given that your local reference is up-to-date with
upstream remotes, executing `git rebase master` will integrate upstream changes
and place your commits on top.

[Conventional Commits]: https://www.conventionalcommits.org
