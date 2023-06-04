---
title: git-learn
date: 2021-12-30 11:51:31
tags: git
categories: Basic
cover: https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/eyecatch_git-960x504.png
---

# The lifecycle of the status of files

![The lifecycle of the status of your files](https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/202112301154791.png)

# git add

- It may be helpful to think of it more as “add precisely this content to the next commit” rather than “add this file to the project”.
- f you modify a file after you run `git add`, you have to run `git add` again to stage the latest version of the file.

![image-20211230120957213](https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/202112301209255.png)

# Ignoring Files

## Rules

- Blank lines or lines starting with `#` are ignored.
- Standard glob patterns work, and will be applied recursively throughout the entire working tree.
- You can start patterns with a forward slash (`/`) to avoid recursively.
- You can end patterns with a forward slash (`/`) to specify a directory.
- You can negate a pattern by starting it with an exclamation point (`!`).

## Example

![image-20211230122626631](https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/202112301226686.png)

- `*~` tells Git to ignore all files whose names end with a tilde(~).
- `*.[oa]` tells Git to ignore any files ending in ".o" o ".a"
- `!` but do track `lib.a`, even though you're ignoring .a files above
- a question mark (`?`) matches a single character
- `a/**/z` would match `a/z`, `a/b/z`, `a/b/c/z`, and so on.

# git mv

```shell
mv README.md README
git rm README.md
git add README
```

# [git log](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)

```shell
git log -S function_name
```

if you wanted to find the last commit that added or removed a reference to a specific function, you could call `git log -S function_name`

## git log --pretty

```shell
$ git log --pretty=format:"%h %s" --graph
* 2d3acf9 Ignore errors from SIGCHLD on trap
*  5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
|\
| * 420eac9 Add method for getting the current branch
* | 30e367c Timeout code and tests
* | 5a09431 Add timeout protection to grit
* | e1193f8 Support for heads with slashes in them
|/
* d6016bc Require time for xmlschema
*  11d191e Merge branch 'defunkt' into local
```

# git fetch vs git pull

- git fetch is a command that allows you to download objects from another repository.
  - download files not available locally
  - make no change to local files
  - help you get all information you need
  - a safer alternative
- git pull is a command that allow you to fetch and integrate with another repository or local branch
  - a faster alternative

# git rebase

