---
title: Serverless For Frontend 前世今生
tags:
  - serverless
categories:
  - Frontend
date: 2019-11-27 17:12:14
---

##### 前言

作为一个前端，你可能一直在迷茫，Node.js 的定位是什么？为什么我们需要它？

尤其是到了 2019 这个时间点，未来一段时间内，有一个词 -- Serverless 你会听到想吐。

> 所有人都在说 Serverless
>
> 几乎没有人知道如何落地 Serverless
>
> 但大家都觉得其他人在大力做 Serverless
>
> 所以大家都在宣传自己在做 Serverless

##### 演进史

###### 远古时代

- 天地初开，还没有出现前后端之分，仅有 设计 和 研发 两种角色：
  - 设计师根据需求产出高保真的原型图。
  - 研发根据需求和原型图来编写对应的业务逻辑和页面。

- 在 Web 1.0 的时代，大部分的 B/S 系统都采用的是 集中式架构，分为标准的三层（MVC）：

  - 数据访问层：封装对数据库的访问。
  - 服务层：用于业务逻辑的处理。
  - Web 层：用户处理页面渲染，路由逻辑等等。

- 比较流行的是 Struts + Spring + Hibernate 框架，还有 Dreamweaver 等前端三剑客。

###### 石器时代

- 然而艺术和代码之间的 Gap，对于很多缺乏艺术细胞的直男程序猿来说，是一件非常头疼的事。如何更好的提升用户交互体验，如何像素级的还原设计稿，都需要更高专业度的投入。

- 同时，由于互联网的迅猛增长，集中式架构已经逐渐无法满足海量的访问，从而演进出 分布式架构 ，对研发的能力要求也进一步提升。

- 因此基于专业度的诉求，逐渐分化出 前端研发 和 后端研发 的角色：
  - 前端此时更偏向设计，很多都是懂点研发的设计师承担。
  - 后端则更深入到业务建模，系统运维等方面。

- 此时的业务开发的套路，变为：
  - 前端研发 根据原型图，切图，产出 HTML/CSS/JS ，交付给 后端研发。
  - 后端研发 把 CSS/JS 上传 CDN，然后把 HTML 手动改写为后端模板。
  - 从数据库查询到一段动态数据。
  - 套到模板上，渲染出页面。

- 此时的主要矛盾在于前后端耦合 ：
  - 前端同学交付 HTML 页面，被后端改写为 TPL 模板。
  - 如果需求变更，从而导致 HTML 修改后，后端再次套模板的时候，merge 起来会比较考眼力。
  - 如果模板渲染有问题，往往是前端跑到后端的电脑上直接修改模板来调试，然后还需要同步回去自己的 HTML。

###### 青铜器时代

- 随着 Web 2.0 的到来，以 Google 推出的 Gmail 为号角，前端进入富应用时代，各种框架层出不穷，从 AJAX + jQuery，到逐渐形成 Angular、React、Vue 三国鼎立。

- 此时的业务开发的套路，变为 前后端分离：

  - 后端提供 API 接口，把 领域模型 转换为 数据传输对象，并通过 HTTP 对外服务。
  - 前端通过 AJAX 调用对应的接口，接管模板层，直接在浏览器侧渲染，也称之为前端渲染。

- `前后端分离` 一定程度上解决前后端的耦合问题，约定好接口后，前端可以直接 Mock 然后进行开发。

- 前端第一次翻身，如火如荼的投入到 `前端框架` 和 `前端工程化` 的建设中，矛盾在一定程度上弱化了。

###### 蒸汽时代

- 随着后端 `微服务化` 的演进，开始走向深水区，服务下沉，趋向稳定，业务被划分为很多独立的微服务。

- `前端框架` 和 `前端工程化` 趋向稳定，同时前端也进入了移动时代，出现了跨平台、跨终端适配的场景，对用户体验提出的更高的要求，对首屏时间等性能指标越来越重视，且发布频度越来越快。

- 此时的架构演化为：
- 后端分为很多微服务。
- 前端还是通过 HTTP 访问后端，但`接口变多`了，`性能变低`，且带来安全问题。
- 后端会提供一层 API 粘合层来缓解。

###### 随之而来的新的矛盾：服务下沉与用户体验灵活性的矛盾

- 服务趋向稳定，倾向下沉。
- 用户体验趋向不稳定，诉求服务的高度灵活与定制。
- 不同设备对 API 有不同的诉求，需要裁剪。
- `服务端接口，究竟是面向 UI 还是通用服务？`

此时 [Sam Newman](https://samnewman.io/patterns/architectural/bff/) 提出了 Backends For Frontends ：

- 简称之为 BFF，最重要的是：`服务自治` ，`谁使用谁开发`，带来了灵活与高效。
- BFF 根据团队的技术栈来选型，在我们的业务场景中，相对较优，生态最活跃，最能被前端接受的 Node.js。
- **BFF 层一直都存在**，**因为** `领域模型` - `UI 模型` **的转换是必然会存在的，区别只是在于维护者是谁。**
- GraphQL 之类的网关可以视为通用型的 BFF。

此时，`研发角色`又转变为：

- 后端研发，专注于业务建模，维护中间件服务和业务微服务。
- 全栈研发，专注于处理 Web 层，比模板层更进一步，接管 BFF 层。

其中全栈研发又有`两种来源`：

- 从前端进化过来的，一般会选择 Node.js 作为技术栈，使用诸如 Egg 等框架来降低前端同学的上手成本。
- 前端资源严重不足，于是赋能后端，助其转变为全栈，使用 Ant Design、Umi 等降低后端同学的上手成本。

##### 电气时代

- **BFF 的实践，在社区的分化严重**, 在大公司和创业公司比较受欢迎，但在话语权不强的中型公司，则举步维艰。

  - 小创业公司 - 追求快狠准，先活下来再说，效率优先，干就是了。
  - 大公司 - 具备良好的基建和研发流程支撑，对效能有更高的追求，如荼如火。
  - 中型公司 - 前端话语权不强，百废待兴，现有基建对前端不友好，推行举步维艰。

- 作为国内前端的引航者，过去几年，我们蚂蚁体验技术部工程产品的同学，产出了很多效能产品，包括：

  - Basement 前端工作台 - 打造更适合前端场景的工作流，管控研发流程和质量。
  - 云凤蝶 - 可视化建站，提升非前端专业同学在站点搭建方面的效能。
  - DockerLab - 轻运维，支持快速创新，让 idea 闪现到服务更加便捷。
  - Egg - 企业级的 Node.js 框架，扫清后端框架、中间件生态接入方面的障碍。
  - Ant Design - 企业级的中后台前端框架，让中后台产品 default to good.

- 但这些大部分还局限在 Pro Code 领域，离我们愿景中的终局还有很长的路要走。

- **此时的主要矛盾在于：**

- **专业人才储备**
  - 远远低估了前端的缺人程度，一将难求，无人可招。
  - 全栈人才的培养成本不低，包括前端需要学习后端 DevOps，后端需要学习前端的用户交互。
- **基建墙，各种流程太重**
  - 不同的基建服务需要去不同的后台走申请流程，N 多个工单需等待审批。
  - 像 DRM 的配置、Mobilegw 的配置，需要在每种环境中单独配置一遍。
- **资源浪费**
  - 在 BFF 场景下，服务器水位较低（10% ~ 30%），基于微服务的高可用诉求导致了服务器资源的浪费。
  - 譬如在蚂蚁容灾要求下，至少需要 11 台  4C8G 的容器。据此估算，支撑内部上千个中台应用，则就至少需要约 2000 台 32 核物理机！！！
