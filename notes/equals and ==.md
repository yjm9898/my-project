---
title: equals and ==
date: 2016-12-22 12:27:18
tags: 每日一计
---

## ==和equals
下定义：equals()是方法，==是操作符。在java中不支持操作符重载，方法却是支持的。

下面来画重点：
```
        /**
         * 1.字符串比较
         */
        String str1 = "abc";
        String str2 = "abc";
        System.out.println(str1 == str2); //true
        System.out.println(str1.equals(str2)); //true

        str1 = new String("abc");
        str2 = new String("abc");
        System.out.println(str1 == str2); //false
        System.out.println(str1.equals(str2)); //true

        /**
         * 2.对象比较
         */
        Object obj1 = new Object();
        Object obj2 = new Object();
        System.out.println(obj1==obj2); //false
        System.out.println(obj1.equals(obj2)); //false
        obj1=obj2;
        System.out.println(obj1==obj2); //true
        System.out.println(obj1.equals(obj2)); //true
      

```

下面对字符串比较进行说明
> 由于jdk对string对象的equals方法进行了改写，首先判断对象是不是同一个，如果是返回true,如果不是继续判断两个对象的字面量是否相同，如果相同返回true.
 
> 在代码片段１中，大家都知道直接给string变量赋值的时候，str2和str1都是同一个对象。所以输出两个true.

> 当用new String()方式创建变量，jdk 会分配两个地址，所以==操作符输出了false,而equals先比较对象　在比较字面量，最后输出了true.

> 所以，== 操作符比较的是内存地址，而equals到底比较的是内存地址还是字面值或者属性值，需要视具体的对象而定，或者说是视具体对象实现的equals规则而定，不能简单的说是equals就是比较字面值。
