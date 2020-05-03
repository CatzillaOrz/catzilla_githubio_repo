---
layout: keycloak
title: 前端集成keycloak-Express+Keycloak
date: 2020-04-30 17:18:26
tags:
  - keycloak
  - sso
categories:
  - Frontend
---


##### Keycloak

Keycloak是一个开源软件产品，旨在为现代的应用程序和服务，提供包含身份管理和访问管理功能的单点登录工具。截至2018年3月，红帽公司负责管理这一JBoss社区项目，并将其作为他们RH-SSO产品的上游项目。从概念的角度上来说，该工具的目的是，只用少量编码甚至不用编码，就能很容易地使应用程序和服务更安全。

###### Express + Keycloak 前端应用场景

- Resource-Based Authorization

> 1. Resource-Based Authorization allows you to protect resources, and their specific methods/actions,** based on a set of policies defined in Keycloak, thus externalizing authorization from your application. This is achieved by exposing a `keycloak.enforcer` method which you can use to protect resources.*

```js
    ​app.get('/apis/me', keycloak.enforcer('user:profile'), userProfileHandler)
```

> 1. The keycloak-enforcer method operates in two modes, depending on the value of the `response_mode` configuration option.

```js
    ​app.get('/apis/me', keycloak.enforcer('user:profile', {response_mode: 'token'}), userProfileHandler)
```

> 1. If `response_mode` is set to `token`, permissions are obtained from the server on behalf of the subject represented by the bearer token that was sent to your application. In this case, a new access token is issued by Keycloak with the permissions granted by the server. If the server did not respond with a token with the expected permissions, the request is denied. When using this mode, you should be able to obtain the token from the request as follows:

```js
​   app.get('/apis/me', keycloak.enforcer('user:profile', {response_mode: 'token'}), function (req, res) {
       ​var token = var token = req.kauth.grant.access_token.content;
       ​var permissions = token.authorization ? token.authorization.permissions : undefined;

       ​// show user profile
   ​})
```

> 1. Prefer this mode when your application is using sessions and you want to cache previous decisions from the server, as well automatically handle refresh tokens. This mode is especially useful for applications acting as a client and resource server.
>
> If `response_mode` is set to `permissions` (default mode), the server only returns the list of granted permissions, without issuing a new access token. In addition to not issuing a new token, this method exposes the permissions granted by the server through the `request` as follows:

```js
    app.get('/apis/me', keycloak.enforcer('user:profile', {response_mode: 'token'}), function (req, res) {
       ​var permissions = req.permissions;

       ​// show user profile
   ​})
```

> 1. Regardless of the response_mode in use, the keycloak.enforcer method will first try to check the permissions within the bearer token that was sent to your application. If the bearer token already carries the expected permissions, there is no need to interact with the server to obtain a decision. This is specially useful when your clients are capable of obtaining access tokens from the server with the expected permissions before accessing a protected resource, so they can use some capabilities provided by Keycloak Authorization Services such as incremental authorization and avoid additional requests to the server when keycloak.enforcer is enforcing access to the resource.
>
> By default, the policy enforcer will use the client_id defined to the application (for instance, via keycloak.json) to reference a client in Keycloak that supports Keycloak Authorization Services. In this case, the client can not be public given that it is actually a resource server.
>
> If your application is acting as both a public client(`frontend`) and resource server(backend), you can use the following configuration to reference a different client in Keycloak with the policies that you want to enforce:

```js
​keycloak.enforcer('user:profile', {resource_server_id: 'my-apiserver'}
```

> **It is recommended to use distinct clients** in Keycloak to represent your `frontend` and backend.
> If the application you are protecting is enabled with Keycloak authorization services and you have defined client credentials in keycloak.json, you can push additional claims to the server and make them available to your policies in order to make decisions. For that, you can define a claims configuration option which expects a function that returns a JSON with the claims you want to push:

```js
    app.get('/protected/resource', keycloak.enforcer(['resource:view', 'resource:write'], {
         ​claims: function(request) {
           ​return {
             ​"http.uri": ["/protected/resource"],
             ​"user.agent": // get user agent  from request
           ​}
         ​}
       ​}), function (req, res) {
```

[Advanced authorization](https://www.keycloak.org/docs/latest/securing_apps/index.html#additional-urls)
[readmore](https://www.keycloak.org/docs/latest/securing_apps/index.html#additional-urls)

根据keycloak官方指南中原文部分可以看出如何去区分keycloak的不同应用场景。

###### Keycloak node adapter Usage

- Instantiate a Keycloak class

The Keycloak class provides a central point for configuration and integration with your application. The simplest creation involves no arguments.

```js
   var session = require('express-session');
   ​var Keycloak = require('keycloak-connect');

   ​var memoryStore = new session.MemoryStore();
   ​var keycloak = new Keycloak({ store: memoryStore })
```

By default, this will locate a file named keycloak.json alongside the main executable of your application to initialize keycloak-specific settings (public key, realm name, various URLs). The keycloak.json file is obtained from the Keycloak Admin Console.

Instantiation with this method results in all of the reasonable defaults being used. As alternative, it’s also possible to provide a configuration object, rather than the keycloak.json file:

```js
    ​let kcConfig = {
       ​clientId: 'myclient',
       ​bearerOnly: true,
       ​serverUrl: 'http://localhost:8080/auth',
       ​realm: 'myrealm',
       ​realmPublicKey: 'MIIBIjANB...'
   ​};

   ​let keycloak = new Keycloak({ store: memoryStore }, kcConfig)
```

Applications can also redirect users to their preferred identity provider by using:

```js
    ​let keycloak = new Keycloak({ store: memoryStore, idpHint: myIdP }, kcConfig)
```

- Configuring a web session store

If you want to use web sessions to manage server-side state for authentication, you need to initialize the Keycloak(…​) with at least a store parameter, passing in the actual session store that express-session is using.

```js
   var session = require('express-session');
   ​var memoryStore = new session.MemoryStore();

   ​var keycloak = new Keycloak({ store: memoryStore })
```

- Passing a custom scope value

By default, the scope value openid is passed as a query parameter to Keycloak’s login URL, but you can add an additional custom value:

```js
    ​var keycloak = new Keycloak({ scope: 'offline_access' })
```

- Installing Middleware

Once instantiated, install the middleware into your connect-capable app:

```js
   var app = express();

   ​app.use( keycloak.middleware() )
```

- Checking Authentication

To check that a user is authenticated before accessing a resource, simply use keycloak.checkSso(). It will only authenticate if the user is already logged-in. If the user is not logged-in, the browser will be redirected back to the originally-requested URL and remain unauthenticated:

```js
    ​app.get( '/check-sso', keycloak.checkSso(), checkSsoHandler )
```

- Protecting Resources

  - Simple authentication

    To enforce that a user must be authenticated before accessing a resource, simply use a no-argument version of keycloak.protect():

```js
    ​app.get( '/complain', keycloak.protect(), complaintHandler )
```

[Role-based authorization](https://www.keycloak.org/docs/latest/securing_apps/index.html#protecting-resources)

###### Express中间件

[Passport is authentication middleware for Node](http://www.passportjs.org/docs/authenticate/)

- passport 不能完全适配keycloak的场景，keycloak实现了自己的Adapter。但不排除未来标准化，所以保留部分passport oidc中间件策略功能，统一跳转设置未授权跳转到`oidc/login`保证逻辑完整性

- 此外，官方文档中描述了单点登陆的特性，Adapter自动默认设置server请求logout时销毁session退出用户的实现：

> Explicit user-triggered logout
>
> **By default**, the middleware catches calls to /logout to send the user through a Keycloak-centric logout workflow. This can be changed by specifying a logout configuration parameter to the middleware() call:

```js
    ​app.use( keycloak.middleware( { logout: '/logoff' } ))
```

- 说明中间件支持配置指定的退出策略，只需要配置并use即可

- server端ssl等意外请求报错
  - process.env.NODE_TLS_REJECT_UNAUTHORIZED = '0';的配置可能会影响 Keycloak的 Node Adapter 的正常使用，此处是一个很大的坑！

- readmore
  - Nodejs Passport 系列之一：基础概念
    - [Nodejs Passport 系列之一：基础概念](https://www.shangyang.me/2018/03/07/javascript-nodejs-passport-01-basic/)

###### Read more

[blog-Keycloak and Express](https://codeburst.io/keycloak-and-express-7c71693d507a)
[blog-Openshift, Node and Keycloak](https://medium.com/@auscunningham/openshift-node-and-keycloak-67b25582f51c)

[官方Basic NodeJS Example-keycloak-nodejs-connect](https://github.com/keycloak/keycloak-nodejs-connect/tree/54815e742a931fe6f750a4a024c37ccc4d4fdc43/example)

[官方Docker Using the image](https://hub.docker.com/r/jboss/keycloak/)

> `keycloak-nodejs-connect` && `keycloak Docker image` 可以做一个完整的本地案例
