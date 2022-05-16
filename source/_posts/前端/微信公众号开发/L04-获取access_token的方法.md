---
title: L04-获取access_token的方法
date: 2021-09-11 16:16:46
tags: 微信公众号、前端
categories: 
- 前端
- 微信公众号开发
cover: https://tva1.sinaimg.cn/large/008i3skNly1gucrfm1on5j60tc0s8q4l02.jpg
---

# **L04-获取access_token的方法**
1、方法定义：   

读取本地文件（readAccessToken）
   - 本地有文件
       - 判断它是否过期（isValidAccessToken）
           - 过期：重新请求获取access_token（getAccessToken），保存下来覆盖之前的文件（保证文件是唯一的）（saveAccessToken）
           - 没有过期：直接使用
   - 本地没有文件
       - 发送请求获取access_token（getAccessToken），保存下来（本地文件）（saveAccessToken），直接使用  


2、getAccessToken()  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucs4p99xbj61kw0oi0uw02.jpg)  
- 要引入config模块
- 需要下载request、request-promise-native模块，但只需要引入request-promise-native，终端输入`npm i request request-promise-native`进行下载  

3、测试  

`const w = new Wechat();`  
`w.getAccessToken();`  

结果如下：  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucs6x94swj61mk09uabr02.jpg)  

4、计算失效时间  

- 当前时间+7200s-5*60  

5、需要外层getAccessToken方法返回这个值，所以用一个promise进行封装  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucs7xr6iuj60ys0icdhs02.jpg)  

6、saveAccessToken(accessToken)  
- accessToken是一个对象，需要转换为JSON格式才能读取
- writeFile是一个异步方法，若要保证其能够进行保存，需要将其封装成一个promise对象  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucs8kths0j60y20q0mzj02.jpg)  

7、readAccessToken()  

- 不需要参数
- data是一个json字符串，我们希望得到一个json对象，需要对其进行转化  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucs9dol8yj60ya0no40902.jpg)  

8、isValidAccessToken(data)
- 先判断access_token是否有效
- 再判断access_token是否在有效期内
- 注意代码量的精简（注释部分的判断），直接用return可以判断后返回一个布尔值  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucs9yektfj60uu0os40202.jpg)  

9、测试：按照逻辑编写代码  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucsar0xa1j60u00vpwgk02.jpg)  

- 运行结果如下：  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucsb80eiyj61mg0aqjt502.jpg)  

- 同时，会生成一个accessToken的txt文件  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucsbmfnkfj60aw05g74d02.jpg)
