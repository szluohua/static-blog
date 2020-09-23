---
title: 在bash中通过密钥文件登录服务器
date: 2020-09-09
tags:
 - Bash
categories: 
 - Other
---

在`.ssh` 目录下新建一个名为`config`的文件,并填写相关的信息

```
    Host crm
    HostName 10.0.0.10
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/tm_dev
    User admin
    Port 22
```

配置信息注释:

```
    Host 服务器别称
    HostName ip或者域名
    PreferredAuthentications publickey
    IdentityFile 密钥文件路径
    User 登录的用户
    Port 22
```

登录：

```
    ssh crm
```