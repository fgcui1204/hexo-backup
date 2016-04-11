title: "Learn Git From Codeschool."

date: 2016-03-26 22:33:24

categories: git

tags: [git,codeschool]
----------------------
# Basic(基础部分)

## Staging & Remote
`git reset <file_name>` undo file staged
`git reset HEAD <file_name>` refer to last commit
`git checkout -- <file_name>` blow away all changes from last commit
`git reset --soft HEAD^` undo last commit and put changes into staging
`git commit --amend -m "message"` change the last commit
`git reset --hard HEAD^` undo last commit and all changes
`git reset --hard HEAD^^` undo last  2 commits and all changes

`git diff --stage` see difference for has staged

## push & merge

- when there are some conflict with your co-work's code,like :
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Our Cat-alog</title>
  </head>
  <body>
    <nav>
      <ul>
<<<<<<< HEAD
        <li><a href="cat.html">Cats</a></li>
        <li><a href="dog.html">Dogs</a></li>
=======
        <li><a href="cat.html">Felines</a></li>
        <li><a href="dog.html">Canines</a></li>
>>>>>>> 6a487f9eb0e0a5110bdf2a45a4f5dbcc3d4eec53
      </ul>
    </nav>
  </body>
</html>
```
 **your code** is from `HEAD` to `======`
**co-works code** is from `====` to `>>>>`

## Remote branches & tags
`git branch -r` list all remote branch
`git push origin :branch_name` delete remote branch
`git branch -D branch_name` delete local branch
`git remote show origin` show remote branches
`git remote prune origin` clean up deleted remote branches
`git push origin local_branch_name: remote_branch_name` push local branch to specify remote branch


`Tag` is a reference to a commit, used mostly for release versioning

`git tag` list all tags
`git checkout <tag_name>` checkout code at commit
`git tag -a <tag_name> -m <message>` add a new tag
`git push --tags` push new tags

## Rebase
![rebase](/img/rebase.png)
- How to rebase?
1. git checkout feature_branch
2. git rebase master
3. git checkout master
4. git merge feature_branch

- How to  rebase with remote branch
1. git fetch origin master
2. `git rebase` on feature branch

## Show history and config

`git log` view logs
`git log --pretty=oneline` view log one line
`git log --oneline -p ` view log and code difference in each commit
`git log --oneline -stat` view log and show lines different in each commit
`git diff HEAD` diff between last commit and current state
`git blame index.html --date short` see different and author in each line 
`git rm --cache file_name` not deleted from the local file system, only from git.
`git config --global alias.st status` git st <--> git status

---

# Advance(高级部分)

## Rebase

### Switch commit order
`git rebase -i HEAD~4`, 编辑前四次提交的commit.Switch顺序只需要手动去改变顺序就好

### Split commit

`git rebase -i HEAD~4`, 将`pick`改成`edit`,在pop出来的窗口中，`git reset HEAD^`

### Squash(合并) commit
`git rebase -i HEAD~4`, 将`pick`改成`squash`改成新的commit的信息

### [Update commit message](http://fugang.site/2016/01/06/%E5%A6%82%E4%BD%95%E4%BF%AE%E6%94%B9%E6%9F%90%E6%AC%A1commit%E8%BF%87%E7%9A%84%E4%BF%A1%E6%81%AF/)
`git rebase -i HEAD~4`, 将`pick` 改成`reward`，在pop出来的窗口中，写入新的`commit`的信息

## Stashing
`git stash save` 保存修改的文件，并且restore 上一次的commit
`git stash apply` Bring stashed file back
`git stash list` 列出所有保存的stash，即`stash stack`
`git stash drop <stash_name>`删除某个保存的`stash`

![Stash](/img/stash.png)

`git stash save --keep-index` 其中 `--keep-index`参数，会使已经staging的代码不会被stash

`git stash save --include-untracked` 其中`--include-untacked`参数，会把`untracked`状态的`file`添加到`stash`中

`git stash list --stat` 列出所有的stash以及每个的改动的文件

`git stash show --patch` 显示最近一个`stash`而且显示文件 的`diff`

`git stash branch <branch_name> <stash_to_pop>`会创建一个新的branch而且在新分支上pop代码

`git stash clear` clear all stashes



