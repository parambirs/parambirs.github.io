---
title: 'How to swap two integers in a single line of code?'
date: 2008-01-20T00:00:00-07:00
tags: ["algorithm"]
summary: ""
---

This question was asked during IBM’s interview in our campus in 2005:

> How would you swap two integers in one statement without using a temporary variable in C/C++?

After thinking about it for some time, this is what I got (this is in Java):

```java
class Swap
{
    public static void main( String[] args )
    {
        int a, b;
        a = 46;
        b = 47;
        System.out.println( "a = " + a + ", b = " + b );
        a = b | ( 0 & (b = a));
        System.out.println( "a = " + a + ", b = " + b );
    }
}
```

However, the interviewer gave a hint that it requires the use of XOR operator. If any one of you can think of a solution using XOR, please post your solution here.
