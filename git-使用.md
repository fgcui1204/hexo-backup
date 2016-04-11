title: "git 使用"

date: 2015-03-09 23:55:03

categories: git

tags: git基本命令
------------------

###git分支

-	新建分支：

```
git branch <branch_name>
```

-	切换分支：

```
git checkout <branch_name>
```

-	删除本地分支：

```
git branch -d <branch_name>
```

-	查看分支：

```
git branch
```

-	查看远程分支

```
git branch -a
```

-	删除远程分支：

```
git push origin --delete <branch_name>
```

-	重命名本地分支：

```
git branch -m <old_branch_name> <new_branch_name>
```

-	推送本地分支

```
git push (-u) origin <branch_name>
```

-	撤销上一次commit

```
git reset HEAD~1
```

参考： http://rongjih.blog.163.com/blog/static/335744612010112562833316/
