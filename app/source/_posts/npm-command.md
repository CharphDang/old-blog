---
title: npm常用命令
date: 2018-12-26 16:23:12
tags: 
- npm
categories:
- npm
comments: false
---

> 相信大家很多人，包括我自己，常常会对npm install --save-dev 、 npm install --save傻傻分不清楚,今天这里主要是对npm的常用命令进行一个总结。

___

> 首先，npm 是node.js自带的功能。查看npm版本

```js
npm -v
6.4.1
```

> 升级npm版本

```js
mac下: 
	sudo npm install npm -g
window下:
	npm install npm -g
```

> 使用npm安装模块

```js
npm install <Module Name>
eg: 使用npm安装node中的express框架
	npm install express
```

安装好之后，express包就会放在当前工程目录下的node_modules 目录中，因此在代码中只需要通过 **require('express')** 的方式就好，无需指定第三方包路径。

```js
const express = require('express');
```

> 默认方式为本地安装，还有一种是全局安装

```js
npm install express -g   # 全局安装
```

> 接下来，我们列举下容易混淆的方式（注意①②的不同）

```js
npm install X:

	- 会把X包安装到node_modules目录中

	- 不会修改package.json

	- 之后运行npm install命令时，不会自动安装X
    
npm install X --save:

    - 会把X包安装到node_modules目录中

    - 会在package.json的 ①dependencies 属性下添加X

    - 之后运行npm install命令时，会自动安装X到node_modules目录中

    - 之后运行npm install -production或者注明NODE_ENV变量值为production时，会自动安装msbuild到		  node_modules目录中
npm install X --save-dev:

    - 会把X包安装到node_modules目录中

    - 会在package.json的 ②devDependencies 属性下添加X

    - 之后运行npm install命令时，会自动安装X到node_modules目录中

    - 之后运行npm install -production或者注明NODE_ENV变量值为production时，不会自动安装X到			  node_modules目录中
```

> npm install 使用原则

```js
运行时需要用到的包使用--save，否则使用--save-dev。
```

> npm 卸载模块

```
npm uninstall X
卸载后，你可以到 /node_modules/ 目录下查看包是否还存在，或者使用以下命令查看：
npm ls
```

> 更新模块

```
npm update X
```

### 使用淘宝镜像

> 大家都知道国内直接使用 npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。淘宝 NPM 镜像是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。
>
> 你可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:

```js
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

注意： 如果使用的是cnpm，则只需要将第一个npm命令变为cnpm即可，其他和npm命令使用方式相同。