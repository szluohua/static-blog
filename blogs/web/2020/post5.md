---
title: 多个github账号，提交代码注意事项
date: 2020-09-09
tags:
 - Git
categories:
 -  Web
---

假设已经创建了两个不同的github账号在本地，那么在提交代码时，需要指定origin url，否则会默认使用第一个账号进行提交，从而导致报错 `Please make sure you have the correct access rights
and the repository exists.` 。

本地 `config` 配置。`default` 为公司的账号，`myself` 为自己的账号。
```shell
# default
Host github.com
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa
# myself
Host ieit.github.com
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_163
```
当我从自己的账号创建了 `repo`，那么需要设置相应的`origin`，告诉git，使用哪个身份去提交代码

```bash
# clone 代码
git clone git@github.com:szluohua/static-blog.git
# 设置origin
git remote set-url origin git@ieit.github.com:szluohua/static-blog.git
```