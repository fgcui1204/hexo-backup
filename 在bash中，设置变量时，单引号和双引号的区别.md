title: "在bash中，设置变量时，单引号和双引号的区别"

date: 2015-12-22 00:08:05

tags: linux 变量

categories: linux

---

在bash环境下，当你设置变量时，单引号和双引号的用途有何不同呢？ 其实，他们的最大的不同在于双引号仍然可以含有变量的内容，但单引号内只能是一般的字符串，而不会有特殊符号。

举例：假设你定义了一个变量：name=Fugang, 现在想以name变量的这个内容定义myname这个变量并显示Fugang is a good man这个内容，要如何设置呢？

![variable.jpg](/img/variable.jpg)

由此可见，使用单引号的时候，那么$name将失去原有的变量内容，仅为一般字符的显示类型而已。
