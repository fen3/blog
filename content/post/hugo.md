+++
title = '使用Hugo'
date = 2023-10-15T22:21:24+08:00
draft = false
+++

## 安装
因为用windows，直接下载了.exe文件，设置path变量（或直接放在项目的文件夹里）

## 创建个项目
```shell
hugo new site blog
```

## 主题
```shell
git clone https://github.com/spf13/hyde.git themes/hyde
```
接着在hugo.toml里面增加
```javascipt
theme = hyde
```

## 创建文章
```shell
hugo new posts/learn-hugo.md
```
找到 content/post/learn-hugo.md ，并写上内容

## 启动hugo服务器
```shell
hugo server -D
```
根据提示打开网址，就可以看到网站了



