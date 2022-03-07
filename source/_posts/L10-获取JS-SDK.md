---
title: L10-获取JS-SDK
date: 2021-09-19 14:49:46
tags: 微信公众号、前端、web
categories: 微信公众号开发
cover: https://tva1.sinaimg.cn/large/008i3skNly1gulxuc6i2oj60iw0fgdgd02.jpg
---

# **L10-获取JS-SDK**

## **一、获取JS-SDK** 

1、用户访问服务器上一个节点时，我们返回一个页面给用户，需要设置一个路由

- 下载ejs，终端输入`npm i ejs`  
  
![](https://tva1.sinaimg.cn/large/008i3skNly1gulxyu7su6j60z80akmym02.jpg)  

- 配置模版资源目录（app.js）
  - `app.set('views', './views');`

- 配置模版引擎（app.js）
  - `app.set('view engine', 'ejs');`  

- 新建一个views文件夹，里面新建一个search.html，改为search.ejs  

![](https://tva1.sinaimg.cn/large/008i3skNly1guly0es1mpj61980g075i02.jpg)  

- 页面路由  

![](https://tva1.sinaimg.cn/large/008i3skNly1guly0xsdjcj615a06274l02.jpg)


2、访问测试
- 运行app.js，在浏览器输入内网穿透过的地址（或者是映射到的本地端口号），后加“/search”  

![](https://tva1.sinaimg.cn/large/008i3skNly1guly1u23z0j61g10u0wfx02.jpg)



3、使用微信给我们提供的开发工具——JS-SDK

- 微信JS-SDK是微信公众平台 面向网页开发者提供的基于微信内的网页开发工具包。通过使用微信JS-SDK，网页开发者可借助微信高效地使用拍照、选图、语音、位置等手机系统的能力，同时可以直接使用微信分享、扫一扫、卡券、支付等微信特有的能力，为微信用户提供更优质的网页体验。

4、jsapi_ticket

- 生成签名之前必须先了解一下jsapi_ticket，jsapi_ticket是公众号用于调用微信JS接口的临时票据。正常情况下，jsapi_ticket的有效期为7200秒，通过access_token来获取。由于获取jsapi_ticket的api调用次数非常有限，频繁刷新jsapi_ticket会导致api调用受限，影响自身业务，开发者必须在自己的服务全局缓存jsapi_ticket 。
- 成功返回如下JSON：

![](https://tva1.sinaimg.cn/large/008i3skNly1guly47o32aj61460aqgmk02.jpg)  

- 获得jsapi_ticket之后，就可以生成JS-SDK权限验证的签名了  

5、getTicket() ——获取jsapi_ticket  

![](https://tva1.sinaimg.cn/large/008i3skNly1guly62a7l5j612e0u0dih02.jpg)  

6、saveTicket(ticket)——用来保存jsapi_ticket  

![](https://tva1.sinaimg.cn/large/008i3skNly1guly6gfnrej618c0psdi902.jpg)  

7、readTicket()——用来读取ticket  

![](https://tva1.sinaimg.cn/large/008i3skNly1gulyafaiq5j616k0pa40i02.jpg)  

8、isValidTicket(data)——判断ticket是否过期  

![](https://tva1.sinaimg.cn/large/008i3skNly1gulycl8bbyj619s0rkq4u02.jpg)  

9、fetchTicket()——用来获取没有过期的ticket  

![](https://tva1.sinaimg.cn/large/008i3skNly1gulyd3p6ssj613m0u0n0a02.jpg)  
![](https://tva1.sinaimg.cn/large/008i3skNly1gulydbifnrj61ac0m876o02.jpg)  

10、测试

- 请求地址url要记得引入api.js  

![](https://tva1.sinaimg.cn/large/008i3skNly1gulyduarsuj60y60gkt9t02.jpg)  

- 结果如下，同时会生成一个ticket.txt的文件  

![](https://tva1.sinaimg.cn/large/008i3skNly1gulye6cezgj61dy0auta302.jpg)  

## **二、精简代码**  

- 我们会发现wirteFile和readFile两个方法比较重复，我们对其进行模块化  

### A、模块化wirteFile

- 在tools.js中新建一个writeFileAsync(data, fileName)方法，将原先wirteFile中的方法拷贝进来  

![](https://tva1.sinaimg.cn/large/008i3skNly1gulyjh2m7aj61400jwq4u02.jpg)  

- 接着，返回wechat.js，在saveAccessToken(accessToken)中调用writeFileAsync(data, fileName)方法

- saveTicket(ticket)类似  

![](https://tva1.sinaimg.cn/large/008i3skNly1gulyjwh5guj61340be75d02.jpg)  

### B、模块化readFile

- 在tools.js中新建一个readFileAsync(fileName)方法，将原先readFile中的方法拷贝进来  

![](https://tva1.sinaimg.cn/large/008i3skNly1gulykhgld2j61460jsq4q02.jpg)  

- 接着，返回wechat.js，在readAccessToken()中调用readFileAsync(fileName)方法

- readTicket()类似  

![](https://tva1.sinaimg.cn/large/008i3skNly1gulykw95syj613i08ut9c02.jpg)  

## **三、固定文件的存放位置**   

![](https://tva1.sinaimg.cn/large/008i3skNly1gulylt0l6ej615o0l4tb902.jpg)  

- 测试结果：accessToken.txt和ticket.txt将会保存在utils文件夹中
