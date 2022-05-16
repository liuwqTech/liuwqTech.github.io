---
title: L11-验证JS-SDK
date: 2021-09-21 20:21:46
tags: 微信公众号、前端
categories: 
- 前端
- 微信公众号开发
cover: https://tva1.sinaimg.cn/large/008i3skNly1guoimvpq4zj60vo0bi75802.jpg
---

# **L11-验证JS-SDK**

## 生成JS-SDK使用的签名 

1、创建一个Wechat实例对象  

- 在wechat.js中抛出Wechat，暴露出来

- 在app.js中引入Wechat模块  

![](https://tva1.sinaimg.cn/large/008i3skNly1guoipfzwqyj615s03kdfv02.jpg)  

2、index.js中添加url  

![](https://tva1.sinaimg.cn/large/008i3skNly1guoipugua2j6146088q3o02.jpg)  

3、生成JS-SDK使用的签名  
- a、组合参与签名的四个参数：jsapi_ticket（临时票据）、noncestr（随机字符串）、timestamp（时间戳）、url（当前服务器地址）
  
- b、将其进行字典序排序，以"&"拼接在一起
  
- c、进行sha1加密，最终生成signature  

![](https://tva1.sinaimg.cn/large/008i3skNly1guoir5k3c3j619s0u0dj702.jpg)  

4、将签名渲染到界面上  

![](https://tva1.sinaimg.cn/large/008i3skNly1guoirign71j615e08u74n02.jpg)  



## 使用JS-SDK

1、绑定域名

- 在接口测试号页面上填写js安全域名接口

- 去掉http协议，将后面的内容配置进去  

![](https://tva1.sinaimg.cn/large/008i3skNly1guoisl2yrqj61ls0ewgmy02.jpg)  

2、引入js文件

- 在需要调用JS接口的页面引入如下JS文件，(支持https)：`http://res.wx.qq.com/open/js/jweixin-1.6.0.js`
  
- 如需进一步提升服务稳定性，当上述资源不可访问时，可改访问：`http://res2.wx.qq.com/open/js/jweixin-1.6.0.js(支持https)`

- 备注：支持使用 AMD/CMD 标准模块加载方法加载  

![](https://tva1.sinaimg.cn/large/008i3skNly1guoiu6oeaij618i058t9i02.jpg)  

3、通过config接口注入权限验证配置

- 所有需要使用JS-SDK的页面必须先注入配置信息，否则将无法调用  

![](https://tva1.sinaimg.cn/large/008i3skNly1guoiukv6kej614a0d6gmr02.jpg)  

![](https://tva1.sinaimg.cn/large/008i3skNly1guoiurhlg8j61mm0g8jtn02.jpg)  

4、通过ready接口处理成功验证  
- `wx.ready(function(){});`

- config信息验证后会执行ready方法，所有接口调用都必须在config接口获得结果之后，config是一个客户端的异步操作，所以如果需要在页面加载时就调用相关接口，则须把相关接口放在ready函数中调用来确保正确执行。对于用户触发时才调用的接口，则可以直接调用，不需要放在ready函数中。  

![](https://tva1.sinaimg.cn/large/008i3skNly1guoivjq2f1j617w090jsf02.jpg)  

5、通过error接口处理失败验证  
- `wx.error(function(res){});`

- config信息验证失败会执行error函数，如签名过期导致验证失败，具体错误信息可以打开config的debug模式查看，也可以在返回的res参数中查看，对于SPA可以在这里更新签名。  

![](https://tva1.sinaimg.cn/large/008i3skNly1guoiw5q32ij617k07mq3l02.jpg)  


6、接口调用说明
- 所有接口通过wx对象(也可使用jWeixin对象)来调用，参数是一个对象，除了每个接口本身需要传的参数之外，还有以下通用参数：
  
   -  fail：接口调用失败时执行的回调函数。
    
   - complete：接口调用完成时执行的回调函数，无论成功或失败都会执行。

   - cancel：用户点击取消时的回调函数，仅部分有用户取消操作的api才会用到。
    
   - trigger: 监听Menu中的按钮点击时触发的方法，该方法仅支持Menu中的相关接口。

- 备注：不要尝试在trigger中使用ajax异步请求修改本次分享的内容，因为客户端分享操作是一个同步操作，这时候使用ajax的回包会还没有返回。

- 以上几个函数都带有一个参数，类型为对象，其中除了每个接口本身返回的数据之外，还有一个通用属性errMsg，其值格式如下：

   - 调用成功时："xxx:ok" ，其中xxx为调用的接口名

   - 用户取消时："xxx:cancel"，其中xxx为调用的接口名
    
   - 调用失败时：其值为具体错误信息