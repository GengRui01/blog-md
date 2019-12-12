---
title: "IDEA初次接触以及其中Gitee插件的使用"
tags: ["IDEA", "Gitee"]
date: 2018-07-27
---

用了三年多Eclipse，毕业了才听说还有个叫做IDEA的东西，真是纳闷它为什么会被团队小姐姐强烈推荐，下一个试试~

<!--more-->

安装什么的就不多说了，傻瓜式，直接下一步就好

安装好后就是如下界面：

![image](/media/posts/idea-gitee/1.jpg)

## IDEA配置Git

由于公司代码在gitee上，我得想法子给他导过来

第一步：点击“**Settings**”

![image](/media/posts/idea-gitee/2.jpg)

第二步：选择**Git** 找到git安装目录中**Git/bin/git.exe** 点击**OK**

![image](/media/posts/idea-gitee/3.jpg)

第三步：点击**Test**按钮测试，看到如下结果表示idea配置Git成功

![image](/media/posts/idea-gitee/4.jpg)

## IDEA安装gitee插件

第一步：点击“**Settings**”

![image](/media/posts/idea-gitee/5.jpg)

第二步：Plugins -> Browse repositories -> 搜索 Gitee或者Gitosc-> Install

![image](/media/posts/idea-gitee/6.jpg)

出现如下提示，点“Accept”

![image](/media/posts/idea-gitee/7.jpg)

第三步：安装好后会有如下提示，点击“Restart”重启IDEA就好

![image](/media/posts/idea-gitee/8.jpg)

## 第一个IDEA项目

第一步：新建一个project，选择好JDK安装路径，点击“Next”后，可以选择“hello world”项目，也可以不选

![image](/media/posts/idea-gitee/9.jpg)

第二步：点击next后会出现如下界面，选择好文件夹，文件夹的名称就是项目名，然后点“Finish”

![image](/media/posts/idea-gitee/10.jpg)

终于看到心心念念的Welcome了

![image](/media/posts/idea-gitee/11.jpg)

第三步：接下来对代码稍作修改，由于对快捷键不熟悉，直接右键选择“Run”，控制台打出左下角信息

![image](/media/posts/idea-gitee/12.jpg)

## 项目上传Gitee

第一步：点击导航栏 VCS -> Import into Version Control -> 托管项目到码云

![image](/media/posts/idea-gitee/13.jpg)

第二步：选择password，输入gitee帐号密码，点击Login。

![image](/media/posts/idea-gitee/14.jpg)

第三步：勾选Private，点击“Share”

![image](/media/posts/idea-gitee/15.jpg)

点击“OK”

![image](/media/posts/idea-gitee/16.jpg)

第四步：在项目上右键 ->Git ->Commit Directory

![image](/media/posts/idea-gitee/17.jpg)

填写备注信息，点击“commit”

![image](/media/posts/idea-gitee/18.jpg)

第五步：项目右键 -> Git -> Repository -> Push

![image](/media/posts/idea-gitee/19.jpg)

输入gitee账号密码

![image](/media/posts/idea-gitee/20.jpg)

第六步：登录gitee，进入project下，可以看到项目上传码云成功！

![image](/media/posts/idea-gitee/21.jpg)

## Gitee下载项目

第一步：check out from version control -> Git

![image](/media/posts/idea-gitee/22.jpg)

第二步：输入你要克隆的项目url，点击“Clone”

![image](/media/posts/idea-gitee/23.jpg)

出现如下提示点击“Yes”

![image](/media/posts/idea-gitee/24.jpg)

之后根据项目类型选择，就会克隆成功啦~~~