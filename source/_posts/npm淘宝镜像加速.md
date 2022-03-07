---
title: npm镜像加速
date: 2021-04-26 00:08:46
tags: Electron环境配置
categories: Electron
cover: https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=1953574012,3594304451&fm=26&gp=0.jpg
---

# **Electron环境配置——npm镜像加速**

## 推荐通过cnpm使用，且镜像设置为：`https://registry.nlark.com/`  


## **一、临时使用**  
命令后直接加：  
`--registry https://registry.npm.taobao.org ` 
- 例如：  
    
    `npm install expres --registry https://registry.npm.taobao.org`

## **二、永久使用**  
打开终端，输入：  
`npm config set registry https://registry.npm.taobao.org`  
    
- 验证配置是否成功，可以终端输入：  
  `npm config get registry`  
  或  
  `npm info express`

## **三、通过cnpm使用**  
打开终端，输入：  
`npm install -g cnpm --registry=https://registry.npm.taobao.org`   
  
  - 使用：  
    `cnpm install express`  

## **四、删除镜像**  
打开终端，输入：  
`npm config set registry https://registry.npmjs.org` 
