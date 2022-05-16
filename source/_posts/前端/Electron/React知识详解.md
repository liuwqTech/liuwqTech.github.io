---
title: 神奇的React
date: 2021-05-17 23:03:46
tags: React
categories: 
- 前端
- Electron
cover: https://tva1.sinaimg.cn/large/008i3skNgy1gqc8o3iau2j30od0jomxb.jpg
---

# **神奇的React**
## React的特点 
1、声明式写法  
- React可以非常轻松的去创建用户界面，为你的应用的每一个状态去设置一个简洁的视图，有改变的时候，React也能高效的去更新这个渲染界面。  
- 声明式写法是强调结果的，与之对应的是命令式编程，是强调过程的。  

2、组件化
- React是基于组件的一个设计流程，组件让代码的复用、测试、分离都变得非常简单。  
  

3、一次学习，随处编写  
- 无论使用什么技术栈，我们都能随时引入React开发新特性。React可以开发Web程序、原生手机应用，系统应用、命令行工具等等。  
- 当你学习一项工具之后，能胜任多种工作，何乐而不为呢！


## 为什么要学习React 
1、大厂加持   
2、简单易懂  
3、最流行的，使用人数最多，开发人员最喜欢的

## React开发环境配置
**重要！！确保自己电脑上C++的编译环境已安装好，不然会报错！**  
1、打开命令行终端，输入安装指令：
`npx create-react-app react-hooks`  
(此条命令适用于npm 5.2+)  

- 推荐：进入自己的项目文件夹，输入`create-react-app react-hooks ` 

![](https://tva1.sinaimg.cn/large/008i3skNgy1gqltoaoi0uj30vm09gwfj.jpg)

![](https://tva1.sinaimg.cn/large/008i3skNgy1gqltoszcxoj30vu0jaabt.jpg)  

2、至此，我们的React开发环境已经配置好了，我们来运行一下试试！
- 进入项目文件夹，输入`npm start`，我们将会在本地3000端口看到内置的一个欢迎页面！如下图:  

![](https://tva1.sinaimg.cn/large/008i3skNgy1gqltsmf030j30vq078t96.jpg)  

![](https://tva1.sinaimg.cn/large/008i3skNgy1gqltsr5l3gj31ar0u0gmt.jpg)

**题外小知识：npx是什么？**   
1、它可以避免安装全局模块
用完就会删掉，不会存在浪费的情况  
2、可以调用项目内部安装的模块，例如：调用之前项目中nodemon模块  
- 进入项目目录，命令行输入：`node_modules/.bin/nodemon --version`  
- 直接使用npx命令，输入：`npx nodemon --version`