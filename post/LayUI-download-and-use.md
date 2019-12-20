---
title: "LayUI下载及使用"
tags: ["LayUI"]
date: 2018-09-03
---

LayUI下载及使用

<!--more-->

先去官网下载LayUI的压缩包（官网地址：[https://www.layui.com/](https://www.layui.com/)）

下载到的zip包解压后可以看到如下目录：

![image](/media/posts/LayUI-download-and-use/1.png)

将红色框中文件夹整个复制到前端的目录下

![image](/media/posts/LayUI-download-and-use/2.png)

前端的大佬说html、css、js三个要分开目录，所以我建了如下四个目录，以及网站首页index.html：

![image](/media/posts/LayUI-download-and-use/3.png)

然后在index.html贴入官网开发文档中提供的如下代码：

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
    <title>开始使用layui</title>
    <link rel="stylesheet" href="../layui/css/layui.css">
</head>
<body>

<!-- 你的HTML代码 -->

<script src="../layui/layui.js"></script>
<script>
    //一般直接写在一个js文件中
    layui.use(['layer', 'form'], function(){
        var layer = layui.layer
            ,form = layui.form;

        layer.msg('Hello World');
    });
</script>
</body>
</html>
```

就看到了如下的界面：

![image](/media/posts/LayUI-download-and-use/4.png)

这就算是搞定了一个基本的入门界面