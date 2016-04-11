title: "如何修改某次commit过的信息"
date: 2016-01-06 16:49:35
categories: git
tags: git 
---

今天想要和大家分享的是如何修改某次commit过的信息，这里分两种情况，一种是修改最近一次commit的message，另外一种是修改之前某次commit过的message.

## 修改最近一次commit的message

> 我们可以通过`git commit --amend`修改最近一次commit的message.

比如，我最近一次commit的message如下：

![lastest_commit.jpg](/img/lastest_commit.jpg)

使用命令
```
git commit --amend
```
会看到如下的结果

![amend.jpg](/img/amend.jpg)

现在的message是可以编辑的，修改好了，保存即可，然后push到github上。

** 如果你已经push了最近一次commit,当你再次push的时候，你可能需要加上`--force` **

## 修改之前某次commit的message

> 我们可以通过`git rebase -i HEAD~n`(n表示要显示最近n次提交的记录)

我们执行
```
git rebase -i HEAD~4
```
会看到如下结果。
![rebase_commit.jpg](/img/rebase_commit.jpg)

我们可以将`pick`用`reword`替换掉，保存。

![reword.jpg](/img/reword.jpg)

然后可以输入要修改的message的内容

![update_message.jpg](/img/update_message.jpg)

保存即可，然后通过`git push --force`push到github上



