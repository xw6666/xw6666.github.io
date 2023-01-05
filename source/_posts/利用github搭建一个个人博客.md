---
title: 利用GitHub搭建一个个人博客
date: 2023-01-05 15:33:31
tags: 杂项
categories: 杂项
---

本文通过使用Hexo博客框架搭建静态博客，Hexo博客搭建简单，可以直接使用MarkDown直接写文章，然后Hexo将文章编译成静态文件就直接发布了，所以文章内容没有存放在数据库内。

# 1.准备条件

- GitHub账号

- 域名（非必须）

  

# 2.新建GitHub仓库

首先在GitHub任何页面右上角，使用+下拉菜单选择New repository

![1](D:\code\xw6666\source\images\博客搭建教程\1.png)

这里的仓库名称设置为 <mark>username.github.io</mark>，一定是你的用户名+github.io，例如我的用户名是xw6666，所以我建立的仓库名为<mark>xw6666.github.io</mark>

![2](D:\code\xw6666\source\images\博客搭建教程\2.png)

# 3.为仓库设置SSH—Key

## 3.1创建密钥对

在git bash中输入以下命令

```
cd ~/. ssh
ssh-keygen -t rsa -C "邮件地址"
```

其中邮箱地址是你的github邮箱

然后连续三次回车，最终会生产文件在用户目录下，打开用户目录中的.ssh文件夹找到id_rsa.pub文件，如下图所示

![3](D:\code\xw6666\source\images\博客搭建教程\3.png)

使用记事本打开id_rsa.pub并复制其中的所有内容

之后打开你的GitHub主页点击右上角头像->Settings->SSH and GPG keys

选择New SSH key 

![4](D:\code\xw6666\source\images\博客搭建教程\4.png)

将复制内容粘贴此处，Title随便写，之后点击Add SSH key

![5](D:\code\xw6666\source\images\博客搭建教程\5.png)

## 3.2测试是否添加成功

输入以下命令：

```
ssh -T git@github.com
```

如果看到以下类似内容，则测试成功

![6](D:\code\xw6666\source\images\博客搭建教程\6.png)

## 3.3配置git信息

```
git config --global user.name "用户名"
git config --global user.email  "邮箱"
```

此处一定要填写你的GitHub用户名和GitHub绑定的邮箱
