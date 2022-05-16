---
title: L05-FetchAccessToken方法
date: 2021-09-11 17:45:46
tags: 微信公众号、前端
categories: 
- 前端
- 微信公众号开发
cover: https://tva1.sinaimg.cn/large/008i3skNly1guctz2t2yyj60wy0l841402.jpg
---

# **L05-FetchAccessToken方法**
1、用fetchAccessToken方法封装  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucu01r2d3j60yk0qy0uz02.jpg)  

2、使用async和await来处理异步问题，减少多次回调  
- await只有在async中才能使用

![](https://tva1.sinaimg.cn/large/008i3skNly1gucu0nm1suj60vh0u0ad202.jpg)  

3、优化  

- 将`resolve(res);`改写为`return Promise.resolve(res);`
- 目的：为了接下来可以继续写.then()  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucu2cndjsj610c02kglm02.jpg)  

- 将access_token挂载到this上  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucu2tygrfj610g09q0tq02.jpg)  

- 另一个小优化  

![](https://tva1.sinaimg.cn/large/008i3skNly1gucu38090ij611i0bwgmz02.jpg)