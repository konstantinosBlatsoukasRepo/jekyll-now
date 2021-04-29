---
layout: post
title: git merge, by example
---

Let's see what git merge does by example.

## repository before merging

Current commits at master branch:

```sh
 git log --graph
* commit 2978d2520bd045dff823749bf298187bc8ecdd3e (HEAD -> master)
| Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
| Date:   Thu Apr 29 09:25:43 2021 +0100
|
|     some content added to my_file.txt
|
* commit ecc0748a3cdf08d62a43a4277adf3dc105698990
  Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
  Date:   Thu Apr 29 09:24:27 2021 +0100

      my_file.txt added
```

## new branch from master branch

Let's create a new branch from master branch, named feature.

```sh
git checkout -b feature
```

repository changed to:

```sh
* commit 2978d2520bd045dff823749bf298187bc8ecdd3e (HEAD -> feature, master)
| Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
| Date:   Thu Apr 29 09:25:43 2021 +0100
|
|     some content added to my_file.txt
|
* commit ecc0748a3cdf08d62a43a4277adf3dc105698990
  Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
  Date:   Thu Apr 29 09:24:27 2021 +0100
```

both branches are pointing to the same commit.

## new commit to feature branch

Add a file in the feature branch and commit the change

```sh
touch feature_file.txt
git add feature_file.txt
git commit -m "feature_file.txt added"
```

repository changed to:

```sh
* commit 16d74286bcecdff666bd916c5f977434e92711ba (HEAD -> feature)
| Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
| Date:   Thu Apr 29 09:34:54 2021 +0100
|
|     feature_file.txt added
|
* commit 2978d2520bd045dff823749bf298187bc8ecdd3e (master)
| Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
| Date:   Thu Apr 29 09:25:43 2021 +0100
|
|     some content added to my_file.txt
|
* commit ecc0748a3cdf08d62a43a4277adf3dc105698990
  Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
  Date:   Thu Apr 29 09:24:27 2021 +0100

      my_file.txt added
```

the feature branch is one commit above the master

## merge feature to master

Change branch to master and merge feature

```sh
git checkout master
git merge feature
```

repository changed to:

```sh
* commit 16d74286bcecdff666bd916c5f977434e92711ba (HEAD -> master, feature)
| Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
| Date:   Thu Apr 29 09:34:54 2021 +0100
|
|     feature_file.txt added
|
* commit 2978d2520bd045dff823749bf298187bc8ecdd3e
| Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
| Date:   Thu Apr 29 09:25:43 2021 +0100
|
|     some content added to my_file.txt
|
* commit ecc0748a3cdf08d62a43a4277adf3dc105698990
  Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
  Date:   Thu Apr 29 09:24:27 2021 +0100
```

Master branch just moved one commit, and points to same commit as the feature branch.
This happened, because branch were not diverged (e.g. no new commits are added to master).
This style of merging is called *fast forward merge*, no actual merging is happening.

## merge with no-ff option

However you can force the creation of a new commit when merging, by specifying the option --no-ff.
Imagine that we were in the previous state of the repository:

```sh
* commit 16d74286bcecdff666bd916c5f977434e92711ba (HEAD -> feature)
| Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
| Date:   Thu Apr 29 09:34:54 2021 +0100
|
|     feature_file.txt added
|
* commit 2978d2520bd045dff823749bf298187bc8ecdd3e (master)
| Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
| Date:   Thu Apr 29 09:25:43 2021 +0100
|
|     some content added to my_file.txt
|
* commit ecc0748a3cdf08d62a43a4277adf3dc105698990
  Author: Kostas Blatsoukas <k.blatsoukas@my.tech>
  Date:   Thu Apr 29 09:24:27 2021 +0100

      my_file.txt added
```

Now we want to merge both branches with creating a new commit.

```sh
git checkout master
git merge --no-ff feature
```

The second command above, tells to git not to use the fast forward algorithm (--no-ff)

repository cahnged to:

```sh
*   commit d4c7e8510c7fa9d730b5919ad617ee7f8468dc51 (HEAD -> master)
|\  Merge: 2978d25 16d7428
| | Author: Kostas Blatsoukas <k.blatsoukas@camlin.tech>
| | Date:   Thu Apr 29 10:03:35 2021 +0100
| |
| |     Merge branch 'feature'
| |
| * commit 16d74286bcecdff666bd916c5f977434e92711ba (feature)
|/  Author: Kostas Blatsoukas <k.blatsoukas@camlin.tech>
|   Date:   Thu Apr 29 09:34:54 2021 +0100
|
|       feature_file.txt added
|
* commit 2978d2520bd045dff823749bf298187bc8ecdd3e
| Author: Kostas Blatsoukas <k.blatsoukas@camlin.tech>
| Date:   Thu Apr 29 09:25:43 2021 +0100
|
|     some content added to my_file.txt
|
* commit ecc0748a3cdf08d62a43a4277adf3dc105698990
  Author: Kostas Blatsoukas <k.blatsoukas@camlin.tech>
  Date:   Thu Apr 29 09:24:27 2021 +0100

      my_file.txt added
```

new commit created with commit message that the feature branch merged.

Cheers!
