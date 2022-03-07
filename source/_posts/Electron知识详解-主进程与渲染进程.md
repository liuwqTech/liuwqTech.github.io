---
title: Electron知识详解-主进程与渲染进程
date: 2021-04-26 00:28:46
tags: Electron知识详解
categories: Electron
cover: https://tva1.sinaimg.cn/large/008i3skNgy1gpwghhyke3j31300own1w.jpg
---

# **Electron知识详解-主进程与渲染进程**
## **一、什么是process-进程**  

## **二、什么是thread-线程**  
- 线程是操作系统能够进行运算调度的最小单位。它被包含在进程之中，是进程中的实际的运作单位。  
- 可以用下面这张图来形象的表示进程和线程之间的关系。  
  
  ![](https://tva1.sinaimg.cn/large/008i3skNgy1gpwgk1dhcpj310s0gmab1.jpg) 

## **三、进程和线程之间的区别**  
- 内存使用方面的区别：  
  进程与进程相互之间的内存不能共享，而进程中的各线程的内存是共享该进程的。  
- 通信机制方面的区别：  
  进程通讯可以使用ipc，  
  线程共享进程的内存，通信起来十分方便。  
- 量级方面的区别：  
  线程更为轻量级

## **四、Electron主进程和渲染进程**  
- Elecrtron采用的是chrome一样的多进程机制的。（可通过资源管理器对他们进行比较） 
- 以下这张图可以形象的看出主进程和渲染进程之间的关系：
   
  ![](https://tva1.sinaimg.cn/large/008i3skNgy1gpwgp6logoj30y20l2dgc.jpg)  

### **1、主进程-Main Process**  
- 可以使用和系统对接的Electron API-创建菜单、上传文件等；  
- 创建渲染进程-Renderer Process；  
- 全面支持Node.js；  
- 只有一个，作为整个程序的入口点。 
  
### **2、渲染进程-Renderer Process**  
- 可以有多个，每一个对应一个窗口；  
- 每个都是一个单独的进程；  
- 全面支持Node.js和DOM API；  
- 可以使用一部分Electron提供的API。  
  
## **五、主进程与渲染进程的异同**  
- 两者可以访问的范围如下图所示： 
   
  
  ![](https://tva1.sinaimg.cn/large/008i3skNgy1gpwghhyke3j31300own1w.jpg)