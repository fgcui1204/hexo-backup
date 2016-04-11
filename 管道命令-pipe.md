title: "管道命令(pipe)"

date: 2016-01-22 23:50:17

tags: 管道命令

categories: linux

---

## 什么是管道命令

管道命令操作符是：”|”,它能将由前面一个指令传出的正确输出信息，也就是`standard output`的信息,传递给下一个命令，作为下一个命令的标准的输入，即`standard input`.
> With pipes, the standard output of one command is fed into the standard input of another.

![pipe1.png](/img/pipe1.png)

- 管道后面接的第一个数据必须是‘命令’，而且这个命令必须要能够接收`standard input`的数据才行。这样的命令才是管道命令。
- 管道命令仅会处理`standard output`，对于`standard error output`会忽略。

## 常用的管道命令

### 选取命令: 

#### cut

cut主要功能在于将同一行里面的数据进分解，选出其中的一部分。

- 例如，可以使用下面的命令将环境变量输出来，并且找到其中的第五个路径。

```
## -d 后面是分隔符，-f是根据-d的分隔符讲一段信息切割成数段，取出第几段的意思，如果想取出多段，用','隔开
echo $PATH | cut -d ':' -f 5
```

`cut`还有一个功能就是使用`-c`参数可以取出固定字符中间的数据.

- 例如，我们取出`export`数据之后，从1-10之间的字符。我们可以使用下面的命令实现

```
export | cut -c 1-10
```

#### grep

另一个，很棒的也是经常使用的选取命令是`grep`，它能够解析一行文字，去的关键字，如该行存在关键字，就会将整行取出来。

- 例如，我想查找当前目录下，含有`back`关键字的文件或文件夹


```
ls | grep 'back'
```

- 我想查找不包含`back`的文件夹


```
ls | grep -v 'back'
```

- `grep`也支持选取某个文件的内容。例如我想找出`package.json`文件中，包含`hexo`的那几行。


```
grep --color=auto 'hexo' ./package.json
```
 ** 注意：如果加上`--color=auto`的选项，找到的关键字部分会用特殊颜色显示。**
 
### 排序命令
 
#### sort

`sort`可以帮我们进行排序，而且可以依据不同的数据类型来排序。

- 例如，个人账号都记录在`/etc/passwd`下，可以通过下面的命令将账号进行排序

```
cat /ect/passwd | sort
```

- 个人账号是以':'分隔的，我们可以通过`-k`参数指定按照哪一列排序

```
cat /ect/passwd | sort -t ':' -k 3
```

#### uniq

如果有很多数据，想要将重复的数据仅列出一个显示，我们可以通过`uniq`命令实现。

```
uniq [-ic]
-i : 忽略大小写
-c : 进行计数
```
- 使用`last`将账号取出，仅去除账号列，进行排序后仅取出一位，并统计登录的次数

```
last | cut -d ' ' -f 1 | sort | uniq -c
```

#### wc

`wc`这个命令可以帮助我们查看某个文件里面有多少字，多少行。

```
wc [-lwm]

-l : 仅列出行
-w : 仅列出多少字
-m : 多少字符
```

- 例如，查看`package.json`文件中有多少行，字，字符数

```
cat ./package.json | wc
```


 
最后给大家分享一个非常实用的小工具:[tldr](https://github.com/tldr-pages/tldr), 它可以帮助我们方便的查看一些命令的实用方法。