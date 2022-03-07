---
title: Electron知识详解-BrowserWindow
date: 2021-04-26 18:58:46
tags: Electron知识详解
categories: Electron
cover: https://tva1.sinaimg.cn/large/008i3skNgy1gpzvg3jq4kj314o0hs0tx.jpg
---

# **Electron知识详解-BrowserWindow**
## **一、准备**  
- 首先，按照官网，快速创建一个项目：  

![](https://tva1.sinaimg.cn/large/008i3skNly1gpxcn2ndwgj31fg0bqdg0.jpg)

## **二、小Tips**  
- 程序入口为main.js，所以当main.js有改变的时候，每次执行我们需要关闭命令，重启一遍，这样就会给我们编程带了较大的麻烦。所以我们该怎么解决这个问题呢？
- 解决办法：我们需要下载一个辅助工具nodemon，他可以监控文件的变化，自动运行命令，这样就省去了手动操作，非常方便。  
- 安装：进入项目文件夹，输入命令：`npm install nodemon --save-dev`  
- 安装完成后，打开项目的package.json，可以看到在deDependencies下多了一项nodemon。 

![](https://tva1.sinaimg.cn/large/008i3skNly1gpxcqqvtv6j30d2048jr9.jpg)  

- 接着在启动脚本scripts下修改start为`nodemon --watch main.js --exec \"electron .\"`,如下图所示：

![](https://tva1.sinaimg.cn/large/008i3skNly1gpxcuw6jgwj30p4056mx3.jpg)

## **三、BrowserWindow**  

 1、首先，创建index.html文件  

![](https://tva1.sinaimg.cn/large/008i3skNly1gpxcwlmmi0j30vu0jcmxt.jpg)  

 2、打开main.js文件，进行编辑

![](https://tva1.sinaimg.cn/large/008i3skNly1gpxcymulc1j30t60jadg9.jpg)  

- 首先，从electron引入app和BrowserWindow
- 接着，实例化BrowserWindow，命名为mainWindow
- 设置高为800，宽为600
- 启用node功能
- 最后，主页面加载index.html文件  
  
 3、运行结果    

![](https://tva1.sinaimg.cn/large/008i3skNly1gpxd7n2to7j31410u0jri.jpg)  

## **四、创建第二个窗口**  

 1、首先，创建second.html文件  

![](https://tva1.sinaimg.cn/large/008i3skNly1gpxd977ixzj30ni0ggjrm.jpg)

 2、打开main.js文件，进行编辑  

![](https://tva1.sinaimg.cn/large/008i3skNly1gpxd9nokrsj30l80b8aa7.jpg)  

- 首先，实例化BrowserWindow，命名为secondWindow
- 接着，设置高为400，宽为300
- 启用node功能
- 设置为附属于mainWindow
- 最后，主页面加载second.html文件  
  
 3、运行结果  

![](https://tva1.sinaimg.cn/large/008i3skNly1gpxdbb8mwaj313z0u0q61.jpg)