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

首先在GitHub任何页面右上角，使用+下拉菜单选择New repository。

![](/images/博客搭建教程/1.png)

这里的仓库名称设置为 `username.github.io`，一定是你的用户名+github.io，例如我的用户名是xw6666，所以我建立的仓库名为`xw6666.github.io`

![](/images/博客搭建教程/2.png)

# 3.为仓库设置SSH—key

## 3.1创建密钥对

在git bash中输入以下命令

```
cd ~/. ssh
ssh-keygen -t rsa -C "邮件地址"
```

其中邮箱地址是你的github邮箱

然后连续三次回车，最终会生产文件在用户目录下，打开用户目录中的.ssh文件夹找到id_rsa.pub文件，如下图所示

![](/images/博客搭建教程/3.png)

使用记事本打开id_rsa.pub并复制其中的所有内容

之后打开你的GitHub主页点击右上角头像->Settings->SSH and GPG keys

选择`New SSH key` 

![](/images/博客搭建教程/4.png)

将复制内容粘贴此处，Title随便写，之后点击`Add SSH key`

![](/images/博客搭建教程/5.png)

## 3.2测试是否添加成功

输入以下命令：

```
ssh -T git@github.com
```

如果看到以下类似内容，则测试成功。

![](/images/博客搭建教程/6.png)

## 3.3配置git信息

```
git config --global user.name "用户名"
git config --global user.email  "邮箱"
```

此处一定要填写你的GitHub用户名和GitHub绑定的邮箱。

# 4.环境安装

## 4.1安装node.js

在官网选择对应的系统安装则可，记得安装时要勾选写进环境变量中https://nodejs.org/en/

安装完成后，首先进行重启！！！

重启完后在命令提示符中输入以下命令查看是否安装成功

```
npm --version
```

如果显示了版本号则成功，进入下一步

## 4.2安装Hexo

```
npm install -g hexo-cli
```

安装完后输入hexo，如出现提示符则安装成功，无命令则安装失败。

# 5.初始化项目

## 5.1创建项目

使用hexo命令行在你想要的目录下创建项目目录

```
hexo init {name}
```

这里的name就是项目名字，和你的github用户名相同即可，比如我的就是：

```
hexo init xw6666
```

不出意外你所在目录下会出现一个新的文件夹，这个文件夹就是项目文件

## 5.2编译生成HTML代码

首先进入刚才生成的文件夹内，在cmd下或者git bash下输入以下命令：

```
hexo g
```

## 5.3本地运行项目

输入以下命令本地运行项目

```
hexo s
```

之后项目会默认跑在[http://localhost:4000/](http://localhost:4000/)下，打开浏览器进入会看到你的博客例如：

![](/images/博客搭建教程/7.png)

# 6.部署项目

部署项目以后，别人就能访问到你的网站了，hexo给了我们命令让我们可以直接无脑部署。

## 6.1设置部署地址

打开项目根目录下的`_config.yml`文件，拉到最底下可以看到Deployment，把新建好的复制过来，指定分支为mastero保存退出即可。

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/xw6666/xw6666.github.io.git
  branch: master
```

## 6.2安装git部署插件

需要额外在项目目录下安装一个部署插件，如果不安装会报错，安装插件的命令如下：

```
npm install hexo-deployer-git --save
```

## 6.3部署项目

执行如下命令即可：

```
hexo -d
```

如果此时出现连接不上github而导致失败的情况，可以参考以下两篇博客：

https://blog.csdn.net/Hodors/article/details/103226958

https://blog.csdn.net/weixin_40908748/article/details/122367878

## 6.4访问测试

成功后，这时候我们访问一下 GitHub Repository 同名的链接，比如我的`xw6666`博客的 Repository 名称取的是`xw6666.github.io`，那我就访问 [http://xw6666.github.io](http://xw6666.github.io/)，这时候我们就可以看到跟本地一模一样的博客内容了，如果看不到不要着急，github有延迟，等几分钟就行了。

## 6.5备份源码

因为是静态博客，所以我写的文章都在本地，那我换了电脑之后就没了，所以需要备份，备份很简单，新建一个其他分支，然后全push上去就行了，例如我创建了一个source分支保存源码。

```
git init    //初始化项目
git checkout -b source    //创建并切换到source分支
git add -A    //添加所有文件到暂存区
git commit -m "init blog"    //提交并注释
git remote add origin git@github.com:xw6666/xw6666.github.io.git    //添加到远程仓库
git push origin source    //将代码提交到远程的source分支
```

## 6.6配置主题

默认的主题肯定是不好看的，可以去网上找一套主题，之后把主题的文件夹放在项目下的`themes`文件夹中，并在项目根目录的`_config.yml`中找到`theme: `，填入你的主题文件夹名字即可。

# 7.域名解析

具体可以参考这篇文章，这里不再赘述

https://blog.csdn.net/lijing742180/article/details/85549978



# 8.写文章

博客搭建好了，可以在本地写文章上传。

## 8.1生成文章

在项目根目录中输入以下命令：

```
hexo new "你要写的文章标题"
```

之后hexo会自动在`source/_posts`下生成一个markdown文档，在里面写就行了。

## 8.2上传文章

在根目录输入以下命令：

```
hexo clean   //清除一些缓存
hexo g -d    //编译文件并上传至云端
```

成功之后在你的网站等几分钟就更新了。

## 8.3备份文章

养成好习惯，写完一篇文章上传以后立马备份：

首先先切换以下分支，我的备份分支叫`source`

```
git checkout source
```

之后就是git三板斧：

```
git add .  //将当前目录下的所有内容加入
git commit -m "注释"  //提交并注释
git push origin source   //推送到远端分支
```

