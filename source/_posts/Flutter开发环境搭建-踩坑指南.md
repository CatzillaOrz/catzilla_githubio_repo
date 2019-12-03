---
title: Flutter开发环境搭建-踩坑指南
tags:
  - Flutter
categories:
  - Frontend
layout: flutter
date: 2019-09-16 12:09:30
---

![logo](https://pub.dartlang.org/static/img/flutter-packages.png)

## Flutter开发环境搭建-踩坑指南

相信有不少同学，一开始就被环境搭建恶心到，从而放弃。或者开始抱怨各种坑多，不愿再尝试。

很多同学可能看了太多关于Flutter的文章，都很想上手，但大多数都死在了搭建环境这一步。没关系，今天就来点不一样的，带你们从0开始，搭建你的Flutter环境。

这种文章本来没有必要出，但既然坑都踩了，方便大家搜索快速定位问题，就特意记录下各种深坑，也是为了方便大家快速定位解决问题，欢迎大家围观。

## Get started: install

1. [各个操作平台需要下载的Flutter](https://flutter.io/get-started/install/)
    - 坑位
        1. 打不开（你懂的）
        2. 环境配置
        3. 下完了，该干啥
        4. 选择编辑器
<!-- more -->
    - 填坑
        1. 此处可能需要科学上网`提醒: 中国的开发者们，请先看一下这篇` [wiki](https://github.com/flutter/flutter/wiki/Using-Flutter-in-China)，`查看是否需要对网络环境进行特别设置`。需要注意的是，其实这一步才是死的人最多的吧。后面大段描述下各种深坑。
        2. 此处的坑，可能就是一劳永逸的命令行环境变量配置，Mac例子： `open .bash_profile`点到为止。
        3. 甩出中文社区教程：[编写你的第一个 Flutter App](https://codelabs.flutter-io.cn/codelabs/first-flutter-app-pt1-cn/index.html#1)
        4. 编辑器当然选择**vs code**，谁用谁知道，[Configure Editor](https://flutter.io/get-started/editor/#vscode)请跟随教程指引，下载安装vs code 所需要的**插件**。

2. Install on macOS
    - 甩出官网安装指南 [macOS](https://flutter.io/setup-macos/)
    1. 全文一开头就告诉你要用到的一些东西和命令。(以下都是命令行操作，跟着教程敲就可以)
    1. 他建议你安装在 ~/development
    1. 他建议你下载成功后设置PATH，这块是零时的，你可以试着玩一下，后面直接在 cd ~ 后键入`open .bash_profile`打开后，把你这段 `export PATH=~/development/flutter/bin:$PATH`加入， 这块的 `~/development/flutter/bin`这段就是你解压后的真实路径，很好找吧？除非你忘了你解压到哪里了- -！
    1. 如果。以上几步你都顺利完成了，接下来键入 `flutter doctor`检查下你还缺少哪些，然后痛苦的install各种失败在向你招手...(后面继续讲，是哪些错误)

3. You need a doctor
    - 好不容易Flutter也下载好了，环境PATH也设置好啦，doctor，接着说，同学，你的Flutter还需要治疗。然后抛出以下体检结果：
    - For example:

        ```bash
        Doctor summary (to see all details, run flutter doctor -v):
        [✓] Flutter (Channel beta, v0.5.1, on Mac OS X 10.13.6 17G65, locale zh-Hans-CN)
        [!] Android toolchain - develop for Android devices
            ✗ Android SDK is missing command line tools; download from https://goo.gl/XxQghQ
        [!] iOS toolchain - develop for iOS devices
        ✗ Xcode installation is incomplete; a full installation is necessary for iOS development.
        Download at: https://developer.apple.com/xcode/download/
        Or install Xcode via the App Store.
        Once installed, run:
            sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
        ✗ ideviceinstaller is not installed; this is used to discover connected iOS devices.
        To install, run:
            brew install --HEAD libimobiledevice
            brew install ideviceinstaller
        ✗ ios-deploy not installed. To install:
            brew install ios-deploy
        ✗ CocoaPods not installed. CocoaPods is used to retrieve the iOS platform side's plugin code that your plugin usage on the Dart side.Without resolving iOS dependencies with CocoaPods, plugins will not work on iOS. For more info, see https://flutter.io/platform-plugins To install: brew install cocoapods pod setup
        ```

    - 好在，不管你遇到什么，医生都会告诉你如何用指令安装:

        ```bash
        brew update
        brew install --HEAD libimobiledevice
        brew install ideviceinstaller ios-deploy cocoapods
        pod setup
        ```

        1. 你就每次按医生说的下载就行了。
        1. 如果你以为能一帆风顺那你就大错特错了...

4. Error:
    - 大多数时候会报这个错：

        ```bash
        remote: Counting objects: 8152, done.
        remote: Compressing objects: 100% (5441/5441), done.
        error: RPC failed; curl 18 transfer closed with outstanding read data remaining
        fatal: The remote end hung up unexpectedly
        fatal: early EOF
        fatal: index-pack failed
        Error: Failed to download resource "libimobiledevice"
        Failure while executing; `git clone --branch master https://git.libimobiledevice.org/libimobiledevice.git /Users/~/Library/Caches/Homebrew/libimobiledevice--git` exited with 128.
        ```

    - 心里一万只神兽喷涌而过。下载100% done ？ 然后跟我报错？
    - 丢出各种搜索到的issue：
        1. [error: RPC failed; curl 56 OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 54](https://github.com/CocoaPods/CocoaPods/issues/7025)
        1. [error: RPC failed; curl 56 LibreSSL SSL_read: SSL_ERROR_SYSCALL, errno 54 fatal: The remote end hung up unexpectedly fatal: early EOF fatal: index-pack failed](https://www.jianshu.com/p/63ad42949061)
        1. [RPC failed; curl 56 LibreSSL SSL_read: SSL_ERROR_SYSCALL, errno 54](https://www.jianshu.com/p/803df93e84ad)
    - 偶尔出现这种错：

    ```bash
    Updating Homebrew...
    ==> Cloning https://git.libimobiledevice.org/libimobiledevice.git
    Cloning into '/Users/~/Library/Caches/Homebrew/libimobiledevice--git'...
    remote: Counting objects: 8152, done.
    remote: Compressing objects: 100% (5441/5441), done.
    error: RPC failed; curl 56 LibreSSL SSL_read: SSL_ERROR_SYSCALL, errno 54
    fatal: The remote end hung up unexpectedly
    fatal: early EOF
    fatal: index-pack failed
    Error: Failed to download resource "libimobiledevice"
    Failure while executing; `git clone --branch master https://git.libimobiledevice.org/libimobiledevice.git /Users/~/Library/Caches/Homebrew/libimobiledevice--git` exited with 128.
    ```

    1. [git error: RPC failed; curl 56 LibreSSL SSL_read: SSL_ERROR_SYSCALL, errno 54](https://github.com/lanlin/notes/issues/41)
    1. [RPC failed; curl 18 transfer closed with outstanding read data remaining](https://blog.csdn.net/IT_liuchengli/article/details/77040806)

5. You need calm down

    - 以上错误，不知说你一定会遇到，只是有可能会遇到。
    - 也不是特指以上issue里的某个依赖代码，拉取代码失败，因为你可能在不同的情况遇到类似的问题。
    - 所以，答案也许就在上面这些链接中，但这里也会给你一些建议，若遇到类似报错，不妨尝试

    ```bash
    git config http.postBuffer 524288000
    git config https.postBuffer 524288000
    ```

    - 如果你试了以上办法真的，还是下载失败。最后的秘诀当然不是，重启电脑，应该是`多试几次`
    - **多试几次**
    - **多试几次**
    - **多试几次**
    - 意思就是**多执行几次** brew install让他多去 pull 几次 github的 master
    - 为什么不是 pip 或者 ruby，因为真的没遇到过这俩个出问题，几乎都是brew
    - 说了那么多，最后还是在执行了 `brew install --HEAD libimobiledevice` 13次？还是23次，后终于自动成功了。ᔪ(⁰́◊⁰̀)ᔭ OMG

## Finally

1. 这里提到的只是印象比较深刻的一些，你会发现很多类似的issue里很多人都遇到了此类问题，然而每个人都解决方法也不同。你会发现，有些issue依然开着，下面都回复也是络绎不绝。所以，不放弃，才是最后的解决方案，加油。
1. 看到这里，真是被现实打败了。 光是环境就把人折磨的精疲力尽。但是，如果，这道坎你都过了，那后面还不是水到渠成的事吗！（逃
1. 后续还会继续更新，共勉
1. [Flutter官网教程](https://flutter-io.cn/#section-codelabs)
1. [Flutter中文网](https://flutterchina.club/)
