---
title: NVM安装
date: 2021-04-25 00:41:46
tags: NVM
categories: 
- 前端
- 常用工具安装
cover: https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fwww.srcmini.com%2Fwp-content%2Fuploads%2F2019%2F07%2F639a9c0d19181a1.jpg&refer=http%3A%2F%2Fwww.srcmini.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1621877613&t=9edb094168234607c845c69b06823ed4
---

# **Electron环境配置——NVM安装**
## NVM、Node、NPM之间的区别
- NVM：node.js版本管理工具，即一个NVM可以管理很多Node版本和NPM 版本。   
- Node.js：在项目开发时的所需要的代码库  
- NPM：Node.js包管理工具。在安装的Node.js的时候，NPM也会跟着一起安装，它是包管理工具。NPM管理Node.js中的第三方插件。

## NVM、Node、NPM之间的关系 
- NVM管理Node.js和NPM的版本。NPM可以管理Node.js的第三方插件。  

## NVM安装过程 
### 打开终端，输入命令：  
curl：  
    `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash`  
wget：  
    `wget -q0- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash`  

安装过程图片
![图片1.png](https://i.loli.net/2021/04/25/hoNi96yHEVq37Ug.png)  

### 验证安装：  
- 打开终端输入 `nvm -v` 验证一下是否安装成功，当出现“Node Version Manager”时，说明已安装成功。  

![](https://www.hualigs.cn/image/6084545278528.jpg)  

## NVM常用命令 
- `nvm install stable` 安装最新稳定版node。  
- `nvm install <version>`  安装指定版本，可模糊安装，如：安装v4.4.0，既可nvm install v4.4.0，又可nvm install 4.4  
- `nvm uninstall <version>`  删除已安装的指定版本，语法与install类似  
- `nvm use <version>`  切换使用指定的版本node  
- `nvm ls`  列出所有安装的版本  
- `nvm ls-remote`  列出所有远程服务器的版本（官方node version list）  
- `nvm current`  显示当前的版本  
- `nvm alias <name> <version>`  给不同的版本号添加别名  
- `nvm unalias <name>`  删除已定义的别名  
- `nvm reinstall-packages <version>`  在当前版本 node 环境下，重新全局安装指定版本号的 npm 包
