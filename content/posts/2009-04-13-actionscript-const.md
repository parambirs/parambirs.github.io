---
title: 'Gotcha with ActionScript ‘const’'
date: 2009-04-13T00:00:00-07:00
tags: ["javascript", "typescript", "actionscript"]
summary: ""
---

ActionScript (like JavaScript) lets you access object properties using two different notations. Consider the following class

```ts
class A
{
     public const CONSTANT:int = 34;
}

var a:A = new A();
````
 

The CONSTANT property of object a can be referred using the following two formats:

- `a.CONSTANT`
- `a["CONSTANT"]`


Since CONSTANT is declared using `const`, it cannot be assigned a new value later on. So the following code doesn’t compile (as expected):

```ts
a.CONSTANT = 23;
````

However, the following code compiles and only throws an exception at run-time:

```ts
a["CONSTANT"] = 23;
```

Beware of this behavior of ActionScript when using `const` in your code!
