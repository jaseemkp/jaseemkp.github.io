---
layout: post
title: "Javascript coding style basic rules."
date: 2016-12-31 00:00:00
categories: tech post
---

Some basic rules for making javascript code style very simple in ES6.

* 2 spaces – for indentation
* Single quotes for strings – except to avoid escaping
* No unused variables
* No semicolons
* Space after keywords ```if (condition) { ... }```
* Space after function name ```function name (arg) { ... }```
* Always use ```===``` instead of ```==``` – but ```obj == null``` is allowed to check ```null || undefined```.
* Always handle err function parameter in Node.js
* Use ES6 features like arrow functions, template strings, rest operator, argument spreading, generators, promises,  maps, sets, symbols, etc.
