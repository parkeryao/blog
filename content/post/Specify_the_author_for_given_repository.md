---
title: "用不同的github帐号提交代码到不同的github仓库"
date: 2018-06-04T19:59:24+08:00
draft: false
---


我有两个github帐号，一个用于工作，一个用于个人，所以在提交不同的仓库代码时，需要用到不同的github帐号。

虽然我已经按照上一篇文章的指引分别为两个github帐号建立了相关的key，但发现提交到github时，author不是我设置的两个github帐号中的任何一下，而是我本地的一个git帐号。

问题的根源在于：没有为单个代码仓库设置git author，如下是具体设置办法：

先查看有无设置git的author信息：

```Shell
git config -l                                                   Gary@GarydeMacBook-Air
...
user.email=xxx@xxx.com
user.name=xxx
```

并确认此处设置的author是不是你想用于提交代码的github帐号，如果不是，通过下面方法重新设置：

```Shell
git config user.email "author@xxx.com"
git config user.name "author"
```