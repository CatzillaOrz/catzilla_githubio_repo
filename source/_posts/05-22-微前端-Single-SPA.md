---
title: 微前端-Single-SPA
date: 2020-05-22 16:38:53
tags:
  - SPA
categories:
  - SPA
---

###### JavaScript 微前端

Single-spa 是一个将多个单页面应用聚合为一个整体应用的 javascript 微前端框架。 使用 single-spa 进行前端架构设计可以带来很多好处，例如:

- 在同一页面上使用多个前端框架 而不用刷新页面 (React, AngularJS, Angular, Ember, 你正在使用的框架)
- 独立部署每一个单页面应用
- 新功能使用新框架，旧的单页应用不用重写可以共存
- 改善初始加载时间，迟加载代码

###### 架构概览

Single-spa 从现代框架组件生命周期中获得灵感，将生命周期应用于整个应用程序。 它脱胎于 Canopy 使用 React + React-router 替换 AngularJS + ui-router 的思考，避免应用程序被束缚。现在 single-spa 几乎支持任何框架。 由于 JavaScript 因其许多框架的寿命短而臭名昭著，我们决定让它在任何您想要的框架都易于使用。

Single-spa 包括以下内容:

1. Applications，每个应用程序本身就是一个完整的 SPA (某种程度上)。 每个应用程序都可以响应 url 路由事件，并且必须知道如何从 DOM 中初始化、挂载和卸载自己。 传统 SPA 应用程序和 Single SPA 应用程序的主要区别在于，它们必须能够与其他应用程序共存，而且它们没有各自的 html 页面。

例如，React 或 Angular spa 就是应用程序。 当激活时，它们监听 url 路由事件并将内容放在 DOM上。 当它们处于非活动状态时，它们不侦听 url 路由事件，并且完全从 DOM 中删除。

1. 一个 single-spa-config配置, 这是html页面和向Single SPA注册应用程序的JavaScript。每个应用程序都注册了三件东西

- A name
- A function (加载应用程序的代码)
- A function (确定应用程序何时处于活动状态/非活动状态)

###### 推荐设置

- single-spa 核心团队已经汇总了文档，工具和视频，展示了当前使用single-spa鼓励的最佳实践。 查看[这些文档](https://zh-hans.single-spa.js.org/docs/recommended-setup)以获取更多信息。

###### The Recommended Setup

- Alternatives
  - [qiankun](https://github.com/umijs/qiankun) is a popular alternative to this recommended setup.
  - [Isomorphic Layout Composer](https://github.com/namecheap/ilc) - complete solution for Micro Frontends composition into SPA with SSR support

- In-browser versus build-time modules

Tutorial video: [Youtube](https://www.youtube.com/watch?v=Jxqiu6pdMSU&list=PLLUD8RtHvsAOhtHnyGx57EYXoaNsxGrTU&index=2) / [Bilibili](https://www.bilibili.com/video/av83498486/)

###### READMORE

- [examples](https://zh-hans.single-spa.js.org/docs/examples)
- [Video tutorials](https://zh-hans.single-spa.js.org/docs/videos)
- [single-spa 生态系统](https://zh-hans.single-spa.js.org/docs/ecosystem)
- [微前端在小米CRM系统的实践](https://mp.weixin.qq.com/s/jz4EthRMt-kMEMFWPzcNow)
