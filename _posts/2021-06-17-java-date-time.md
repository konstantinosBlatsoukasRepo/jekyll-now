---
layout: post
title: java date and time
---

Basics of java date and time (java 8).

## Classes in java (since jdk 8)

The following classes are not taking into consideration the timezone.
All the classes are compliant to ISO-8601 (e.g. the gregorian calendar).


- LocalDate
- LocalTime
- LocalDateTime


## Parsing date/time

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