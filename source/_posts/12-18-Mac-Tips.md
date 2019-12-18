---
title: Mac Tips
date: 2019-12-18 11:12:58
tags:
    - Mac
    - Tips
categories: Mac
---

## Mac 软件推荐(续)之程序猿篇

### 快捷键

```bash
空格键: 预览
cmd + ,: 设置
cmd + -/=: 缩小/放大
ctrl + u: 删除到行首(与zsh冲突, zsh中是删除整行)
ctrl + k: 删除到行尾
ctrl + p/n: 上/下移动一行或者前/后一个命令
ctrl + b/f: 光标前/后移char
esc + b/f: 光标前/后移word(蛋疼不能连续work)
ctrl + a/e: 到行首/行尾
ctrl + h/d: 删前/后字符
ctrl + y: 粘贴
ctrl + w: 删除前一个单词
esc + d: 删后一个单词
ctrl + _: undo
ctrl + r: bck-i-search/reverse-i-search, 输入关键字搜索历史命令
cmd + shift + 3 截取整个屏幕.
cmd + shift + 4 部分窗口, 出现十字供选取, 若此时按空格键(这个技能得点赞), 会选取当前应用的窗口, 再 tap 即可完成截图.
```

Mac 内置的更多的快捷键列表可以参考 [Mac 官网](https://support.apple.com/zh-cn/HT201236)

### iTerm2

[iTerm2](http://www.iterm2.com/features.html)官网有介绍功能. 以下是觉得可能常用的功能.

- 分屏功能
  - cmd + d 竖着分屏, cmd + shift + d 横着分屏
  - cmd + t 新建一个 tab, cmd + num 切换到第 num 个 tab
  - 当前窗口含有分屏时, 通过 cmd + [ 和 cmd + ] 来进行切换小的分屏
- 热键 设置一个热键, 比如我的是 alt + 空格, 弹出 iTerm2, 且以半透明的方式显示在当前 active 的窗口上面.

### zsh

这个墙裂推荐啊. 结合 [oh my zsh](http://ohmyz.sh/), 丰富的[插件资源](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins-Overview).

语法高亮, 自动补全等特别好, 在此推荐的几个插件或功能.

- git
- autojump
- osx
- zsh-autosuggestions

### Vim

[所需即所获：像 IDE 一样使用 vim](https://github.com/yangyangwithgnu/use_vim_as_ide)
