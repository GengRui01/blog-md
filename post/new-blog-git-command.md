---
title: "更新博客会用到的git命令"
tags: ["Git", "Hugo"]
date: 2018-07-24
---

更新博客会用到的git命令

<!--more-->

首先指定主题以及目标网址生成最终页面

```git
$ hugo --theme=hugo-nuo --baseUrl="https://GengRui01.github.io/"
```

执行上述语句之后所有静态页面都会生成到 public 目录，将pubilc目录里所有文件 push 到master 分支

```git
$ cd public
$ git init
$ git remote add origin https://github.com/GengRui01/GengRui01.github.io.git
$ git add -A
$ git commit -m "third commit"
$ git push -u origin master
```