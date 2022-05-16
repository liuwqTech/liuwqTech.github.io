---
title: Electron知识详解-进程之间的通信
date: 2021-04-27 20:07:46
tags: Electron知识详解
categories: 
- 前端
- Electron
cover: https://tva1.sinaimg.cn/large/008i3skNgy1gpyk7s4l4nj30sa0kwgmg.jpg
---

# **Electron知识详解-进程之间的通信**
## 前提准备
1、首先，我们要知道，Electron是使用IPC在进程之间进行通讯的。下图呢就十分生动形象的表示了这种通讯方式。  
  <img src="https://tva1.sinaimg.cn/large/008i3skNgy1gpyk7s4l4nj30sa0kwgmg.jpg"  />  

2、那有没有什么方法可以让这些通信更为可视化呢？有的，那就是Devtron。接下来我们就来尝试着安装一下Devtron插件。  

- 进入Devtron官网，按照提示进行插件的下载安装。  
  ![](https://tva1.sinaimg.cn/large/008i3skNgy1gpykd71pauj318g09sdg5.jpg)  

- 安装完成  
  ![](https://tva1.sinaimg.cn/large/008i3skNgy1gpykdw4nfbj30vc0kejtp.jpg)  

- 验证安装-在main.js中添加一下代码：
  ![](https://tva1.sinaimg.cn/large/008i3skNgy1gpykf88xqkj30w80pmq3y.jpg)  

- 运行一下，打开devtron插件，如下图所示：
  ![](https://tva1.sinaimg.cn/large/008i3skNgy1gpykh1d8sgj31fs0u0tas.jpg)  

 **【重点！本人安装devtron后，运行会报错，原因为新版不兼容，我们可以安装旧版。】**
输入：`npm i electron@5.0.6 -D`，安装此版本运行即可。

## 代码实例1  
- 目标：在我们的页面上增加一个按钮，点击它，将我们的事件发送到主进程，然后主进程再进行回话，之后渲染进程将这个消息输出到界面。  

1、首先，在index.html中增加一个按钮，id设置为sendmessage，内容为Send message to main。 
  ![](https://tva1.sinaimg.cn/large/008i3skNgy1gpykkszcjlj30wo0ok3zf.jpg)  

2、接着，我们在renderer.js中添加一个点击事件：  
- 从electron引入ipcRenderer模块  
- 获取到按钮的id，在上面添加一个点击事件  
- 调用ipcRenderer中的sene方法来发送消息  

![](https://tva1.sinaimg.cn/large/008i3skNgy1gpzui1d651j312w0jw0ty.jpg)  

3、最后，我们在main.js中以事件的形式对消息进行接收：  
- 从electron中引入ipcMain模块  
- 将消息打印在控制台上  

![](https://tva1.sinaimg.cn/large/008i3skNgy1gpzuzqqhrkj30yo0miwff.jpg)  

4、运行结果：  
- 点击button，打开devtron，查看ipc  

![](https://tva1.sinaimg.cn/large/008i3skNgy1gpzv1gs87vj31b00kmdgc.jpg)  

- 同时，我们也可以在终端中看见我们打印出的消息：  

![](https://tva1.sinaimg.cn/large/008i3skNgy1gpzv2jyhlrj30vg0kgtbe.jpg)  

- **小Tip：我们也可用`console.log(event)`来打印出event事件**  

- 接下来，我们来要让主进程将消息发送回去：  
1、首先，我们对index.html进行编辑，添加一个消息接收id  

![](https://tva1.sinaimg.cn/large/008i3skNgy1gpzv6mfp6vj30wy0pg758.jpg)  

2、在main.js中，使用reply方法进行消息回复  
  ![](https://tva1.sinaimg.cn/large/008i3skNgy1gpzv78iqcej30w80nu75b.jpg)  

3、最后，在renderer.js中进行接收，并输出显示  

![](https://tva1.sinaimg.cn/large/008i3skNgy1gpzv8038myj31180ho3zb.jpg)  

4、运行结果：  
- 点击Send message to main，在页面上我们会看到从主进程发来的回复消息，同时打开devtron，我们也能监听到这两个发送和回复的过程。  

![](https://tva1.sinaimg.cn/large/008i3skNgy1gpzv8wl5v0j31g80u0gni.jpg)

## 代码实例2 
**除了通过ipc发送事件的方式，我们还有什么方法能够进程之间的通信呢？**  
- Remote，官网上是这么说的，remotr提供了一种简单的方式来进行渲染进程和主进程之间的ipc通讯。
有了remote这个模块，我们就不需要通过发送事件的方法来请求主进程做一些事情了，而是可以直接调用主进程中的一些方法。听起来很不错哦！！  

- 来，跟着我修改一下renderer.js:  
  1、运用remote模块，我们来导入一下BrowserWindow。  
  2、接着，我们实例化一个小窗口，来加载百度网页。  
  ![](https://tva1.sinaimg.cn/large/008i3skNgy1gpzvbjjxllj31060q8jsq.jpg)  

- 来一起看下运行结果：  
  ![](https://tva1.sinaimg.cn/large/008i3skNgy1gpzvckjai3j31g50u0dmz.jpg)    

- 哈哈哈，这样就可以在渲染进程中直接运用主进程中的方法了，是不是非常nice啊！快去试试！



