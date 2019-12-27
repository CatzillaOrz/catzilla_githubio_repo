---
title: Shell 助力开发效率提升
date: 2019-12-27 11:26:13
tags:
    - Shell
    - tips
categories: Shell
---

- 通过本文的介绍, 你应该对相关命令有一个初步的了解, 知道比如用什么命令可以完成怎样的操作,
- 至于具体的参数, 你不用去刻意地记, 等到你用到的时候, 你再去 cmd --help 或者 man cmd去看, 用熟悉了, 常用的你也就记住了.

- 本文首先介绍了Linux/Mac下一些常用的命令行工具, 然后介绍了一些常用的命令, 最后通过一两个案例来说明这些工具的强大之处:
  - 比如给定一个nginx日志文件, 能够找出HTTP 404 请求最多的top 10 是什么? 比如能找到请求耗时最多的top 10是什么? 再比如能够简单的得到每小时的”PV”是多少?
  - 再比如拿到一篇文章, 能否简单统计一下这篇文章单次词频最高的10个词语是什么?

###### Mac 环境

- zsh
- on-my-zsh
- plugin
  - git
  - autojump
  - osx(man-preview/quick-look/pfd(print Finder director)/cdf(cd Finder))
- 常用快捷键(bindkey)
- 演示: 高亮/git/智能补全/跳转(j,d)…

###### Shell 基础命令

- which/whereis, 常用 whatis, man, --help

    ```shell
    ➜  .oh-my-zsh git:(master)$ whereis ls
    /bin/ls
    ➜  .oh-my-zsh git:(master)$ which ls
    ls: aliased to ls -G

    ```

- 基本文件目录操作

    ```shell
    rm, mkdir, mv, cp, cd, ls, ln, file, stat, wc(-l/w/c), head, more, tail, cat...
    ```

- 利器 管道: |

##### Shell 文本处理

这里就是通过案例讲了一下12个命令的大致用法和参数, 可以通过点击右边的目录直达你想要了解的命令.

```shell
find, grep, xargs, cut, paste, comm
join, sort, uniq, tr, sed, awk
```

###### find

- 常用参数
  - 文件名 `-name`, 文件类型`-type`, 查找最大深度`-maxdepth`
  - 时间过滤(create/access/modify) `-[cam]time`
  - 执行动作 `-exec`

- 示例

    ```shell
    find ./ -name "*.json"
    find . -maxdepth 7 -name "*.json" -type f
    find . -name "*.log.gz" -ctime +7 -size +1M -delete (atime/ctime/mtime)
    find . -name "*.scala" -atime -7 -exec du -h {} \;
    ```

###### grep

- 常用参数

    ```shell
    -v(invert-match),
    -c(count),
    -n(line-number),
    -i(ignore-case),
    -l, -L, -R(-r, –recursive), -e
    ```

- 示例

    ```shell
    grep 'partner' ./*.scala -l
    grep -e 'World' -e 'first' -i -R ./  (-e: or)
    ```

- 相关命令: `grep -z / zgrep / zcat xx | grep`

###### cut

- 常用参数

    ```shell
    -b(字节)
    -c(字符)
    -f(第几列), -d(分隔符), f范围: n, n-, -m, n-m
    ```

- 示例

```shell
echo "helloworldhellp" | cut -c1-10
cut -d, -f2-8 csu.db.export.csv
```

###### paste

- 常用参数

    ```shell
    -d 分隔符
    -s 列转行
    ```

- 示例

    ```shell
    ➜  Documents$ cat file1
    1 11
    2 22
    3 33
    4 44
    ➜  Documents$ cat file2
    one     1
    two     2
    three   3
    one1    4

    ➜  Documents$ paste -d, file1 file2
    1 11,one     1
    2 22,two     2
    3 33,three   3
    4 44,one1    4
    ➜  Documents$ paste -s -d: file1 file2
    a 11:b bb:3 33:4 44
    one     1:two     2:three   3:one1    4

    ```

###### join

- 类似sql中的 `...inner join ...on ...`, `-t` 分隔符, 默认为空格或tab

    ```shell
    ➜  Documents$ cat j1
    1 11
    2 22
    3 33
    4 44
    5 55
    ➜  Documents$ cat j2
    one     1   0
    one     2   1
    two     4   2
    three   5   3
    one1    5   4
    ➜  Documents$ join -1 1 -2 3 j1 j2
    1 11 one 2
    2 22 two 4
    3 33 three 5
    4 44 one1 5
    ```

> to be continued
