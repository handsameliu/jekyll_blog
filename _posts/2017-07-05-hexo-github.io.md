---
layout:     post
title: hexo+github.io建立轻博客
date:       2017-07-05
author:     "handsameliu"
header-img: "img/post-bg-js-version.jpg"
tags:
  - 教程
  - github
  - hexo
---

## 准备工作

<!-- more -->

使用hexo前需要安装nodejs以及git。可以到官方进行下载[nodejs][1]，[git][2]

### 设置github仓库

git安装完成后需要创建对应的仓库。可到 [github.com][5]

> **重要：**名称要起为 `你的github名称.github.io` 。这里建议你这么起名，因为github默认这样解析。你也可以起其他的名字，因为github已经对你的账户默认绑定了一个域名`你的github名称.github.io`，你起一个其他名字的话就算是单独的子网页了(访问的时候不方便)。

**重要：** 起好名字后需要到项目的设置`setings`中设置 `github pages`选项，可以直接设置`Source`的值为`master`。(貌似现在默认就是master了)。至于模版`Theme Chooser`不选也可以的。

上传前还要确定自己本地的git已经生成了公钥并且绑定了自己的github。

安装完成后就可以安装hexo了,最好先建立一个独立的文件夹然后在这个文件夹进行一下步骤

## 开始

### 安装hexo
```
$ npm install hexo-cli -g	# 全局安装
$ hexo init blog # 这个名字可以随便起，反正最后都要转移到git项目中
$ cd blog	
$ npm install	# 安装依赖
$ hexo g # hexo generate的简写
$ hexo s # hexo server的简写
```

完成以上步骤后命令行提示：`06:55:40.452 INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.` 后就可以在 `http://localhost:4000/` 查看了。会显示一个默认的页面。

> (以下这张图是配置好后的图片)。

![图片](/img/20170704151457.jpg)

### 更换默认主题

主题 `Next`（他的start最多，文档比较详细）[Next官方文档-开始使用][3]

进入到blog项目的根目录中运行以下命令

	git clone https://github.com/iissnan/hexo-theme-next themes/next

完成下载后进入到themes/next文件夹中安装所需要的包

	cd themes/next
	npm install

安装完成后修改`站点配置文件`, blog根目录下的`_config.yml`文件，将theme改为next

	theme: next

改好后启动项目验证修改情况。

	hexo clean  # 清空缓存（建议在有大的改动时运行一次）
	hexo g
	hexo s

此时即可使用浏览器访问 `http://localhost:4000` ，检查站点是否正确运行。你会看到一个全新的主题。

### 自定义主题内容

你可能觉得当前的主题并不好看，Next提供了三个主题任你选择

- Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
- Mist - Muse 的紧凑版本，整洁有序的单栏外观
- Pisces - 双栏 Scheme，小家碧玉似的清新

想要改变主题中的某些设置，你需要到主题的包中修改对应的设置。在themes/next中修改`_config.yml`文件

	#scheme: Muse
	#scheme: Mist
	scheme: Pisces

完成后可以重复上一步，验证修改情况。

页面此时的语言并非中文，出现中文会乱码，那么就需要在blog根目录下修改配置文件`_config.yml`了。

	language: zh-Hans

目前 NexT 支持的语言如以下表格所示：

|语言|代码|设定示例|
|:--|:--|:--|
|简体中文|zh-Hans|language: zh-Hans|
|English|en|language: en|
|Français|fr-FR|language: fr-FR|
|Português|pt|language: pt or language: pt-BR|
|繁體中文|zh-hk 或者 zh-tw|language: zh-hk|
|Русский язык|ru|language: ru|
|Deutsch|de|language: de|
|日本語|ja|language: ja|
|Indonesian|id|language: id|
|Korean|ko|language: ko|

与language一起的还有很多字段，顺便都可以设置一下的

	# Site
	title: 大标题
	subtitle: 子标题
	description: 自我介绍
	author: 作者
	language: zh-Hans
	timezone: 时区

现在头像也没有显示出来，同样也是在站点的配置文件中`_config.yml`中，字段是`avatar`。

该字段可同时接收完整的url和本地相对路径。

相对路径的文件目录是在`source`中，可以创建一个img文件夹存放图片，链接示例为`/img/avatar.png`

	avatar: http://example.com/avatar.png 或 /img/avatar.png

这时就可以设置菜单了，我们可以到blog根目录下的配置文件中`_config.yml`修改

它对应的字段是`menu`，找到后可以将你不喜欢的目录内容用#注释掉。内容的设置格式是：item name: link

	menu:
		home: /
		archives: /archives
		tags: /tags
		categories: /categories
		#about: /about
		#commonweal: /404.html

> 若你的站点运行在子目录中，请将链接前缀的 / 去掉

当前目录中除了有文章`archives`外还有其他两个链接，分别是`tags`和`categories`

tags代表了文章的标签，categories代表了文章的分类。这两个页面是所有标签和分类的汇总，当文章中包含对应的字段时编译器会自动编译到对应的页面中。

这里就要用到hexo的特定语法了，他只能对指定的markdown标签解析。

在blog根目录下使用 `hexo new page` 新建一个页面，命名为 `tags`

	hexo new page tags

刚刚新建的页面会在`source`文件夹下产生对应的文件夹及index.md文件。

可在index文件中修改type属性。将页面的类型设置为 tags ，主题将自动为这个页面显示标签云。

	title: 标签
	date: 2017-12-22 12:39:04
	type: "tags"
	---

同理，categories也是同样的操作。

在blog根目录下使用 `hexo new page` 新建一个页面，命名为 `categories`

	hexo new page categories

刚刚新建的页面会在`source`文件夹下产生对应的文件夹及index.md文件。

可在index文件中修改type属性。将页面的类型设置为 `categories` ，主题将自动为这个页面显示分类云。

	---
	title: 标签
	date: 2017-12-22 12:39:04
	type: "categories"
	---

完成以上两步菜单的所有链接就都可以点击了。

但是在tags和categories中是没有内容的。我们就来添加一下。

创建一个新的文章

	hexo new hello-world

这里我借用一下他人的 [hexo参考文档][4]

在文章开头加入以下代码

	---
	title: test
	tags:
	  - Testing
	categories:
	  - Testing
	---

重新执行 `hexo g` `hexo s` 查看网页效果，`tags`和`categories`中就会存在内容。

完成后即可推送到git仓库上

	hexo n #写文章
	hexo g #生成
	hexo d #部署 #可与hexo g合并为 hexo d -g  会将当前的markdown文档转换为html页面并上传到对应的git仓库中

这里还不能推送到git仓库上的，因为并没有指定对应的git仓库名称。配置一下git仓库地址就可以了，但前提是要有对应的仓库哦。

在blog根目录下的`_config.yml`文件中找到`deploy`字段，设置对应的值(这里使用的是我的git地址)

	deploy:
		type: git
		repo: git@github.com:handsameliu/handsameliu.github.io.git  # 这里一定要ssh格式，ssh格式可以到自己的git仓库中获取
		branch: master

[1]:https://nodejs.org/en/
[2]:https://git-scm.com/download/
[3]:http://theme-next.iissnan.com/getting-started.html
[4]:https://segmentfault.com/a/1190000002632530
[5]:https://github.com











