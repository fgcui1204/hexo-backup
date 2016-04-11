title: "如何将github repository迁移到一个新的repository中"

date: 2015-11-17 22:49:24

tags: github repository 迁移

categories: 工具

---

先描述一下场景，小A同学在github上建了一个repository，并写了一些代码，这时候，小B同学clone了这个项目，并做了一些修改，然后add,commit,push....突然看到提示，没有权限。最简单的办法给小A童鞋联系，给B同学这个权限。但是很多时候，是联系不上小A同学的，但是小B同学想保留原来小A的commit记录，所以不删除之前的`.git`文件。怎么办呢？这时候我们可以在自己的github上创建一个新的repository，然后push的时候，直接push到自己的repository中就阔以啦~

假设小B同学创建的的新的repository是b_student;

在commit完成之后，执行

```
git remote add origin_b<远程服务器名字> git@github.com:fgcui1204/b_student.git

git push -u origin_b master
```

就会将新的代码push到自己的repository中了。而且之前小A同学commit的记录还会保存。
