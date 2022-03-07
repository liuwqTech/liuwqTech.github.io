---
title: Electron安装
date: 2021-04-25 16:37:46
tags: Electron环境配置
categories: Electron
cover: https://gimg2.baidu.com/image_search/src=http%3A%2F%2F5b0988e595225.cdn.sohucs.com%2Fimages%2F20180107%2F4bf08721c7804960af61ff3a5311360d.jpeg&refer=http%3A%2F%2F5b0988e595225.cdn.sohucs.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1621932125&t=bfc1f446e98a359b2b79a15ad2bc7f5d
---

# **Electron环境配置——Electron安装**
## **一、普通安装**
- 打开终端，输入命令  
  `npm install electron --save-dev`  

  安装过程如下：  
![](https://tva1.sinaimg.cn/large/008i3skNgy1gpw32txis1j30a206kmy0.jpg)
![](https://tva1.sinaimg.cn/large/008i3skNgy1gpw33ldi9dj30a806o3zq.jpg)  

## **二、推荐使用淘宝加速镜像进行安装**  
- 终端输入：  
  `ELECTRON_MIRROR=https://cdn.npm.taobao.org/dist/electron/ npm install electron --save-dev`

## **三、验证Electron**  
- 1、如果npm版本大于5.2，则打开终端，输入：  
  `npx electron -v`

- 2、如果npm版本小于5.2，则打开终端，输入：  
  `./node_modules/.bin/electron -v`  

## **四、打开Electron**  
- 打开终端，输入：  
  `./node_modules/.bin/electron`  
    
  打开后，如下：  
  ![](https://tva1.sinaimg.cn/large/008i3skNgy1gpw3abk86bj309s06m3z5.jpg)

