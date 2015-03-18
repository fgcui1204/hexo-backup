title: "git 使用"
date: 2015-03-09 23:55:03
tags: git 学习工具
---

###git分支
- 新建分支：
```
git branch <branch_name>
```
- 切换分支：
```
git checkout <branch_name>
```
- 删除本地分支：
```
git branch -d <branch_name>
```
- 查看分支：
```
git branch
```
- 查看远程分支
```
git branch -a
```
- 删除远程分支：
```
git push origin --delete <branch_name>
```
- 重命名本地分支：
```
git branch -m <old_branch_name> <new_branch_name>
```
- 推送本地分支
```
git push (-u) origin <branch_name>
```
