---
title: setting
date: 2021-07-07 12:39:07
categories:
- note
tags: 
- git 
-  hexo 
-  butterfly
cover: https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/202305251435947.png
---



![image-20210707130820709](https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/202305251435947.png)

# Git

只针对github

```
git config --global http.https://github.com.proxy localhost:10809

git config --global --unset http.https://github.com.proxy
```

为git开启代理

```
git config --global https.proxy localhost:10809
git config --global http.proxy localhost:10809

git config --global --unset http.proxy
```

关闭 crlf转换

```bash
git config --global core.autocrlf false     //禁用自動轉換
```

Git升级

```bash
git update-git-for-windows
```

# Hexo

- [Hexo doc](https://butterfly.js.org/posts/4aa8abbe/)

## hexo highlight

```html
<!DOCTYPE html>
<html>
<head>
<meta  charset=utf-8"/>
    <style>
        mark {
            background-color:#FFFF00 ; font-weight:bold;
        }
    </style>
```

## 多电脑协同

#### 增加电脑时

```bash
git clone address-of-your-site

cd address-of-your-site

npm install # due to the exist of gitignore, we should install module
```

#### 不同电脑提交

```bash
git pull # 同步

git add -A

git commit -m ""

git push 

hexo clean

hexo g

hexo d
```

# Typora

```html
<div style="page-break-after:always"></div>
```



```
AppData\Roaming\Typora\typora-user-images
```

