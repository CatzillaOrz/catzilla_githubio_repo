---
title: kyecloak权限控制策略
date: 2020-07-29 15:28:17
tags:
  - keycloak
---

##### Keycloak支持细粒度的授权策略，并可以对这些策略进一步组合

- 基于属性（ABAC）
- 基于角色（RBAC）
- 基于用户（UBAC）
- 基于上下文（CABC）
- 基于规则（Rule-based）
- 通过JavaScript
- 使用Jboss Drools
- 基于时间（Time-based）
- 通过SPI自定义访问控制策略（ACM）

Keycloak提供了一组管理界面和RESTful API，用于创建权限、关联权限与授权策略，以及在应用程序中执行授权决策。

资源服务器通常执行的是基于角色的访问控制（RBAC）策略，即检查用户所拥有的角色是否关联了要访问的资源。虽然这种方式非常有用，但是它们也有一些限制：

- 资源和角色紧耦合，角色的更改（如添加、删除或更改访问上下文）可能影响多个资源。
- 基于RBAC的应用程序无法很好地响应安全性需求的调整。
- 项目规模扩大时，复杂的角色管理会很困难而且容易出错。
- 不够灵活。角色并不能有效代表用户身份，即缺乏上下文信息。客观上来说被授予了角色的用户，至少会拥有某些访问权限。

当下的项目中，我们需要考虑不同地区、不同本地策略、使用不同设备、以及对信息共享有较高需求的异构环境。Keycloak授权服务可以通过以下方式帮助您提高应用程序和服务的授权能力：

- 不同的访问控制机制以及细粒度的授权策略。
- 中心化的资源、权限以及策略管理。
- 中心化的策略决策。
- REST风格的授权服务。
- 授权工作流以及用户访问管理。
- 可作为快速响应您项目中安全需求的基础设施。

---

##### 授权服务

Keycloak授权服务构建在已经广泛使用的标准上，比如OAuth2和UMA规范。OAuth2客户端（如前端应用程序）可以利用token endpoint从服务器获取Access Token，然后使用Access Token从资源服务器（例如后端服务）获取被保护的资源。Keycloak授权服务扩展了OAuth2，通过评估与被请求的资源或范围相关联的策略来发出Access Token。这意味着资源服务器可以基于含有权限Access Token来控制对资源的访问。在Keycloak授权服务中，这种具有权限的Access Token称为请求方令牌或简称RPT。除了发布RPTs之外，Keycloak授权服务还提供了一组RESTful端点，这些端点允许资源服务器管理它们受保护的资源、范围、权限和策略，帮助开发人员将这些功能集成到应用程序中，以支持细粒度授权。

###### 授权服务端点发现和元数据

Keycloak提供了一个发现文档来帮助客户端获取与Keycloak授权服务交互所需的任何信息，包括端点位置和功能。

```bash
curl -X GET \
  http://${host}:${port}/auth/realms/${realm}/.well-known/uma2-configuration

```

请将上面占位符中的变量替换为实际的值。

- token_endpoint

遵循OAuth2的Token端点支持 urn:ietf:params:oauth:grant-type:uma-ticket 授权类型。客户端可以向此端点发送授权申请并获取Keycloak RPT。

- token_introspection_endpoint

遵循Oauth2的Token检查端点。客户端可以使用该端点查询RPT的状态，并确定与token相关的任何其他信息，比如Keycloak授予的权限。

- resource_registration_endpoint

遵循UMA协议的资源注册端点，资源服务器可以使用它来管理资源以及范围。此端点提供了资源、范围的创建、查询、更新以及删除等功能。

- permission_endpoint

遵循UMA协议的权限管理端点，资源服务器可以用来管理权限tickets。提供了对permission ticket的创建、查询、更新以及删除等功能。

###### 获取权限

- 客户端验证方式
- 推送声明

###### User-Managed Access

- Keycloak 授权服务是基于User-Managed Access的，我们简写为UMA。UMA协议在下面这些方面增强了OAuth2：
  - 隐私
  - 点对点授权
  - 资源共享

- 授权过程

在UMA中，授权过程开始于客户端试图访问受UMA保护的资源服务器时。

受UMA保护的资源服务器会要求请求带有bearer token。下面模拟了不带权限ticket的请求：

```bash
curl -X GET \
  http://${host}:${port}/my-resource-server/resource/1bfdfe78-a4e1-4c2d-b142-fc92b75b986f
```

资源服务器将一个响应发送回客户机，该响应带有一个权限ticket和一个as_uri参数，该as_uri表示Keycloak服务器的地址，客户端可以将ticket发送到此地址来获得RPT。

下面是资源服务器的响应，注意响应中包含权限ticket：

```bash
HTTP/1.1 401 Unauthorized
WWW-Authenticate: UMA realm="${realm}",
    as_uri="https://${host}:${port}/auth/realms/${realm}",
    ticket="016f84e8-f9b9-11e0-bd6f-0021cc6004de"
```

权限ticket是Keycloak权限API发放的一种特殊token。它们代表被申请的权限（如资源以及范围）以及其它请求中的关联信息。只有资源服务器可以创建它们。

客户机已经拥有了权限ticket和Keycloak服务器的地址，就可以使用文档发现API来获取token endpoint的地址并发送授权请求。

下面的例子展示了客户端向token endpoint请求RPT：

```bash
curl -X POST \
  http://${host}:${port}/auth/realms/${realm}/protocol/openid-connect/token \
  -H "Authorization: Bearer ${access_token}" \
  --data "grant_type=urn:ietf:params:oauth:grant-type:uma-ticket" \
  --data "ticket=${permission_ticket}
```

Keycloak评估通过后会发放权限相关的RPT：

```bash
HTTP/1.1 200 OK
Content-Type: application/json
...
{
    "access_token": "${rpt}",
}
```

响应和其他授权类型的响应类似。RPT在access_token响应参数中，如果client没有通过授权服务端会返回403状态码：

```bash
HTTP/1.1 403 Forbidden
Content-Type: application/json
...
{
    "error": "access_denied",
    "error_description": "request_denied"
}
```

- 提交权限请求

在整个授权过程中，客户端首先需要从受UMA保护的资源服务器获得一个权限ticket，以便在Keycloak的token端点用它来交换RPT。

如果Keycloak判断客户端无法通过授权，会返回403及request_denied错误信息。

```bash
HTTP/1.1 403 Forbidden
Content-Type: application/json
...
{
    "error": "access_denied",
    "error_description": "request_denied"
}
```

上面的响应表示Keycloak不能对该权限ticket发放RPT。

某些时候客户端会用到异步授权流程，并让被资源所有者来决定是否授权。为此，客户端可以在对token端点的请求中带上submit_request参数：

```bash
curl -X POST \
  http://${host}:${port}/auth/realms/${realm}/protocol/openid-connect/token \
  -H "Authorization: Bearer ${access_token}" \
  --data "grant_type=urn:ietf:params:oauth:grant-type:uma-ticket" \
  --data "ticket=${permission_ticket} \
  --data "submit_request=true"
```

当使用submit_request参数时，Keycloak会记录下每个被拒绝的权限申请。然后资源所有者就可以检查并管理这些它们。

你可以将此功能视为APP中的请求访问按钮，用户可以向其他用户申请以访问他们的资源。

- 管理对用户资源的访问

###### 保护API

保护API遵循UMA提供了一系列端点：

- 资源管理
- 权限管理
- 策略API

Keycloak利用UMA保护API使资源服务器能够管理各自用户的权限。除了资源和权限API外，Keycloak还提供了策略API。可以让资源服务器代表其用户设置资源的权限。

使用此API要求资源服务器拥有名为protection API token（PAT）的特殊OAuth2 access token。在UMA中，PAT是有着uma_protection范围的token。

- 什么是PAT，如何获取它们？

一个protection API token（PAT）是有着uma_protection范围的特殊token。创建资源服务器时Keycloak会自动创建uma_protection的角色，并将其与客户端的服务帐户关联起来。

资源服务器可以像获取其他OAuth2 token那样从Keycloak获取PAT。下边是一个curl的例子：

```bash
curl -X POST \
    -H "Content-Type: application/x-www-form-urlencoded" \
    -d 'grant_type=client_credentials&client_id=${client_id}&client_secret=${client_secret}' \
    "http://localhost:8080/auth/realms/${realm_name}/protocol/openid-connect/token"
```

上面的示例使用client_credentials授权类型从服务器获取PAT。服务器返回的响应如下：

```json
{
  "access_token": ${PAT},
  "expires_in": 300,
  "refresh_expires_in": 1800,
  "refresh_token": ${refresh_token},
  "token_type": "bearer",
  "id_token": ${id_token},
  "not-before-policy": 0,
  "session_state": "ccea4a55-9aec-4024-b11c-44f6f168439e"
}
```

Keycloak可以使用不同的方式来认证客户端应用。上面使用了 client_credentials 方式，它要求提供一个client_id和一个client_secret。你可以自由选择其它方式来认证。

- 管理资源

Keycloak提供了遵循UMA协议的端点来远程管理资源。

```bash
http://${host}:${port}/auth/realms/${realm_name}/authz/protection/resource_set
```

- 管理权限申请
- 使用策略API来管理资源权限

###### RPT

RPT是使用JSON web signature（JWS） 签发的JSON web token（JWT）。RPT基于之前由Keycloak发出的OAuth2 access token来构建，该token用于用户自身或代表用户的特定客户端。

解码RPT可以看到它的payload如下：

```json
{
  "authorization": {
      "permissions": [
        {
          "resource_set_id": "d2fe9843-6462-4bfc-baba-b5787bb6e0e7",
          "resource_set_name": "Hello World Resource"
        }
      ]
  },
  "jti": "d6109a09-78fd-4998-bf89-95730dfd0892-1464906679405",
  "exp": 1464906971,
  "nbf": 0,
  "iat": 1464906671,
  "sub": "f1888f4d-5172-4359-be0c-af338505d86c",
  "typ": "kc_ett",
  "azp": "hello-world-authz-service"
}
```

从此token中，你可以从服务器获取所有 permissions 声明的权限。

注意，权限与要保护的资源/范围直接相关，并且完全与实际授予和发出这些权限的机制解耦。

- 审视RPT
- 获取RPT信息
- 每次检视RPT时都需要调用服务端吗？

###### 授权客户端的Java API

根据实际项目的需求，资源服务器客远程管理资源以及通过编程来检查权限。如果你正在使用Java，则可以使用授权客户端API来访问Keycloak授权服务。

Keycloak针对资源服务器的不同需求，提供了不同端点（如token端点、资源端点和权限管理端点）。

- Maven依赖
- 配置
- 创建授权客户端
- 获取用户授权
- 使用保护API来创建资源
- 检查RPT

###### 策略执行器

- 配置
- 声明信息点
  - 从HTTP请求中获取信息
  - 从外部HTTP服务获取信息
  - 静态声明
  - Cliam Informatica Provider 的SPI

- 获取授权上下文
- 使用AuthorizationContext获取授权客户端实例
- JavaScript集成
  - 处理来自UMA保护的资源服务器的授权响应
  - 获取权限
  - 授权请求
  - 获取RPT

- 设置TLS/HTTPS
