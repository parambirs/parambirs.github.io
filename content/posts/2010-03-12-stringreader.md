---
title: 'Using StringReader to iterate over a String'
date: 2010-03-12T00:00:00-07:00
tags: ["java"]
summary: "Iterate over a String character by character."
---

StringReader makes it convenient to iterate over a `String` character by character as follows:

```java
StringReader reader = new StringReader("This is a string.");
int ch;
while((ch = reader.read()) != -1)
{
    System.out.println((char)ch);
}
```
