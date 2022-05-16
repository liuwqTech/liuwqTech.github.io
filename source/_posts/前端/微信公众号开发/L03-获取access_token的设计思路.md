---
title: L03-获取access_token的设计思路
date: 2021-09-11 09:14:46
tags: 微信公众号、前端
categories: 
- 前端
- 微信公众号开发
cover: https://tva1.sinaimg.cn/large/008i3skNly1gucf7xqrtyj60x00oe3za02.jpg
---

# **L03-获取access_token的设计思路**
1、什么是access_token？   

A、access_token是公众号的全局唯一接口调用凭据，公众号调用各接口时都需要使用access_token。

B、access_token的存储至少要保留512个字符空间。

C、有效期目前为2h，需定时刷新，重复获取将导致上次获取的access_token失效。

D、接口权限，每天最多获取2000次

E、接口调用请求说明

![](https://tva1.sinaimg.cn/large/008i3skNly1gucf9xyoq3j60ns02ujrh02.jpg)  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucfac67f7j60xm0ccq3m02.jpg)  

F、返回说明
- 正常情况下，微信会返回下述JSON数据包给公众号：
`{"access_token":"ACCESS_TOKEN","expires_in":7200}`  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucfb5sq92j60u408saab02.jpg)  

- 错误时微信会返回错误码等信息，JSON数据包示例如下（该示例为AppID无效错误）:
`{"errcode":40013,"errmsg":"invalid appid"}`  

G、返回码说明  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucfc9bcypj60xl0u041402.jpg)




2、新建03_获取access_token文件夹，在wechat里新建accessToken.js文件  

3、获取access_token的思路  

读取本地文件
   - 本地有文件
       - 判断它是否过期
           - 过期：重新请求获取access_token，保存下来覆盖之前的文件（保证文件是唯一的）
           - 没有过期：直接使用
   - 本地没有文件
       - 发送请求获取access_token，保存下来（本地文件），直接使用