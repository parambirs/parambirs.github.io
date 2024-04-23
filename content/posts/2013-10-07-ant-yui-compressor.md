---
title: 'Ant task for minifying JS/CSS files using YUI Compressor'
date: 2013-10-07T00:00:00-07:00
tags: ["ant", "yui", "javascript", "css"]
summary: "Created an open-source Ant library for JS/CSS minification, incorporating YUI Compressor."
---

Recently, while working on a project, I had the need for minifying a set of Javascript and CSS files. Although the project was in HTML5 with no server side component, I had set up the build process using Apache Ant.

I looked online for an ant library that could accomplish this and came across one that was almost exactly what I was looking for. It could minify JS files using [YUI Compressor](http://yui.github.io/yuicompressor/) but lacked a couple of features I required. So, I went through the library’s source code and added the following features

- Minification option for CSS files
- Option to delete original source files after minification

Since other people might want to use these additional features, I’ve made the modified library open source on GitHub:

https://github.com/parambirs/ant-yui-compressor

The [README](https://github.com/parambirs/ant-yui-compressor/blob/master/README.md) file describes the available options and how to use them. There’s also an [example](https://github.com/parambirs/ant-yui-compressor/tree/master/example) folder with a working ant project for JS/CSS minification. My code is based on the [yui-compressor-ant-task](https://code.google.com/p/yui-compressor-ant-task/) hosted on google code.