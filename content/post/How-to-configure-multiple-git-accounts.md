---
date: "2018-06-02T08:36:54-07:00"
title: "如何在Mac上配置两个git帐号"
draft: false
---

前言：我想有相当一部分程序员拥有多个git帐号，比较常规的一种情况可能是两个git帐号：一个办公，一个私用。这就意味着我们需要用不同的帐号来操作不同的代码仓库（公司的和个人的）。本篇文章就想和大家分享一下，如何在本地配置两个不同用途的git帐号。

大体步骤如下：
>

1. 清除git全局设置
2. 生成新的SSH keys
3. 添加并识别新的SSH keys私钥
4. 添加新的SSH keys到Git账号的SSH设置中

### 1. 清除git的全局设置
查看目前的全局设置情况

```Shell
$ git config --list     
                                                                                
credential.helper=osxkeychain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
user.name=parkeryao
user.email=parkeryao.work@gmail.com
```

取消相应帐号的全局设置

```Shell
$ git config --global --unset user.name "parkeryao"
$ git config --global --unset user.email "parkeryao.work@gmail.com"
```

检查设置是否生效

```Shell
$ git config --list                                                                                      
credential.helper=osxkeychain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
```

### 2. 生成新的SSH keys
把之前和git相关的所有key都删除掉

```Shell
$ ssh-keygen -t rsa -C "parkeryao.work@gmail.com"
$ ssh-keygen -t rsa -C "gary.ylyao@gmail.com"


$ cd ~/.ssh
$ ls -l                                                                                                  
total 56
-rw-------  1 Gary  staff  1675  6  2 21:38 id_rsa_gary.ylyao_github
-rw-r--r--  1 Gary  staff   402  6  2 21:38 id_rsa_gary.ylyao_github.pub
-rw-------  1 Gary  staff  1675  6  2 21:37 id_rsa_parkeryao.work_github
-rw-r--r--  1 Gary  staff   406  6  2 21:37 id_rsa_parkeryao.work_github.pub
-rw-------  1 Gary  staff  5843  3 30 12:24 known_hosts
-rw-r--r--  1 Gary  staff  1331  3 17  2017 known_hosts.old
```

### 3. 添加并识别新的SSH keys私钥
```Shell
$ ssh-agent bash                                                                                         
bash-3.2$ ssh-add ~/.ssh/id_rsa_gary.ylyao_github
Identity added: /Users/Gary/.ssh/id_rsa_gary.ylyao_github (/Users/Gary/.ssh/id_rsa_gary.ylyao_github)
bash-3.2$ ssh-add ~/.ssh/id_rsa_parkeryao.work_github
Identity added: /Users/Gary/.ssh/id_rsa_parkeryao.work_github (/Users/Gary/.ssh/id_rsa_parkeryao.work_github)
```

### 4. 添加新的SSH keys到Git账号的SSH设置中
```Shell
$ pbcopy < id_rsa_gary.ylyao_github.pub
$ pbcopy < id_rsa_parkeryao.work_github.pub
```

### 5. 配置~/.ssh/config文件

```Shell
#primary gitHub user(gary.ylyao@gmail.com)
 Host git@github.com
 HostName https://github.com
 User git
 IdentityFile ~/.ssh/id_rsa_gary.ylyao_github

#secondary(personal) gitHub user(parkeryao.work@gmail.com)
 Host git@github.com
 HostName https://github.com
 User git
 IdentityFile ~/.ssh/id_rsa_parkeryao.work_github
```



	                                                                                    
The agent has no identities.

