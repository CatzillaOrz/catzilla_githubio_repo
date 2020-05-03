---
layout: 前端
title: 从Devops看前端基建
date: 2020-05-03 12:09:43
tags:
  - 前端方案
  - Devops
categories:
  - Frontend
---

###### 从DevOps流程看前端基建

![20200503151646-2020-05-03](https://raw.githubusercontent.com/CatzillaOrz/imgcdn/master/vsc_img/20200503151646-2020-05-03.png)

---

![devops-2020-05-03](https://raw.githubusercontent.com/CatzillaOrz/imgcdn/master/vsc_img/devops-2020-05-03.png)

---

当你进入一个新团队，前端从 0 开始，怎样从DevOps的角度去提高团队效能呢？

![devops流程和工具](https://raw.githubusercontent.com/CatzillaOrz/imgcdn/master/vsc_img20200503134221.png)

一套简易的DevOps流程包含了协作、构建、测试、部署、运行。

而前端常说的开发规范、代码管理、测试、构建部署以及工程化其实都是在这一整个体系中。

当然，中小团队想玩好DevOps整套流程，需要的时间与研发成本，不比开发项目少。

DevOps核心思想就是：“快速交付价值，灵活响应变化”。其基本原则如下：

- [ ] 高效的协作和沟通；
- [ ] 自动化流程和工具；
- [ ] 快速敏捷的开发；
- [ ] 持续交付和部署；
- [ ] 不断学习和创新;

##### 在团队内/外促进协作

前端基建协作方面可以写的东西太多了，暂且粗略分为：团队内 与 团队外。

以下可能是前端们都能遇到的问题：

- 成员间水平各异，编写代码的风格各不相同，项目间难以统一管理。
- 不同项目Webpack配置差异过大，基础工具函数库和请求封装不一样。
- 项目结构与技术栈上下横跳，明明是同一 UI 风格，基础组件没法复用，全靠复制粘贴。
- 代码没注释，项目没文档，新人难以接手，旧项目无法维护。

1. 三层代码规范约束

   - 第一层，ESLint：
     - 常见的ESLint风格有：airbnb，google，standard。
     - 在多个项目间，规则不应左右横跳，如果项目周期紧张，可以适当放宽规则，让`warning`类弱警告可以通过。且一般建议成员的`IDE`和插件要统一，将客观因素影响降到最低。

   - 第二层，Git Hooks
     - git 自身包含许多 `hooks`，在 `commit`，`push` 等 git 事件前后触发执行。而`husky`能够防止不规范代码被`commit`、`push`、`merge`等等。
     - 代码提交不规范，全组部署两行泪。

    ```bash
    npm install husky pre-commit  --save-dev
    ```

    例子：

    ```js
    // package.json
    "scripts": {
        // ...
        "lint": "node_modules/.bin/eslint '**/*.{js,jsx}' && node_modules/.bin/stylelint '**/*.{css,scss}'",
        "lint:fix": "node_modules/.bin/eslint '**/*.{js,jsx}' --fix && node_modules/.bin/stylelint '**/*.{css,scss}' --fix"
    },
    "husky": {
        "hooks": {
        "pre-commit": "npm run lint",
        "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
        }
    },
    ```

    通过简单的安装配置，无论你通过命令行还是`Sourcetree`提交代码，都需要通过严格的校验。

    建议在根目录`README.md`注明提交规范：

    ```md
    ## Git 规范

    使用 [commitlint](https://github.com/conventional-changelog/commitlint) 工具，常用有以下几种类型：

    - feat ：新功能
    - fix ：修复 bug
    - chore ：对构建或者辅助工具的更改
    - refactor ：既不是修复 bug 也不是添加新功能的代码更改
    - style ：不影响代码含义的更改 (例如空格、格式化、少了分号)
    - docs ：只是文档的更改
    - perf ：提高性能的代码更改
    - revert ：撤回提交
    - test ：添加或修正测试

    举例
    git commit -m 'feat: add list'

    ```

    - 第三层，CI(持续集成)。

        > [前端代码规范最佳实践](https://mp.weixin.qq.com/s?__biz=MjM5NTk4MDA1MA==&mid=2458073414&idx=2&sn=cea7476d1c65f85eb45f38051586eb9e&scene=21#wechat_redirect)

        前两步的校验可以手动跳过（找骂），但CI中的校验是绝对绕不过的，因为它在服务端校验。使用 gitlab CI 做持续集成，配置文件 .gitlab-ci.yaml 如下所示:

    ```js
    lint:
    stage:lint
    only:
        -/^feature\/.*$/
    script:
        -npmlint
    ```

    这层校验，一般在稍大点的企业中，会由运维部的配置组完成。

    ![20200503135413](https://raw.githubusercontent.com/CatzillaOrz/imgcdn/master/vsc_img20200503135413.png)

1. 统一前端物料

公共组件、公共 UI、工具函数库、第三方 sdk 等该如何规范？

- 如何快速封装部门 UI 组件库？

首先，得感谢各大 UI 组件库的维护者们，给我们省了非常多的开发成本。
遥想`Jquery`时代，到处找插件的日子....
但是每个新团队都有自己的 UI 风格取向，你单引一个`ElementUI`，肯定会出现业务水土不服以及观感不同的地方，而如果你在每个项目都强行魔改，到处污染样式，这得多心累啊。
虽然各大组件库都有提供初始化变量的方式，但业务向的组件就没办法了。
解决方案之一，就是国外很火的一个开源库：`StoryBook`:

![20200503140528](https://raw.githubusercontent.com/CatzillaOrz/imgcdn/master/vsc_img/20200503140528.png)

Storybook是一个开源工具，用于独立开发React、Vue和Angular的UI组件。它能有组织和高效地构建 UI 组件。

Storybook提供了一个沙箱，用于隔离地构建 UI 组件。

类似于组件库的官方文档，却更加强大。可以通过控件和对出入参数调整，快速查看组件的用法，测试也可以对组件功能完整性等做校验。

一般的建议步骤是：

  1. 将业务从公共组件中抽离出来。
  1. 在项目中安装StoryBook(多项目时另起)
  1. 按官方文档标准，创建stories，并设定参数（同时也建议先写Jest测试脚本），写上必要的注释。
  1. 为不同组件配置StoryBook控件，最后部署。

- 如何统一部门所用的工具函数库和第三方sdk

其实这里更多的是沟通的问题，首先需要明确的几点：

- 部门内对约定俗成的工具库要有提前沟通，不能这头装一个MomentJs，另一头又装了DayJS。一般的原则是：轻量的自己写，超过可接受大小的找替代，譬如:DayJS替代MomentJs，ImmerJS替代immutableJS等。
- 部门间的有登录机制，请求库封装协议等。如果是SSO/扫码登录等，就协定只用一套，不允许后端随意变动。如果是请求库封装，就必须要后端统一Restful风格，相信我，不用Restful规范的团队都是灾难。前端连调会生不如死。
- Mock方式、路由管理以及样式写法也应当统一。

1. 在团队外促进协作

- 核心原则就是：**“能用文档解决的就尽量别 BB。”**
虽说现今前端的地位愈发重要，但我们经常在项目开发中遇到以下问题：

- 不同的后端接口规范不一样，前端需要耗费大量时间去做数据清洗兼容。
- 前端静态页开发完了，后端迟迟不给接口，因为没有接口文档，天天都得问。
- 测试反馈的问题，在原型上没有体现。

- 首先是原型方面：

- 一定要看明白产品给的原型文档！！！多问多沟通，这太重要了。
- 好的产品一般都会提供项目流程详图，但前端还是需要基于实际，做一张页面流程图。
- 要产品提供具体字段类型相关定义，不然得和后端扯皮。。。

- 其次是后端：
  - 执行Restful接口规范，不符合规范的接口驳回。
  - 必要的接口文档站点与 API 测试（如Swagger，Apidoc），不接受文件传输形式的接口。

- 然后是测试方面：

  - 为了避免测试提出一些无效的 bug，最好提前参与测试的用例评审。
  - 在实际开发中，如果有不合理功能需要修改，所有的修改都必须要求产品经理更新到 PRD 以及原型设计中。否则，测试如果不知道的话，会认为是 bug。
  - 通过自测和编写Jest单元测试，将代码意外bug降到合理程度。
  - 和测试一起吐槽后端的接口规范（滑稽）。

- 最后是运维方面：

  - 除了CI/CD相关的，其实很可以和运维一起写写nginx和插件开发。
