title: "Ruby元编程（二）-对象模型"

date: 2015-10-15 21:38:35

tags: ruby

categories: ruby
----------------

### 打开类

#### 概念

首先让我们看一段代码

```
class D
	def x
		puts 'x'
	end
end

class D
	def y
		puts 'y'
	end
end

obj = D.new
obj.x           # => "x"
obj.y           # => "y"
```

在这段代码中，当第一次提及`class D`时，Ruby会创建一个新的`class`，并定义`x`方法。 当第二次提到`class D`时，因为`classD`已经存在了，所以ruby不会再去创建，而是重新 打开这个已经存在的类，并为之定义y方法。

所以说，`class`的任务并不是每次都定义新的`class`，其主要任务是把你带到`class`的上下文 中，让你可以在里面定义方法。因此，在Ruby中，我们可以重新打开已经存在的类并可以对他进行 动态的修改，这种行为被称作`打开类`

#### 打开类的问题

在平时的工作中，我们使用`打开类`这种技巧，可以方便的去修改Ruby中原有的一些类库。 在其中定义自己的方法。然而，它也有自己的不足之处。当我们粗心的在一些类库中定义自己的 方法时，如果方法名相同，会覆盖当前类中原有的方法，因此会造成代码中，其他引用类库原方法的 地方报错。所以记住，小心不要重名！小心不要重名！小心不要重名！

### 类的真相

#### 对象中有什么

再来我们看一段代码：

```
class MyClass
 def my_method
  @v = 1
 end
end

obj = MyClass.new
obj.class          # => MyClass
```

我们可以通过`irb`查看obj的内部

```
obj.my_method
obj.instance_variables   # => [ :@v ]
obj.methods        # => [ :my_method ]
```

我们发现在对象中包含实例变量和方法

##### 实例变量

在Ruby中，对象的类和它的实例变量没有关系，当给实例变量赋值时，他们就突然出现了 。因此对于同一个类，可以创建具有不同实例变量的对象。例如，当你没有调用过 obj.my_method方法，obj对象就不会有任何实例变量。

##### 方法

共享统一各类的对象也共享同样的方法，因此方法必须存放在类中，而非对象中。 换句话说，实例变量存放在对象中，而方法存放在类中。（类的实例方法，对象的实例变量）

##### 小结

一个对象的实例变量存在于对象本身之中，而一个对象的方法存在于对象 自身的类中。这就是同一个类的对象共享同样的方法，但不共享实例变量的原因。

#### 常量

任何以大写字母开头的引用（包括类名和模块名）都是常量。常量和变量的最大的区别在于他们的作用域 （scope）不同。

```
module MyModule
  MyConstant =  'Outer constant'

  class MyClass
    MyConstant = 'Inner constant'
  end
end    
```

##### 常量的路径

像文件路径一样，常量也可以通过路径来标识。常量的路径用双冒号进行分隔。

```
module M
  class C
    X = 'a constant'
  end
  C::X          #=>"a constant"
end
M::C::X        #=>"a constant"
```

#### 命名空间

```
module Rake
    class Task
    # ...
```

现在Task类的全名就变成Rake::Task,像Rake这样只是用来充当常量容器的模块，被称为命名空间。
