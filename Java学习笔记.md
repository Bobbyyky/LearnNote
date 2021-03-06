# Java学习笔记

[TOC]



+++

## 第一章 类型

### 1.1类型转换

+++

+ java中基本数据类型的转换是根据数据的精度进行判断的
+ ***double > float > long > int > char > short > byte为精度从小到大的排序***
+ 当低精度转换为高精度自动完成转换，高精度转换为低精度需要转换运算，不然无法通过编译

### 1.2数组

+++

+ Java中数组的定义时`[]`可以写在数组名前，也可以写在数组名后，其中写在数组名前将会对该`[]`后的所有数组产生影响，写在数组名后只会对这一数组产生影响
+ 在定义时不可以在`[]`中直接定义数组的大小，若要定义数组的大小可以

```java
		/**
         * 这是申明数组的方法*/
        int [] test1, test2 [];
        /**
         * 等价于*/
        //int  test1 [], test2 [][];
        
        /**
         * 这是第一种定义数组大小的方式*/
        test1 = new int [2];
        test2 = new int[2][2];

        /**
         * 这是第二种定义数组大小的方式*/
        test1[0] = 1;
        test1[1] = 2;
        test1[2] = 3;
```

+ 数组访问时越界可以通过编译，但是运行时会报错
+ Java中多维数组不要求每一维的数组大小相同

+++

## 第二章 运算符

### 2.1运算符

+++

+ 在Java中数字进行运算时，结果类型为运算数字精度最高的类型，若是精度小于int，则按int进行运算

#### 2.1.1位运算:mahjong:

+ 位运算是直接对数字的二进制数进行操作，当数据运算量十分大时，采取位运算会极大的提升运算效率
+ 按位与`&`若两个数字的对应位都是1，则运算结果的相应位为1，否则为0
+ 按位或`|`若两个数字的对应位都是0，则运算结果的相应位为0，否则为1
+ 按位非`~`是单目运算符，若对应位为0，则运算结果的相应位位1，否则为1
+ 按位异或`^`若两个数字的对应位相同，则运算结果的相应位为0，否则为1

#### 2.1.2运算符综述

| 优先级 | 运算符                | 描述     |
| ------ | --------------------- | -------- |
| 1      | [ ]  ( )  .  ,   ;    |          |
| 2      | instanceof  ++  --  ! |          |
| 3      | *  /  %               |          |
| 4      | +  -                  |          |
| 5      | >>  <<  >>>           | 移位运算 |
| 6      | <  <=  >  >=          |          |
| 7      | ==  !=                |          |
| 8      | &                     | 按位与   |
| 9      | ^                     | 按位异或 |
| 10     | \|                    | 按位或   |
| 11     | &&                    | 逻辑与   |
| 12     | \|\|                  | 逻辑或   |
| 13     | ? :                   | 三目运算 |
| 14     | =                     |          |



+++

## 第三章 类和对象

### 3.1类

+++

+ 类是Java中实现对象的基础，使用`class`来完成对类的声明，例如

```java
class leiMing (/*参数列表*/){
    /*类的具体内容，有变量、方法*/
}
```

+ 在类中的成员变量对整个类都有效，与其他因素无关
+ 成员变量不进行初始化，会有默认值
+ 方法中声明的为局部变量，局部变量一定要初始化，因为局部变量没有默认值
+ 若是局部变量和成员变量命名相同，那么在这个方法中局部变量会覆盖成员变量，若要调用被覆盖的成员变量需要使用其它的方式

### 3.1对象的创建

+++

***若一个变量的数据类型是类，那么这个变量就是对象***

#### 3.1.1构造方法

+++

+ 构造方法是类中一种特殊的方法，它只在类创建对象时使用
+ 构造方法没有数据类型，它的名字一定要与类的名字相同
+ 可以有多个构造方法，要通过参数的不同进行区分
+ 类中会有一个默认的无参数、无语句的构造方法

#### 3.1.2实例化

+++

+ 对象在声明后要使用`new`来为对象分配实体，否则为空对象，无法使用
+ 若对象的引用完全相同，那么他们在内存空间指向的是同一空间

### 3.1.3参数传值

+++

+ 参数得到的值都是传递过去的，也就是说改变参数的值并不会对原来传递的值产生影响

### 3.2类和实例的区别

+++

+ ***使用`static`定义的为类的公共属性***
+ 类即为类中都拥有的，实例是有使用范围的，只对当前的对象负责
+ 在类被创建之后类变量和类方法就被创建了，而实例变量和实例方法只有在对象创建之后才被创建
+ 使用类名可以直接调用类变量和类方法
+ 对类变量的改变是全局的，而对实例变量的改变只对当前的对象有效，对其他对象无效
+ 实例方法可以操作类变量，但类方法不能操作实例变量

### 3.3转型:mahjong:

+++

* 转型分为上转型和下转型
* 转型就是父类的引用指向其子类对象（接口也是可以的）
* 创建一个父类对象其实例化是其子类完成的就是转型

```java
Father f1 = new Son(); //上转型

Father f2 = new Son(); //下转型
Son s1 = (Son)f2; 
```

* 上转型就是把子类对象变成父类对象，而下转型就是把父类对象转为子类对象
* **上转型的好处**  比如存在一个的`函数`的参数是一个父类，而这个父类被许多子类继承，那么正常情况下有多少子类就要重写多少个方法，如果用到上转型，就只需要写一个方法，每个子类只需要上转型就可以给这个`函数`传参，这就让代码很舒服

