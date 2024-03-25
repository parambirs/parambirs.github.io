---
title: 'scalac -feature'
date: 2020-06-27T00:00:00-07:00
draft: true
tags: ["scala", "scalac"]
summary: "Use scalac's -feature option to emit warnings for features that should be imported explicitly."
---

A useful option to turn on for the scala compiler is `-feature`. It emits warning and location for usages of features that should be imported explicitly.

An example such an advanced feature is implicit conversions. An implicit conversion is an implicit value of type `A => B`. e.g.

```scala
def main(args: Array[String]): Unit = {
    val s = "Hello, world!"
    val len: Int = s
    println(len)
}

implicit def stringToInt(s: String): Int = s.length
```

Compiling this piece of code with the `-feature` scalac flag enabled gives the following warning:
```
[warn] Main.scala:8:16: implicit conversion method stringToInt should be enabled
[warn] by making the implicit value scala.language.implicitConversions visible.
[warn] ----
[warn] This can be achieved by adding the import clause 'import scala.language.implicitConversions'
[warn] or by setting the compiler option -language:implicitConversions.
[warn] See the Scaladoc for value scala.language.implicitConversions for a discussion
[warn] why the feature should be explicitly enabled.
[warn]   implicit def stringToInt(s: String): Int = s.length
[warn]                ^
[warn] one warning found
```