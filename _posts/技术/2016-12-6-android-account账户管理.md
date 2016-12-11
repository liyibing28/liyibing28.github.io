---
layout: post
title: warmshowers利用account实现账户管理
category: 技术
tags: warmshwoers android 学习
keywords: 
description: 
---

android学习～～利用google accountmanager实现账户管理

为什么要学习accountmanager ，warmshowers android app中选择account进行管理。
好处：标准的用户鉴权方式；为开发者简化了登录的流程；处理访问拒绝的场景；可以为一个账户处理不同类型的访问令牌（如：只读、全权限）；轻松的在不同程序间共享令牌；有如Sync Adapter这样的后台处理的良好支持；并且，在手机的设置界面中有一个很酷的入口

### 参考资料： 
http://blog.csdn.net/wy3243996/article/details/52411139

* Authentication Token：是由服务器提供的一个临时的访问令牌。所有需要识别用户的请求，在发送到服务器时都要带着这个令牌。

* AccountManager– 管理设备上的所有账户，也是这项功能的核心。App从 AccountManager 获得auth-token，它也将决定是否该打开登录、创建用户的 Activity ，或者从之前的请求中返回一个已经存储好的auth-token。 AccountManager 了解不同场景下该调用何种操作。

- AccountAuthenticator– 是一个为具体账户类型提供鉴权处理过程的组件。 AccountManager 查找合适的 AccountAuthenticator ，与其通信，并根据账户类型执行所有动作。 AccountAuthenticator 知道哪个 Activity 用来让用户输入登录信息，也知道服务器上次返回的auth-token在哪里存储。在一个账户类型下，多个不同的服务也会共用同一个 AccountAuthenticator 。比如Google的AccountAuthenticator 为GMail提供认证服务，也为其他的Google程序，如：Google Calendar和Google Drive提供授权服务。

- AccountAuthenticatorActivity– “登录／创建用户“ Activity 的基类，当用户需要认证的时候， authenticato r调用会这个 Activity 。这个 Activity 负责用户登录或用户创建过程，并将auth-token返回给

authenticator


当你的App需要auth-token时，只需调用 AccountManager#getAuthToken() 。 AccountManager 将负责一切必须的步骤直到给你拿到auth-token。Google提供了一个流程图展现了整个过程：

- mAuthTokenType 是我从服务器请求的令牌的类型

### warmshower 实现account结构：
- activity 
 - AuthenticatorActivity 与用户交互界面，“登录／创建用户“
- api
 - Restclient 与后台服务器交互
- auth
 - http/HttpAuthenticationFailedException 错误处理
 - http/HttpAuthenticator 账户通信操作（验证，下载）
 - http/HttpSessionContainer
 - AuthenticationHelper
 - AuthenticationService
 - Authenticator 所有操作的核心
 - NoAccountException
