---
title: 'Creating singleton collections'
date: 2010-03-04T00:00:00-07:00
tags: ["java"]
summary: "Collections class provides static methods for immutable singleton collections (List, Map, Set)."
---

The [Collections](http://java.sun.com/j2se/1.4.2/docs/api/java/util/Collections.html) class in the `java.util` package provides convenient methods to create singleton collection objects. A singleton collection object is a `List`, `Map` or `Set` that contains only a single element. Here’s how you can create a singleton set:

```java
Set set = Collections.singleton( "The Element" );
```

There are 2 more methods which allow you to create a singleton list or map:

```java
List list = Collections.singletonList( "The Element" );
Map map = Collections.singletonMap( "Key" , "Value" );
```

The collection object returned in all these methods is immutable. Also, all 3 of them are `static` methods in the `Collections` class.