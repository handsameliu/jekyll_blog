---
layout:     post
title:  git突然无法连接提示公钥或密码错误的坑
subtitle:   "git"
date:       2017-08-23
author:     "handsameliu"
header-img: "img/post-bg-git.jpg"
tags:
  - 总结
  - git
  - ssh
---

ssh配置文件莫名其妙的出现了问题，可能由于在电脑上乱试版本管理工具导致的吧。

然后就是各种调，因为当前电脑会链接github和gitlab以及服务器，所以本地的ssh有点多，设置起来也费点时间。

<!-- more -->


那么今天遇到的问题就坑了

- ssh -T git@github.com		正常连接
- git pull origin master	提示密码不对无法连接
- 各种查看后发现项目.git文件中的config文件中的 `remote "origin"`地址有误一个`:`被改成了`/`。导致找不到目录，我一看所有我之前用SourceTree配置过的项目都存在这样的情况。估计是当初病急乱投医乱配导致的。以下是处理方法
- 可以先在指定的项目中 `git remote -v` 看到远程连接的地址，如果和你github中项目的ssh方式提供的网址一下那就不用往下看了。（这里的remote建议使用ssh的方式，就不用输密码了）
- 在命令行运行`git remote rm origin` 删除远程连接
- 在命令行运行`git remote add origin ssh方式的github项目地址` 重新添加一下即可

--------------------

前提：你要安装git，windowsCMD无法直接运行ssh命令。如果你之前配置过要重新配置相同网站的sshkey可以吧.ssh文件夹中的`known_hosts`改名或删掉，以免影响新的key

**以github为例**

首先需要直接打开gitbash，在命令行中输入`ssh-keygen -t rsa -C "建议填注册网站时填写的邮箱"`

> ssh-keygen -t rsa -C "841489295@qq.com"

然后会询问你生成的文件名，你可以指定也可以不指定（多ssh管理时建议指定）

然后回车就可以了，他就会在`C:\Users\电脑用户名\.ssh`生成一个你指定名称的rsa文件

取出已`.pub`为后缀的文件中的内容，添加到你的github或者gitlab以及其他以git版本管理为功能的网站中（一般叫SSHKey）

添加后还有重要的一步，就是测试一下是否能连接成功，在命令行输入`ssh -T git@github.com`, 会让你输入网站登录的密码。显示如下即为正确。

```
$ ssh -T git@github.com
The authenticity of host 'github.com (192.30.255.112)' can't be established.
RSA key fingerprint is SHA256:nThbg........
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.255.112' (RSA) to the list of known hosts.
Enter passphrase for key 'C:\\Users\\acer\\.ssh\\github-rsa':
Hi 你github上的用户名! You've successfully authenticated, but GitHub does not provide shell access.
```

操作完成后.ssh文件中的`known_hosts`文件中会写入一条你刚刚连接网站的记录。

多sshkey使用时，还有更重要的一步，需要配置.ssh文件中的`config`文件，`每多一个sshkey就要配置一次`

```
Host github.com              建议输入你的网站名  不建议重复
HostName github.com			 输入你网站的域名，既你克隆项目时，使用的ssh方式中url里@符号后面的地址
IdentityFile C:\\Users\\acer\\.ssh\\github-rsa			你的rsa文件地址
PreferredAuthentications publickey						照抄，意思是已公钥的方式
User handsameliu			 用户名，可以填你网站的用户名，也可以填网址 不建议重复
```

完成后就可以正常clone了。
