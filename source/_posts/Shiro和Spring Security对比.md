---
title: Shiro和Spring Security对比
date: 2021-11-02 09:22:46
tags: Shiro、后端、验证授权、前后端
categories: 后端知识详解
cover: https://tva1.sinaimg.cn/large/008i3skNly1gvjsbbrnggj60g40fm74w02.jpg
---


# Shiro和Spring Security对比

## 一、Spring Security 特点

- Shiro的功能它都有，但是不能脱离Spring。

- Spring Security的权限细粒度更高（还未发现高在哪里）。

## 二、Shiro 特点

- 易于理解的 Java Security API；

- 简单的身份认证（登录），支持多种数据源（LDAP，JDBC，Kerberos，ActiveDirectory 等）；

- 对角色的简单的签权（访问控制），支持细粒度的签权；

- 支持一级缓存，以提升应用程序的性能；

- **内置的基于 POJO 企业会话管理，适用于 Web 以及非 Web 的环境**；

- 异构客户端会话访问；

- 非常简单的加密 API；

- **不跟任何的框架或者容器捆绑，可以独立运行**。

## 三、Shiro 和 Spring Security 比较

首先 Shiro 较之 Spring Security，Shiro在保持强大功能的同时，还在简单性和灵活性方面拥有巨大优势。Shiro是一个强大而灵活的开源安全框架，能够非常清晰的处理认证、授权、管理会话以及密码加密。如下是它所具有的特点：

- Shiro比Spring更容易使用，实现和最重要的理解；

- Spring Security更加知名的唯一原因是因为品牌名称，“Spring”以简单而闻名，但讽刺的是很多人发现安装Spring Security很难；
- 然而，Spring Security却有更好的社区支持；
  Apache Shiro在Spring Security处理密码学方面有一个额外的模块；
- Shiro 功能强大、且 简单、灵活。是Apache 下的项目比较可靠，且不跟任何的框架或者容器绑定，可以独立运行。

**注：**

​       OAuth在"客户端"与"服务提供商"之间，设置了一个授权层（authorization layer）。"客户端"不能直接登录"服务提供商"，只能登录授权层，以此将用户与客户端区分开来。"客户端"登录授权层所用的令牌（token），与用户的密码不同。用户可以在登录的时候，指定授权层令牌的权限范围和有效期。

​       "客户端"登录授权层以后，"服务提供商"根据令牌的权限范围和有效期，向"客户端"开放用户储存的资料。

​        OpenID 系统的第一部分是身份验证，即如何通过 URI 来认证用户身份。目前的网站都是依靠用户名和密码来登录认证，这就意味着大家在每个网站都需要注册用户名和密码，即便你使用的是同样的密码。

​       如果使用 OpenID ，你的网站地址（URI）就是你的用户名，而你的密码安全的存储在一个 OpenID 服务网站上（你可以自己建立一个 OpenID 服务网站，也可以选择一个可信任的 OpenID 服务网站来完成注册）。

​       与OpenID同属性的身份识别服务商还有ⅥeID,ClaimID,CardSpace,Rapleaf,Trufina ID Card等，其中ⅥeID通用账户的应用最为广泛。

## 四、**总结**

​       个人认为现阶段需求，权限的操作粒度能控制在路径及按钮上，数据粒度通过sql实现。Shrio简单够用。

​       至于OAuth，OpenID 站点间统一登录功能，现租户与各个产品间单点登录已经通过cookies实现，所以Spring Security的这两个功能可以不考虑。

​       SpringSide网站的权限也是用Shrio做的。