---
title: 'Grep recursively for text'
date: 2012-05-25T00:00:00-07:00
tags: ["unix", "shell", "grep"]
summary: "Search recursively for text in all files in the current directory"
---

Here’s a unix command to search for lines containing a particular text in all the files recursively in the current directory:

```sh
find . | xargs grep text-to-find
```
