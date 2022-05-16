---
title: Electron实战-本地文件的读写
date: 2021-05-11 15:51:46
tags: Electron实战
categories: 
- 前端
- Electron
cover: https://tva1.sinaimg.cn/large/008i3skNgy1gqejh4bt7pj30g40g4aa4.jpg
---

# **Electron实战-本地文件的读写**
## 知识准备  
我们所熟知的Node.js中提供了一组类似于UNIX（POSIX）标准的文件操作API，为我们的文件操作带来了巨大的方便。同时，在fs文件系统中，模块中的方法均有异步和同步版本，例如，读取文件内容的函数有异步的 fs.readFile() 和同步的 fs.readFileSync()。

异步的方法函数最后一个参数为回调函数，回调函数的第一个参数包含了错误信息(error)。所以我个人建议大家使用异步方法，比起同步，异步方法性能更高，速度更快，而且没有阻塞。

其中，对于文件操作有这么几类常用的函数：

- `fs.readFile()` ：读取文件
- `fs.writeFile()`：文件写入
- `fs.appendFile()` ：追加写入
- `fs.rename()` ：文件名修改
- `fs.unlink()` ：文件删除
- 由于fs模块中并没有对文件的复制，但是我们可以通过以上操作自己封装一个：
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqek5qsdv2j30yk0nawf9.jpg" style="zoom: 50%;" />

而对于文件夹操作有这么几类常用的函数：

- `fs.mkdir()` ：目录创建
- `fs.chmod()` ：修改文件夹权限
- `fs.exists()` ：判断文件或者文件夹是否存在
- `fs.rmdir()` ：删除文件夹
- `fs.readdir()` ：读取文件夹
- `fs.stat()` ：显示文件的详细信息
- 由于fs模块中没有删除包含文件的文件夹的函数，因此我们仿写一个函数来删除包含文件的文件的函数：

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqekaz0itxj30yi0n8my7.jpg" style="zoom: 50%;" />

## 实战—本地文件读取
- 目标：创建一个本地txt文件，用electron实现将文件内容显示到页面文本框中
  
### 静态页面-index.html
- 在页面上添加一个文本框，以及一个读取按钮，并相应的设置好id，方便调用

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h0qgmrwqc3j213i0m0jt5.jpg"  />             

### 窗口创建-main.js
- 创建一个宽1600，高1200的窗口，并自动打开开发者工具调试界面
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqekiyshqhj312g0l0mxw.jpg" style="zoom: 50%;" />

### 渲染界面-renderer.js
- 首先，我们需要在开始导入fs模块，如下：
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqekjjimo6j30u802o3yb.jpg"  /> 

- 接着，用btn1绑定读取文件按钮，用list绑定文本框，同时，为按钮添加一个点击事件，利用fs.readFile()函数来读取txt文件内容，并将内容显示在文本框中
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqekk9acgwj31540r2gmo.jpg" style="zoom: 50%;" />

### 本地文件的创建
- 在项目根目录下，创建一个文本文件，内容为“好好学习，天天向上！”
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqekmt2kzej30vc09i3yd.jpg" style="zoom: 50%;" />

### 来看看效果
- 左边能看到我们的读取按钮和文本框，同时也打开了调试工具，可以看到控制台打印状况
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqeko3kn5cj31g50u00ur.jpg" style="zoom:33%;" />

- 我们点击“Read word”按钮，提示读取成功
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqekoam20tj31cy0r6jt0.jpg" style="zoom:33%;" />

- 此时，我们可以看到我们本地txt文本文档中的内容已经现在在界面的文本框中了，同时，控制台也有相应的信息输出
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqekp3tq7sj31fw0u00up.jpg" style="zoom:33%;" />

## 实战—本地文件写入  

### 静态界面-index.html
- 在页面上，我们添加一个保存按钮，用来将文本框中的内容保存到本地文件
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqeme08nt2j31160nu0tf.jpg" style="zoom: 50%;" />

### 渲染界面-renderer.js
- 首先，我们用btn2绑定到我们刚才创建的保存按钮上。
- 接着，为我们的btn2绑定一个点击事件，点击后，用text来获取文本框中的内容，然后通过调用fs.writeFile()函数来对本地文件进行写入
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqemejafq3j315y0tsta9.jpg" style="zoom: 50%;" />

### 来看看效果
- 我们可以看到界面上新增的一个按钮了
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqemhylprcj31g20u0dhx.jpg" style="zoom:33%;" />

- 接下来，我们在文本框中输入“我爱编程！”
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqemi4iypyj31g00u0wgj.jpg" style="zoom:33%;" />

- 输入完后，点击“Save word”按钮，提示写入成功，同时控制台又有相应的信息输出
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqemk9mg2dj316j0u0tb0.jpg" style="zoom:33%;" />

- 最后，我们转去本地目录，看一下我们输入的内容到底有没有写入到本地txt文本文档中去
  

<img src="https://tva1.sinaimg.cn/large/008i3skNgy1gqemkefkasj31940tsdhr.jpg" style="zoom: 33%;" />

-  **至此，我们用electron对本地文件进行读写的功能都已经实现了，各位小伙伴们快自己跟着练习一遍吧！**