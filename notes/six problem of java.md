---
title: six problem of java
date: 2016-12-27 10:14:17
tags:
---

#java中6个面点

* String
String str = "hello world";
我声明了什么? 回答通常是:一个string,内容是"hello world".这样模糊的回答是概念不清的根源.正确的答案:
这个语句声明的是一个指向对象的引用,名为"str",可以指向
类型为string的任何对象,目前指向"hello world"这个string
类型的对象.**我们只是声明了一个只能指向string对象的引用
变量**.
String string = s;
这里声明了另外一个只能指向string对象的引用,名为string,
并没有**第二个对象产生**,string还是指向原来那个对象,也就是
和str指向同一个对象

* "=="和equals()
==操作符专门用来比较变量的值是否相等.
> int a = 10;
int b = 10;
a==b 将是true.
但是,
String a = new String("foo");
String b = new String("foo");
a==b 将是false.

跟据**对象变量其实是一个引用**,他们的值是指向对象所在的内存地址,
而不是对象本身.a和b都使用了new操作符,
内存中产生了两个内容为"foo"的字符串,
他们位于不同的内存地址.a和b的值其实
是两个不同的内存地址的值,所以使用"=="
操作符,结果会是false.不过,他们的内容
都是"foo",应该是"相等",但是==操作符
并不涉及到对象内容的比较

对象内容的比较,正是equals方法做的事.

```
boolean equals(Object o){
	return this==o;
}
```
Object 对象默认使用了==操作符.如果自创的类没有覆盖equals方法,
那你的类使用equals和使用==会得到同样的结果.

* String到底变了没有

没有.因为String被设计成不可变(immutable)类,所以他的所有对象都是
不可变对象.
```
String s = "hello";
s = s + "world";
```
s所指向的对象是否改变了?从第一个问题就容易导出这个结论.s原来指向一个string对象,内容是"hello",然后对s进行了+操作,那么s所指向
的对象是否发生了改变?(**没有**) 不过,s不再指向原来那个对象了,而
指向了另一个string对象,内容为"hello world",原来那个对象还存在内存中.

* final关键字到底修饰了什么?

final使得被修饰的变量"不变",但是由于对象型变量的本质是"引用",使
得"不变"也有两种含义:引用本身的不变,和引用指向的对象不变
> 引用本身的不变： 
final StringBuffer a=new StringBuffer("immutable"); 
final StringBuffer b=new StringBuffer("not immutable"); 
a=b;//编译期错误 

> 引用指向的对象不变： 
final StringBuffer a=new StringBuffer("immutable"); 
a.append(" broken!"); //编译通过 

**final只对引用的值(它指向的那个对象的内存地址)有效, 
它迫使引用只能指向初始指向的那个对象,改变他的指向会导致编译期错误.至于它锁指向的对象的变化,finally是不负责的. 和==操作符类似, ==操作符只负责引用的"值"相等,至于这个地址所指向的对象内容是否相等,==操作符不管**

> **理解final问题有很重要的含义。许多程序漏洞都基于此----final只能保证引用永远指向固定对象，不能保证那个对象的状态不变。在多线程的操作中,一个对象会被多个线程共享或修改，一个线程对对象无意识的修改可能会导致另一个使用此对象的线程崩溃。一个错误的解决方法就是在此对象新建的时候把它声明为final，意图使得它“永远不变”。其实那是徒劳的。**

* instanceof是什么?

instanceof是java的一个二元操作符,和==,>,<是同一类.他的作用是检测
他左边的对象是否是它右边的类的实例,返回boolean类型的数据

```
String s = "I am an object";
boolean isobject = s instanceof Object;
```

说完本质,来说说实际用处.
> 我们写一个处理账单的系统,其中有这样三个类:
public class Bill{...}
public class PhoneBill extends Bill{...}
public class GasBill extends Bill{...}
在处理程序里有一个方法,接收一个Bill类型的对象,计算金额.
假设两种账单计算方法不同,而传入的Bill对象可能是两种中的
一种,所以要用instanceof来判断:
public double calculate(Bill bill){
	if(bill instanceof PhoneBill){
		//计算电话账单
	}
	if(bill instanceof GasBill){
		//计算燃气账单
	}
}
这样可以用一个方法处理两种子类

> 然而，这种做法通常被认为是没有好好利用面向对象中的多态性。其实上面的功能要求用方法重载完全可以实现，这是面向对象变成应有的做法，避免回到结构化编程模式。只要提供两个名字和返回值都相同，接受参数类型不同的方法就可以了： 
public double calculate(PhoneBill bill) { 
//计算电话账单 
} 
public double calculate(GasBill bill) { 
//计算燃气账单 
} 

**所以，使用instanceof在绝大多数情况下并不是推荐的做法，应当好好利用多态。**
