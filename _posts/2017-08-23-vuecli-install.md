---
layout:     post
title: vue-cli笔记--安装
subtitle:   "Vue2"
date:       2017-08-23
author:     "handsameliu"
header-img: "img/post-bg-vue.jpg"
tags:
  - 总结
  - Vue2
  - vue-cli
---

要新开项目了，纠结于使用哪种框架，领导的意思是使用最简单的jquery，而我想要使用时下热门的vue。虽然还没有定调，那就先学学vue吧、

由于之前做过angular以及react的实际开发，所以对于vue这个两者结合，取其精华去其糟粕的框架觉得应该可以无痛对接。事实确实如此。

<!-- more -->

## 什么是vue？

按照官网的说法，vue是一套`渐进式框架`，与其他重量级框架不同的是，vue才是用的是自底向上的增量开发的设计。vue只关心视图层，不仅易于上手还有丰富的第三方框架整合支持（这里我要说一句，第三方框架有好的，但是坑的比较多。react和angular就不同了，资源非常丰富。也许这就是国内和国外的区别）。
另一方面，当与单文件组件和vue生态系统支持的库结合使用时，vue也能够为复杂的单页应用程序提供驱动支持。
vue自称是mvvm框架，但是它太过于注重视图层了，所以我觉得他可能是阉割版的mvvm库。
不过其基于数据驱动和组件化的思想构建，相较于angular和react更加简洁，更易于理解。
如果之前喜欢了jquery的dom操作，那么学习mvvm框架就请抛开操作dom的思维。框架通过特殊的html语法将dom和数据绑定在一起。一旦你创建了绑定，dom将和数据保持同步，每当变更了数据dom也会改变（可以参考vue官网的例子）。

## 安装

> 安装前提是需要先安装nodejs。以及一个命令行工具，可以使用git自带的gitbash。

首先全局安装vue-cli  `npm install -g vue-cli` ，过程中可能出现长时间无响应的情况，可以`CTRL+C`结束，重新运行安装程序，如果还不行可以将npm的源地址改为淘宝的源地址，推荐使用临时镜像的方式（淘宝的源版本可能稍稍落后一点点）。 `npm --registry https://registry.npm.taobao.org install -g vue-cli` 如果想要直接切换源可以上网查询 <http://riny.net/2014/cnpm/>

安装完成后可以在命令行运行 `vue -V` 查询版本号，如果有提示那么就是安装成功了。安装完成后我们就开始创建项目。

> 由于是vue-cli，所以一些必须的包已经集成了，所以无法再下载webpack。

## 创建项目

1.首先找到一个你理想的文件夹目录，`不推荐带有中文名的目录结构哦`。这里我们使用gitbash命令行工具执行。

2.在你的文件夹中运行 `vue init webpack my-project` 其中的`my-project`是项目的名称，可以随意起名但不能有中文。随后会弹出一串询问信息，根据询问信息的提示安装即可。如下：

```
vue init webpack my-project                               // 这个是那个安装vue脚手架的命令
This will install Vue 2.x version of the template.        // 这里说明将要创建一个vue 2.x版本的项目
For Vue 1.x use: vue init webpack#1.0 exprice
Project name (exprice)                                    // 项目名称
Project name exprice
Project description (A Vue.js project)                    // 项目描述
Project description A Vue.js project
Author Datura                                             // 项目创建者
Author Datura
Vue build (Use arrow keys)                                // 构建预设
Vue build standalone
Install vue-router? (Y/n)                                 // 是否安装Vue路由，也就是以后是spa（单页面应用需要的模块）
Install vue-router? Yes
Use ESLint to lint your code? (Y/n) n                     // 是否启用eslint检测规则，可以安装但是建议选no，除非你有良好的编码习惯，安装了也没事可以在webpack配置文件中注释掉相关项
Use ESLint to lint your code? Yes
Setup unit tests with Karma + Mocha? (Y/n)
Setup unit tests with Karma + Mocha? Yes
Setup e2e tests with Nightwatch? (Y/n)
Setup e2e tests with Nightwatch? Yes
vue-cli · Generated "my-project".
To get started:                                           // 启动服务的方式
cd my-project
npm install
npm run dev
```

如果安装程序没有退出，可以使用ctrl+c的方式退出，然后按照启动方式操作即可。或者手动进入到你创建的目录中，运行 npm install ，安装依赖包完成后运行 npm run dev 即可

由于时下流行的vue全家桶中有 `vue-resource` 这个包，但是刚才并没有安装，所以我们还需要手动安装一下 `npm install --save vue-resource`

安装完成后我们就可以使用编译器打开刚刚创建的项目目录了。如下

```
my-project
    |--build                        // 构建脚本目录
        |--build.js                 
        |--check-versions.js
        |--dev-client.js
        |--dev-server.js
        |--utils.js
        |--vue-loader.conf.js
        |--webpack.base.conf.js
        |--webpack.dev.conf.js
        |--webpack.prod.conf.js
        |--webpack.test.conf.js
    |--config                       // 构建配置目录
        |--dev.env.js
        |--index.js                 // 配置主文件
        |--prod.env.js
        |--test.env.js
    |--node_modules                 // 依赖包目录
    |--src                          // 主文件源码目录
        |--assets                   // 静态文件目录 我觉得有一个static就可以了。。。。
            |--log.png
        |--components               // 组件目录
            |--Hello.vue
        |--router                   // 路由目录
            |--index.js
        |--App.vue                  
        |--main.js                  // 页面配置
    |--static                       // 静态文件目录
        |--.gitkeep                 
    |--test                         // 测试文件目录，这里就不说了，因为我没有用到过。。。
    |--.babelrc                     // es6 es7文件转义配置文件
    |--.editorconfig                // 编译器的配置文件，我使用的是vscode编译器，其他编译器下的项目不会存在
    |--.eslintrc.js                 // 代码检查配置文件
    |--.gitignore                   // git屏蔽文件配置
    |--.postcssrc.js                // postCss配置文件
    |--index.html                   // 项目主页面
    |--package.json                 // 项目依赖描述文件
    |--README.md                    // 项目简介
```

`npm run dev` 运行后会自动打开页面，端口号是8080，你需要确保你的端口没有被占用才能正常启动。

当你每次对页面有所修改的时候，系统会自动打包build新的文件并刷新页面，非常方便。

