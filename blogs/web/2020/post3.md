---
title: Git配置http(s)协议与ssh协议代理
date: 2020-09-09
tags:
 - Git
categories:
 -  Web
---

为了加速 `git clone` 的速度，很多人都是通过查询`dns`之后，修改`hosts`文件，但这个方法限制于地方区域，有些地方能用，有些地方根本用不了。所以只能使用代理配置。以下总结了`git`代理`ssh`协议与`http`协议的方法。

### 1. 获取代理的socks5的端口

![image](https://jscode.top/api/v1/getFile?path=/article/vc-upload-1574129111148-2.png)

### 2 git配置ssh协议代理

进入.ssh目录，新增config文件，如果已经存在的话，直接编辑即可。新增一条 `ProxyCommand nc -X 5 -x 127.0.0.1:1086 %h %p`，如果本身不存在config文件的，直接复制以下内容进文件即可，但注意修改 **端口** 以及 **私钥文件路径**

```
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
    ProxyCommand nc -X 5 -x 127.0.0.1:1086 %h %p
```

如果是双github账号配置的话，同理，直接加上命令即可。

```
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa
    ProxyCommand nc -X 5 -x 127.0.0.1:1086 %h %p
Host ieit.github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/luo_rsa
    ProxyCommand nc -X 5 -x 127.0.0.1:1086 %h %p
```

### 3. git配置http(s)协议代理

在网上搜索到的都是配置 `http.proxy` 与 `https.proxy`。实际生效的是 `http.proxy`，并没有 `https.proxy` 这一条设置，这是没用的。

* 设置代理

```
git config --global http.proxy 'socks5://127.0.0.1:1086'
```

* 取消代理

```
git config --global --unset http.proxy
```

### 4. 补充通过ssh连接服务器，使用代理

假设ip是： 47.102.135.121

```
ssh root@47.102.135.121 -o "ProxyCommand=nc -X 5 -x 127.0.0.1:1086 %h %p"
```

参考文章：

[给SSH配置基于http\(s\)和socks协议的代理服务器](https://github.com/mrdulin/blog/issues/69)

[git 设置和取消代理](https://gist.github.com/laispace/666dd7b27e9116faece6)