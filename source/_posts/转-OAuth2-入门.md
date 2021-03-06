title: '[转]OAuth2 入门'
author: star liu
tags: []
categories: []
copyright: false
date: 2020-02-22 10:57:00
---
最近在看Gitlab的OAouth2 文档，计划使运维平台接入Gitlab的授权认证。

<!--more-->
*转载自[cnblog-白细胞-oauth2深入介绍](https://www.cnblogs.com/Wddpct/p/8976480.html#3-oauth-2-%E7%9A%84%E6%8E%88%E6%9D%83%E6%B5%81%E7%A8%8B)*

# 1. 前言
OAuth 2 是一个授权框架，或称授权标准，它可以使第三方应用程序或客户端获得对HTTP服务上（例如 Google，GitHub ）用户帐户信息的有限访问权限。OAuth 2 通过将用户身份验证委派给托管用户帐户的服务以及授权客户端访问用户帐户进行工作。综上，OAuth 2 可以为 Web 应用 和桌面应用以及移动应用提供授权流程。

本文将从OAuth 2 角色，授权许可类型，授权流程等几方面进行讲解。

在正式讲解之前，这里先引入一段应用场景，用以与后文的角色讲解对应。

开发者A注册某IT论坛后，发现可以在信息栏中填写自己的 Github 个人信息和仓库项目，但是他又觉得手工填写十分麻烦，直接提供 Github 账户和密码给论坛管理员帮忙处理更是十分智障。
不过该论坛似乎和 Github 有不可告人的秘密，开发者A可以点击“导入”按钮，授权该论坛访问自己的 Github 账户并限制其只具备读权限。这样一来， Github 中的所有仓库和相关信息就可以很方便地被导入到信息栏中，账户隐私信息也不会泄露。
这背后，便是 OAuth 2 在大显神威。

# 2. OAuth2 角色
OAuth 2 标准中定义了以下几种角色：

- 资源所有者（Resource Owner）
- 资源服务器（Resource Server）
- 授权服务器（Authorization Server）
- 客户端（Client）

## 2.1 资源所有者（Resource Owner）

资源所有者是 OAuth 2 四大基本角色之一，在 OAuth 2 标准中，资源所有者即代表授权客户端访问本身资源信息的用户（User），也就是应用场景中的“开发者A”。客户端访问用户帐户的权限仅限于用户授权的“范围”（aka. scope，例如读取或写入权限）。

如果没有特别说明，下文中出现的"用户"将统一代表资源所有者。

## 2.2 资源/授权服务器（Resource/Authorization Server）
资源服务器托管了受保护的用户账号信息，而授权服务器验证用户身份然后为客户端派发资源访问令牌。

在上述应用场景中，Github 既是授权服务器也是资源服务器，个人信息和仓库信息即为资源（Resource）。而在实际工程中，不同的服务器应用往往独立部署，协同保护用户账户信息资源。

## 2.3 客户端（Client）
在 OAuth 2 中，客户端即代表意图访问受限资源的第三方应用。在访问实现之前，它必须先经过用户者授权，并且获得的授权凭证将进一步由授权服务器进行验证。

如果没有特别说明，下文中将不对"应用"，“第三方应用”，“客户端”做出区分。

# 3. OAuth 2 的授权流程
目前为止你应该对 OAuth 2 的角色有了些概念，接下来让我们来看看这几个角色之间的抽象授权流程图和相关解释。

*
请注意，实际的授权流程图会因为用户返回授权许可类型的不同而不同。但是下图大体上能反映一次完整抽象的授权流程。
*

![upload successful](/images/pasted-2.png)

- Authrization Request

	客户端向用户请求对资源服务器的`authorization grant`。

- Authorization Grant（Get)

	如果用户授权该次请求，客户端将收到一个`authorization grant`。

- Authorization Grant（Post）

	客户端向授权服务器发送它自己的客户端身份标识和上一步中的`authorization grant`，请求访问令牌。

- Access Token（Get）

	如果客户端身份被认证，并且`authorization grant`也被验证通过，授权服务器将为客户端派发`access token`。授权阶段至此全部结束。

- Access Token（Post && Validate）

	客户端向资源服务器发送`access token`用于验证并请求资源信息。

- Protected Resource（Get）

	如果`access token`验证通过，资源服务器将向客户端返回资源信息。

# 4. 客户端应用注册
在应用 OAuth 2 之前，你必须在授权方服务中注册你的应用。如 Google Identity Platform 或者 Github OAuth Setting，诸如此类 OAuth 实现平台中一般都要求开发者提供如下所示的授权设置项。

- 应用名称
- 应用网站
- 重定向URI或回调URL

重定向URI是授权方服务在用户授权（或拒绝）应用程序之后重定向供用户访问的地址，因此也是用于处理授权码或访问令牌的应用程序的一部分。

## 4.1 Client ID 和 Client Secret

一旦你的应用注册成功，授权方服务将以`client id`和`client secret`的形式为应用发布`client credentials`（客户端凭证）。`client id`是公开透明的字符串，授权方服务使用该字符串来标识应用程序，并且还用于构建呈现给用户的授权 `url` 。当应用请求访问用户的帐户时，`client secret`用于验证应用身份，并且必须在客户端和服务之间保持私有性。

# 5. 授权许可（Authorization Grant）
如上文的抽象授权流程图所示，前四个阶段包含了获取`authorization grant`和`access token`的动作。授权许可类型取决于应用请求授权的方式和授权方服务支持的 Grant Type。OAuth 2 定义了四种 Grant Type，每一种都有适用的应用场景。

- Authorization Code

	结合普通服务器端应用使用。
    
- Implicit

	结合移动应用或 Web App 使用。
- Resource Owner Password Credentials

	适用于受信任客户端应用，例如同个组织的内部或外部应用。
- Client Credentials

	适用于客户端调用主服务API型应用（比如百度API Store）
    
以下将分别介绍这四种许可类型的相关授权流程。

## 5.1 Authorization Code Flow
`Authorization Code` 是最常使用的一种授权许可类型，它适用于第三方应用类型为`server-side`型应用的场景。`Authorization Code`授权流程基于重定向跳转，客户端必须能够与`User-agent`（即用户的 Web 浏览器）交互并接收通过`User-agent`路由发送的实际`authorization code`值。


![upload successful](/images/pasted-3.png)

### 1. User Authorization Request
首先，客户端构造了一个用于请求authorization code的URL并引导User-agent跳转访问。
```
https://authorization-server.com/auth
 ?response_type=code
 &client_id=29352915982374239857
 &redirect_uri=https%3A%2F%2Fexample-client.com%2Fcallback
 &scope=create+delete
 &state=xcoiv98y2kd22vusuye3kch
response_type=code
```
此参数和参数值用于提示授权服务器当前客户端正在进行`Authorization Code`授权流程。

- client_id 客户端身份标识。
- redirect_uri 标识授权服务器接收客户端请求后返回给`User-agent`的跳转访问地址。
- scope 指定客户端请求的访问级别。
- state 由客户端生成的随机字符串，步骤2中用户进行授权客户端的请求时也会携带此字符串用于比较，这是为了防止CSRF攻击。

### 2. User Authorizes Applcation
当用户点击上文中的示例链接时，用户必须已经在授权服务中进行登录（否则将会跳转到登录界面，不过 OAuth 2 并不关心认证过程），然后授权服务会提示用户授权或拒绝应用程序访问其帐户。以下是授权应用程序的示例：

![upload successful](/images/pasted-4.png)

### 3. Authorization Code Grant
如果用户确认授权，授权服务器将重定向`User-agent`至之前客户端提供的指向客户端的`redirect_uri`地址，并附带`code`和`state`参数（由之前客户端提供），于是客户端便能直接读取到`authorization code`值。
```
https://example-client.com/redirect
?code=g0ZGZmNjVmOWIjNTk2NTk4ZTYyZGI3
&state=xcoiv98y2kd22vusuye3kch
```
`state`值将与客户端在请求中最初设置的值相同。客户端将检查重定向中的状态值是否与最初设置的状态值相匹配。这可以防止CSRF和其他相关攻击。

`code`是授权服务器生成的`authorization code`值。`code`相对较短，通常持续1到10分钟，具体取决于授权服务器设置。

### 4. Access Token Request
现在客户端已经拥有了服务器派发的authorization code，接下来便可以使用authorization code和其他参数向服务器请求access token（POST方式）。其他相关参数如下：

- grant_type=authorization_code - 这告诉服务器当前客户端正在使用Authorization Code授权流程。
- code - 应用程序包含它在重定向中给出的授权码。
- redirect_uri - 与请求authorization code时使用的redirect_uri相同。某些资源（API）不需要此参数。
- client_id - 客户端标识。
- client_secret - 应用程序的客户端密钥。这确保了获取access token的请求只能从客户端发出，而不能从可能截获authorization code的攻击者发出。
### 5. Access Token Grant
服务器将会验证第4步中的请求参数，当验证通过后（校验`authorization code`是否过期，`client id`和`client secret`是否匹配等），服务器将向客户端返回`access token`。
```
{
  "access_token":"MTQ0NjJkZmQ5OTM2NDE1ZTZjNGZmZjI3",
  "token_type":"bearer",
  "expires_in":3600,
  "refresh_token":"IwOGYzYTlmM2YxOTQ5MGE3YmNmMDFkNTVk",
  "scope":"create delete"
}
```
至此，授权流程全部结束。直到access token 过期或失效之前，客户端可以通过资源服务器API访问用户的帐户，并具备scope中给定的操作权限。

## 5.2 Implicit Flow
`Implicit`授权流程和`Authorization Code`基于重定向跳转的授权流程十分相似，但它适用于移动应用和 `Web App`，这些应用与普通服务器端应用相比有个特点，即`client secret`不能有效保存和信任。

相比`Authorization Code`授权流程，`Implicit`去除了请求和获得`authorization code`的过程，而用户点击授权后，授权服务器也会直接把`access token`放在`redirect_uri`中发送给`User-agent`（浏览器）。 同时第1步构造请求用户授权 `url` 中的`response_type`参数值也由 `code` 更改为 `token` 或 `id_token` 。


![upload successful](/images/pasted-5.png)

### 1. User Authorization Request
客户端构造的URL如下所示：
```
https://{yourOktaDomain}.com/oauth2/default/v1/authorize?client_id=0oabv6kx4qq6
h1U5l0h7&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%3
A8080&state=state-296bc9a0-a2a2-4a57-be1a-d0e2fd9bb601&nonce=foo'
```
response_type的response_type参数值为 token 或 id_token 。

其他请求参数与Authorization Code授权流程相比没有并什么变化。

### 2. User Authorizes Application（略）
### 3. Redirect URI With Access Token In Fragment
假设用户授予访问权限，授权服务器将User-agent（浏览器） 重定向回客户端使用之前提供的redirect_uri。并在 uri 的 #fragment 部分添加access_token键值对。如下所示：
```
http://localhost:8080/#access_token=eyJhb[...]erw&token_type=Bearer&expires_in=3600&scope=openid&state=state-296bc9a0-a2a2-4a57-be1a-d0e2fd9bb601
```
token_type - 当且仅当response_type设置为 token 时返回，值恒为 Bearer。

注意在Implicit流程中，access_token值放在了 URI 的 #fragment 部分，而不是作为 ?query 参数。

### 4. User-agent Follows the Redirect URI
User-agent（浏览器）遵循重定向指令，请求redirect_uri标识的客户端地址，并在本地保留 uri 的 #fragment 部分的access_token信息。

### 5. Application Sends Access Token Extraction Script
客户端生成一个包含 token 解构脚本的 Html 页面，这个页面被发送给User-agent（浏览器），执行脚本解构完整的redirect_uri并提取其中的access_token（access token信息在第4步中已经被User-agent保存）。

### 6. Access Token Passed to Application
User-agent（浏览器）向客户端发送解构提取的access token。

至此，授权流程全部结束。直到access token 过期或失效之前，客户端可以通过资源服务器API访问用户的帐户，并具备scope中给定的操作权限。

## 5.3 Resource Owner Password Credentials Flow
Resource Owner Password Credentials授权流程适用于用户与客户端具有信任关系的情况，例如设备操作系统或同一组织的内部及外部应用。用户与应用交互表现形式往往体现为客户端能够直接获取用户凭据（用户名和密码，通常使用交互表单）。


![upload successful](/images/pasted-6.png)

### 1. Resource Owner Password Credentials From User Input
用户向客户端提供用户名与密码作为授权凭据。

### 2. Resource Owner Password Credentials From Client To Server
客户端向授权服务器发送用户输入的授权凭据以请求 access token。客户端必须已经在服务器端进行注册。
```
POST /token HTTP/1.1
Host: server.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded

grant_type=password&username=johndoe&password=A3ddj3w
grant_type - 必选项，值恒为 password
```
### 3. Access Token Passed to Application
授权服务器对客户端进行认证并检验用户凭据的合法性，如果检验通过，将向客户端派发 access token>
```
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"example",
  "expires_in":3600,
  "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
  "example_parameter":"example_value"
}
```
## 5.4 Client Credentials Flow
Client Credential是最简单的一种授权流程。客户端可以直接使用它的client credentials或其他有效认证信息向授权服务器发起获取access token的请求。


![upload successful](/images/pasted-7.png)

两步中的请求体和返回体分别如下：
```
POST /token HTTP/1.1
Host: server.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
grant_type - 必选项，值恒为 client_credentials
{
       "access_token":"2YotnFZFEjr1zCsicMWpAA",
       "token_type":"example",
       "expires_in":3600,
       "example_parameter":"example_value"
}
```
# 6. 总结
说实在的，笔者已经有很长一段时间没有好好地分享心得，发表博客，这固然有工作繁忙，学习充实的原因，但确实也是有些懒，既然认识到了，自然就不希望再堕落下去了。

这一年里读了很多书，做了很多事，虽然自觉在大学时期便接触了部分项目管理和开发的知识，但是工作后的收获仍然十分动心。微服务也好，DDD也好，或是具体的数据库理论和运维工程实践上笔者也有了更深的认识。而时至今日，笔者觉得自己是又到了重新积淀，重新迈向下一个阶段的时候。鉴权服务作为构建健壮微服务必不可少的一环（甚至可以说是第一个工程），所以以 OAuth 2 作为重启的第一篇。当然 OAuth 2 也仍然只是鉴权体系中的授权理论，更基础的认证（Authentication）理论还没有引出，希望在之后的日子里能带来更多关于鉴权相关的博文，如认证体系和功能权限设计在工程上的应用，共勉。

**学习和编程都是快乐的。**

# 参考资料及文献
- [The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)
- [What is the OAuth 2.0 Authorization Code Grant Type?](https://developer.okta.com/blog/2018/04/10/oauth-authorization-code-grant-type)
- [An Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2#grant-type-implicit)
- [Implicit Flow](https://developer.okta.com/authentication-guide/implementing-authentication/implicit)
- [Response Properties](https://developer.okta.com/docs/api/resources/oidc#authorize)
- [认证授权 OAuth2授权](http://www.cnblogs.com/linianhui/p/oauth2-authorization.html)

**名词中英文对照**

| 英文                | 中文     |
| ------------------- | -------- |
| Authorization Grant | 授权许可 |
| Authorization Code  | 授权码   |
| Access Token        | 访问令牌 |
| Authorization       | 授权     |
| Authentication      | 认证     |