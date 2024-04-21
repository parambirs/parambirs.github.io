---
title: 'Downloading sources for maven and sbt project dependencies'
date: 2015-08-06T00:00:00-07:00
tags: ["maven", "scala", "sbt", "java"]
summary: "Command-line options for downloading sources for dependencies in maven and sbt."
---

My experience with Intellij for downloading sources for the dependencies hasn’t been great. It frequently fails to download the sources. I’ve tried a few (googled) suggestions but they haven’t helped. However, there’s an alternative way using the command line. The downside to this is that it will download the sources for all dependencies. But, I don’t really mind that till I start running out of space on my laptop!

### Maven

```	
$ mvn dependency:sources
```

### sbt

```	
$ sbt updateClassifiers
```
