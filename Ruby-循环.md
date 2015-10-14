title: "Ruby 循环"

date: 2015-10-15 00:10:56

tags: 循环

categories: ruby
----------------

循环时的注意事项
----------------

-	循环的主体是什么
-	停止循环的条件是什么

实现循环的方法
--------------

### times方法

如果只是单纯的执行一定次数的处理，用times方法最简单。

```
5.times do
  puts "hello world"
end
```

### for语句

for语句不是方法，是ruby提供的循环控制语句

```
sum = 0
for i in 1..5
  sum += i
end
puts sum
```

### 普通的for语句

上一节介绍的是for语句的特殊用法，普通的for语句如下：

```
for 变量 in 对象 do
  希望的循环处理
end
**可以省略do
```

例如：

```
names = ["Ming", "Danny", "Freg"]
for name in names
  puts name
end
```

### while 语句

```
while 条件 do
  希望循环的处理
end
```

例如：

```
i = 1
while i < 2
  puts 1
  i +=1
end
```

### util 语句

util语句的判断条件和while相反。换句话说，while语句是一直执行循环，直到条件不成立 为止，而until语句是一直执行循环处理，直到条件成立为止。

```
sum = 0
i = 1
until sum >= 50
  sum += i
  i += 1
end

puts sum
```

### each 方法

each方法就爱那个对象集合里的对象逐个取出，这与for语句循环取出数组元素非常相似

```
names = ["Ming", "Danny", "Freg"]
names.each do |name|
  puts name
end
```

### loop方法

loop方法没有终止循环条件，只是不断执行循环处理。

```
loop do
  print "Ruby"
end
```

这样会造成死循环，经常在我们的程序里我们会使用`break`来终止循环。

循环控制
--------

在进行循环的语句中，我们可以使用一些关键字来终止循环，或者跳到下一个循环。ruby提供了三种 循环控制的命令 - break 终止程序，跳出循环 - next 跳到下一个循环 - redo 在相同的条件下重复刚才的处理
