# 本博客介绍

---

> 本意想记录下自己的学习历程，也希望因此能给同样想学习了解前端的同学朋友们提供帮助。本人目前仍是在读大学生，可能不能定时的对博客进行定时更新，请见谅。（作者朋友也参与进来编写随笔后端）如果此博客能对您们有些许帮助，请多多支持！谢谢！

!> 现在我又做了一个基于个人的[博客网站](http://strk2.cn/)，网址是http://strk2.cn，以后前端的博客文章基本都会更新到上面
- [前端](FrontSide/VUE.md)

  - [Vue 基础](FrontSide/VUE.md)
  - [Vue-Cli](FrontSide/VueCli.md)
  - [Vue-Router](FrontSide/VueRouter.md)
  - 数据结构和算法
    - [数据结构与算法](FrontSide/dataStructures.md)
    - [刷题日记](FrontSide/algorithm.md)
  - [开发随笔](FrontSide/Proj-essay.md)

- **一些关于前端各种问题的思考文章：**
  - [事件循环](FrontSide/eventloop.md)
- [后端](ServerSide/Servlet.md)
- [计算机基础](docs/FundamentalsOfComputer/Network.md)

# 开发规范

---

各位 container 成员，最基本的 git 流程：

- `clone`远程仓库
- `git checkout -b 分支名字` 创建新分支

  !> 注意，分支的名字建议为： `你的用户名/你的功能`

- 在新分支写完代码后，`add`、`commit`、`push`三部曲。

此时，如果团队成员已经改了远端仓库，就需先

> `git checkout master` // 切换到 master
> `git pull origin master` // 拉取远端分支以获取最新内容
> `git checkout test` // 切换到自己的分支
> `git rebase master` --------①
> 或者 `git merge master` ----------②

!> 注意：commit 的时候写清楚自己做了什么

- 然后在 github 上请求合并 `pull request`

因为用的是`docsify`，所有在本地可以通过`docsify serve`来启动服务器进行预览

!> 前提是必须装了`node`以及`docsify`模块包，包安装命令：`npm i docsify -g`
