---
layout:     post
title:		编写vue插件并上传到npm
subtitle:   "Vue2"
date:       2017-11-17
author:     "handsameliu"
header-img: "img/post-bg-vue.jpg"
tags:
  - 总结
  - Vue2
  - vue-cli
  - NPM
  - webPack
---

[https://github.com/handsameliu/vue-simple-calendar](https://github.com/handsameliu/vue-simple-calendar)  欢迎start，如果有问题，可以提issues。

以下是过程中遇到的问题。记录下来，以免再犯。

<!-- more -->

1、`npm adduser` 是注册。如果已经有npmjs的账号了那就直接使用`npm login`。（退出使用`npm logout`）

2、使用前一定要确认连接的镜像是官方的而不是淘宝的。
	获取当前镜像地址 `npm get registry`
	切换到官方 `npm config set registry https://registry.npmjs.org`
	切换到淘宝 `npm config set registry http://registry.npm.taobao.org`

3、在pulish前一定要确认，你所构建的项目名称是否有重复。如果有重复马上换掉。否则会报403错误。解决方法是`重命名package.json中的name`

```
$ npm publish
npm ERR! publish Failed PUT 403
npm ERR! Windows_NT 10.0.15063
npm ERR! argv "E:\\nodejs\\node.exe" "E:\\nodejs\\node_modules\\npm\\bin\\npm-cli.js" "publish"
npm ERR! node v6.11.0
npm ERR! npm  v3.10.10
npm ERR! code E403

npm ERR! You do not have permission to publish "vue-simple-calendar". Are you logged in as the correct user? : vue-simple-calendar
npm ERR!
npm ERR! If you need help, you may report this error at:
npm ERR!     <https://github.com/npm/npm/issues>

npm ERR! Please include the following file with any support request:
npm ERR!     E:\words_liu\vue-simple-calendar\npm-debug.log
```

至于如何编写及发布插件到npm参考这个就可以了。主要关注发布流程即可。[http://www.imooc.com/article/19691](http://www.imooc.com/article/19691)




