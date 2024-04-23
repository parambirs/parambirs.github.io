---
title: 'Ubuntu file manager with root privileges'
date: 2011-03-23T00:00:00-07:00
tags: ["linux", "ubuntu"]
summary: "Use nautilus-gksu to easily open a folder as root user."
---

Many a times, you need to move/copy files into folders for which you need root access in Linux. Normally, you have to go through the terminal to accomplish this (using `sudo`). However, you can install the `nautilus-gksu` package which adds an "Open as Administrator" option to the right click menu in Nautilus (the file manager in Ubuntu). To do this, enter the following command in terminal:

```
sudo apt-get install nautilus-gksu
```

You will need to log out and log in again to make this work!
