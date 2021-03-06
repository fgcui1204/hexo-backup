title: "JAVA——继承"

date: 2015-07-07 22:45:16

tags: java

categories: java
----------------

java学习之继承
==============

###继承的概念

继承是从已有的类中派生出新的类，新的类能重用已有类的数据属性和方法，并同时可以有自己的新的方法和属性。这样可以很容易的复用以前的代码。 比如可以先定义一个类叫Person，Person有姓名，年龄，性别三个属性，而又由Person这个类派生出Student和Teacher两个类，给Student添加一个属性clszz，而给Teacher添加一个属性TeachYear（教龄）。

###继承的实现语法

```
<修饰符> class <子类名> extends <父类名>{
.....
}
```

###继承的好处

-	减少代码冗余
-	维护变得简单
-	扩展变得容易

###注意点

-	java 和某些面向对象语言(如c++)在实现继承的不同之处在于java类只支持单继承，不支持多重继承，java的接口可支持多继承。
-	使用extends关键字实现单继承。
-	实现继承关系的类之间有着必然的联系。不能将不相关的类实现继承，就像人类不能继承汽车类。
-	怎么去判断两个类之间有着必然联系呢？实际上，当两个类有着共同的属性或者方法时，那么这两个类之间就有可能有着共同的父类或者两个类之间存在继承关系。
-	构造方法不能被继承，只能被调用。一个类得到构造构造方法只有两种途径：自定义构造方法；使用缺省构造方法。但是，可以在子类中调用父类的构造方法。父类构造函数只能在子类构造函数中通过 super 显示调用，并且必须是第一句
-	java中所有的类都有一个通用的父类，Object，这个是JVM自动加上去的。

###构造函数的继承

当子类继承一个父类时，构造子类时需要调用父类的构造函数，存在三种情况

-	父类无构造函数或者一个无参数构造函数，子类若无构造函数或者有无参数构造函数，子类构造函数中不需要显式调用父类的构造函数，系统会自动在调用子类构造函数前调用父类的构造函数
-	父类只有有参数构造函数，子类在构造方法中必须要显示调用父类的构造函数，否则编译出错
-	父类既有无参数构造函数，也有有参构造函数，子类可以不在构造方法中调用父类的构造函数，这时使用的是父类的无参数构造函数

###一个简单的例子

A类：

```
class A{
  int num = 1;
  public A(int i){
      System.out.println("这个是A类");
      System.out.println("i="+i);
      num = i;
  }
}
```

B类继承A类

```

class B extends A{
  int num = 0;
  public B(){
    super(10);
    System.out.println("bbbbbbbb");
    System.out.println("numB="+num);
    System.out.println("numA="+super.num);
  }
}
```

C类包括main函数

```
public class C{
  public static void main(String[] args){
    new B();
  }
}
```

运行结果：

```
这个是A类
i=10
bbbbbbbb
numB=0
numA=10
```
