---
title: 'Execution time in Firebug'
date: 2011-12-13T00:00:00-07:00
tags: ["profiling", "javascript"]
summary: "Profiling JavaScript code in Firebug"
---

```js
function populateData(){
    console.time('populateData timer');
    /////
    ///// Code Here
    /////
    console.timeEnd('populateData timer');
}
````

This will print something like this in firebug: `populateData timer: 30ms`
