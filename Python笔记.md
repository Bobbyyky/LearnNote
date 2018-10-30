# Python笔记

[TOC]

+++

### 函数参数

#### 必选参数

* 根据`位置顺序`传参，无法更改顺序

#### 默认参数

* 这个参数有默认值，变化小的参数可以设为默认参数，这样会降低调用难度
* 需要在位置参数后面
* 一定要是不变的对象，不然在重复调用的时候就会导致默认参数的值发生改变

#### 可变参数

* 个数可变，是不确定的，所以可以是`list`或`tuple`
* 为了使传参时可以简便，在定义可变参数时这样定义`* + 参数名`，在传参时直接传入值就可以了
* 如果要传入`list`或`tuple`,可以在传参时这样`* + list名 或 tuple名`

#### 关键字参数

* 任意个含参数名的参数，在函数内部自动组装为一个`dict`，有很好的扩展性，扩展函数的功能
* 这样定义`** +  参数名`
* 如果要传入`dict`，可以在传参时这样`** + dict名`

#### 命名关键字参数

* 关键字参数的传入不受到限制，如需要限制就要用到`命名关键字参数`
* 定义时这样`（*  , 命名关键字参数名, 命名关键字参数名,...）`
* 在`*`后的视为命名关键字参数，如果在可变参数后定义命名关键字参数，就不需要`*`了

#### 参数组合

* 参数定义顺序一定要是必选参数，默认参数，可变参数，关键字参数，命名关键字参数

---

### 生成器（迭代器）

#### 生成器（迭代器）

* 是可以边循环边计算生成的一种机制，是一直更新的，同一时间内内存只有一个值，但是只能往后取值，是一次性的，无法统计长度
* 定义方法一：`list`定义时使用`[ ]`，而生成器`generator`可以使用`( )`定义
* 定义方法二：自己定义一个函数，将`print`换为`yield`即可，只要函数中包含`yield`那么这个函数就是`generator`
* 调用方法一：使用`next( )`直接调用（不方便）
* 调用方法二：使用`for循环`

---

### Iterable和 Iterator

* 可以直接作用于`for`循环的对象称为`可迭代对象  Iterable`
* 而可以被`next( )`调用并不断返回下一个值的对象称为`迭代器  Iterator` 
* `Iterator`对象表示的是一个数据流 ，可以把这个数据流看做是一个有序序列 ，我们不能提前知道序列长度，只能不断通过`next()`函数按需计算下一个数据 ，所以`Iterator`的计算是惰性的 ，只有在需要下一个值时才会计算
* 把`Iterable`变成`Iterator`可以使用`iter()`函数 ，`(iter( 需要转换的Iterabl ), Iterator)`

---

### 函数

#### map函数和reduce函数

[官方介绍及论文](https://ai.google/research/pubs/pub62)

[一篇CSDN博客](https://blog.csdn.net/oppo62258801/article/details/72884633)

* `map()`函数接收两个参数，第一个是函数，第二个是`Iterable`，`map`将传入的函数依次作用到序列的每个元素，并把结果作为新的`Iterator`返回 
* `reduce`把一个函数作用在一个序列上，这个函数必须接收两个参数，`reduce`把结果继续和序列的下一个元素做累积计算 ，`reduce`函数接受两个参数，第一个是函数，第二个是序列

#### filter函数

* `filter()`接收一个函数和一个序列。和`map()`不同的是，`filter()`把传入的函数依次作用于每个元素，然后根据返回值是`True`还是`False`决定保留还是丢弃该元素 

#### sorted函数

* `sorted`有三个参数，第一个`iterable`返回一个新的`sorted list`，后两个为命名关键字参数；第二个`key`接受一个函数，来实现自定义排序；第三个`reverse`是一个布尔值，默认值是`False`及默认顺序排列，若是`True`则反序排列

#### 返回函数的函数

* 类似与`java`的打包，将功能打包，在需要的时候在调用，会降低计算压力
* 在打包时不要引用循环变量，因为循环变量不是立即执行的，它在函数返回时才会执行，所以会导致无法实现想要的功能
* 如果要引用循环变量，可以在其中再创建一个函数，用这个函数的参数来绑定这个循环变量的值。使得无论循环变量的值怎么变化，但循环变量当前的值已经得到了执行，所以函数返回时已经将循环变量遍历了一遍

#### 匿名函数

* 不需要定义函数名，不需要`return`，表达式的结果就是要返回值，可以将匿名函数赋值给变量
* 使用`lambda`定义，这样`lambda 参数: 表达式`

#### 装饰器(decorator)

* 可以在代码运行期间动态的增加功能，不会修改函数的定义，本质上，decorator就是一个返回函数的高阶函数 
* 使用装饰器会导致返回的函数的函数名发生改变，需要把原始函数的属性赋值给返回的函数，使用`functools.wraps `来完成，否则会导致一些依赖函数签名的代码执行时出错
* 使用@语法，把decorator置于函数的定义处 ，进行调用
* 一个完整的decorator的写法如下 

```
import functools

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```



* 带参数的decorator 

```
import functools

def log(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator
```

#### 偏函数

* 在python中有一些函数可以实现很多复杂的功能，这时需要对一些参数进行赋值。但是有时我们只是在重复的调用某一类功能，这样我们总是在对某一参数重复传递同一值。这时我们可以使用偏函数来固定某一个参数，来降低我们调用这个函数的复杂程度
* 使用`functools.partial `来创建偏函数

例如，其中`int2`是新定义的函数名，`int`是原函数，`base`是要固定的参数

``` 
import functools
int2 = functools.partial(int, base=2)
```

+++

### 类和对象

#### 属性的访问限制

* 在python中类和对象的属性是默认公开的，即可以自由的调用、修改
* 可以使用在属性名称前加两个下划线`__`，使得该属性成为私有变量，这样属性就无法随意访问了，但是也有方法可以访问，这是因为Python解释器对外把`__name`变量改成了`_XXXXXX__name` ，所以，仍然可以通过`_XXXXXX__name`来访问`__name`变量 ，不同版本的解释器的XXXXXX不同，这个方法可以限制访问
* 还可以使用特殊的`__slots__`变量 ，我们把需要限制的属性名称赋值给`__slots__`变量就可以了，这个方法可以限制修改

Student是类名，name、age是被允许修改的属性

```
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```

* `__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用的 ，除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__` 

#### 属性的范围限制

+ 一些属性是有范围限制的，但是直接定义之后，没有办法强制限制
+ 可以通过写方法来进行强制限制，但是调用时就变得复杂
+ 可以使用`@property`装饰器把一个方法变成属性调用，`@property`的实现比较复杂，把一个getter方法变成属性，只需要在方法前加上`@property`就可以了，这样`@property`本身又创建了另一个装饰器`@score.setter`，负责把一个setter方法变成属性赋值 

```
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```

#### Mixln模式

+ 一个类可以通过多重继承来获得多个功能，在一个类继承一个类时同时继承其他类来扩展功能，这种模式通常称为Mixln
+ MixIn的目的就是给一个类增加多个功能，这样，在设计类的时候，我们优先考虑通过多重继承来组合多个MixIn的功能，而不是设计多层次的复杂庞大的继承关系。
+ 一般情况下在是额外继承功能的类名之后` + Mixln`使得可以更好的看出继承关系 

#### 定制类

[廖雪峰的举例](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319098638265527beb24f7840aa97de564ccc7f20f6000)

+ Python官方提供一系列类的特殊方法，来帮助我们来定制类
+ 这一系列的类的特殊方法都是`__XXX__`这样的形式,使得我们可以实现许多特殊用途的功能

#### 枚举类

+ -用枚举类定义一个class，将一组常量放在这个class中，每个常量都是这个class的唯一实例，且这个class不可变，而且成员可以直接比较 
+ 用`Enum`类实现枚举类的功能，`@unique`装饰器可以帮助我们检查保证没有重复值 

+++

### 错误、调试、测试

