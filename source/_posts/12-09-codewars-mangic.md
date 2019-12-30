---
layout: codewars
title: codewars mangic
date: 2019-12-09 15:06:32
tags:
    - codewars
categories: codewars
---

##### javascript - 7 kyu Love vs friendship

- 7 kyu Love vs friendship
<!-- more -->

```js
If　a = 1, b = 2, c = 3 ... z = 26

Then l + o + v + e = 54

and f + r + i + e + n + d + s + h + i + p = 108

So friendship is twice stronger than love :-)

The input will always be in lowercase and never be empty.
```

- solution:

```js
const wordsToMarks = s => [...s].reduce((res, c) => res += c.charCodeAt() - 96, 0)

```

#### shell - 8kyu Even or Odd

- 8kyu Even or Odd

```shell

#!/bin/bash
# your code here

(( $1 & 1 )) && echo "Odd" || echo "Even"

```

#### shell - 8kyu Opposite number

- 8kyu Opposite number

```shell

#!/bin/bash
echo "- $1" | bc


```
