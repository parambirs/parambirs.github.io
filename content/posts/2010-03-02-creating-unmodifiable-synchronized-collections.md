---
title: 'Creating unmodifiable & synchronized collections'
date: 2010-03-02T00:00:00-07:00
tags: ["java"]
summary: ""
---

By default, the classes available in the Java Collections framework are read/write enabled & not thread safe. Sometimes the requirement is to have a read only i.e. unmodifiable collection. The [Collections](http://java.sun.com/j2se/1.4.2/docs/api/java/util/Collections.html) class available in the `java.util` package provides utility methods to create such collections. Note: Do not confuse this class with the `Collection` interface which is the root interface of the collection hierarchy.

The objects returned by these methods are wrappers over the original collection object you supply to the method ([Decorator design pattern](http://en.wikipedia.org/wiki/Decorator_pattern)).

The `Collections` class has 6 `static` methods that return a read only collection object. These are:

```java
static Collection unmodifiableCollection(Collection c)
static List unmodifiableList(List list)
static Map unmodifiableMap(Map m)
static Set unmodifiableSet(Set s)
static SortedMap unmodifiableSortedMap(SortedMap m)
static SortedSet unmodifiableSortedSet(SortedSet s)
```

It is important that you don’t store the reference to the original object, otherwise you’ll still be able to access the original object and thus the collection will not be unmodifiable. Therefore, it is best to first populate the collection with the desired values and then replace the original reference with the read-only reference e.g.

```java
List list = new Arraylist();
// ... populate the list
list = Collections.unmodifiableList(list);
```

The classes available in the collections package are not thread safe by default. The designers of the Java collections hierarchy did this on purpose since synchronized collection classes are slower and you don’t require synchronization always. The `Collections` class has 6 utility static methods to wrap collection objects into synchronized ones. These methods are:

```java
static Collection synchronizedCollection(Collection collection)
static List synchronizedList(List list)
static Map synchronizedMap(Map map)
static Set synchronizedSet(Set set)
static SortedMap synchronizedSortedMap(SortedMap map)
static SortedSet synchronizedSortedSet(SortedSet set)
```

It is best to create a collection object and then wrap it in a synchronized one immediately. Also, you should not keep the reference to the original object otherwise you’ll be able to access it in a thread-unsafe manner. The best way to do this is to create a synchronized collection object as follows:

```java
List list = Collections.synchronizedList(new ArrayList());
```
