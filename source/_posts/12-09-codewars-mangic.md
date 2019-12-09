---
layout: codewars
title: codewars mangic
date: 2019-12-09 15:06:32
tags:
    - codewars
categories: codewars
---

## codewar

### 7 kyu Love vs friendship
<!-- more -->

```js
If　a = 1, b = 2, c = 3 ... z = 26

Then l + o + v + e = 54

and f + r + i + e + n + d + s + h + i + p = 108

So friendship is twice stronger than love :-)

The input will always be in lowercase and never be empty.
```

```js
const wordsToMarks = s => [...s].reduce((res, c) => res += c.charCodeAt() - 96, 0)

```
