---
title: 在Github Actions中通过FTP部署文件到服务器
date: 2020-09-29
tags:
 - Github
categories: 
 - Other
---

当我们部署静态网页网站，可以使用OSS的云存储服务来部署，这样可以直接走CDN访问，相当不错。当每次本地打包再上传，会比较麻烦。所以我们可以通过github action来自动部署。废话不多说，直接上代码。

```yml
# This workflow will do a clean install of node dependencies, build the source code and run tests across different versions of node
# For more information see: https://help.github.com/actions/language-and-framework-guides/using-nodejs-with-github-actions

name: Build and Publish Front End Framework Website

on:
# 当接收到push或者pr请求时触发更新
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]
jobs:
  FTP-Deploy-Action:
    name: FTP-Deploy-Action
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    
    - name: Use Node.js 12.x
      uses: actions/setup-node@v1
      with:
        node-version: '12.x'
        
    - name: Build Project
      run: |
        npm install
        npm run build
        
    - name: List output files
      run: ls
    - name: FTP-Deploy-Action
      uses: SamKirkland/FTP-Deploy-Action@3.1.1
      with:
        ftp-server: ftp://v0.ftp.upyun.com/
        ftp-username: ${{ secrets.FTP_USERNAME }}
        ftp-password: ${{ secrets.FTP_PASSWORD }}
        local-dir: dist

```
另外要新增一个文件 `.git-ftp-include`, 内容为 `!dist/`。告知要上传的目录，否则不会上传。

[官方文档](https://github.com/marketplace/actions/ftp-deploy)