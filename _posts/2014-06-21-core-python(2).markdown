---
layout: post
title:  "Core Python programming（2）"
category: Core Python programming
tags: python
---
4-5 chapter

###第四章 python对象

####4.1Python对象

所有的Python对象拥有三个特性:__身份,类型和值__

####4.2标准类型(基本数据类型)

* 数字
* Integer 整形
* Boolean 布尔型
* Long integer 长整型
* Floating point real number 浮点型
* Complex number 复数型
* String 字符串
* List 列表
* Tuple 元祖
* Dictionary 字典

####4.3其他内建类型

* 类型
* Null对象(None)
* 文件
* 集合/固定集合
* 函数/方法
* 模块
* 类 

通过__type()__函数你能得到特定对象的类型信息

####4.4内部类型

* 代码
* 帧
* 跟踪记录
* 切片
* 省略
* Xrange

####4.5标准类型操作符

1. 对象值的比较
2. 对象身份的比较
3. 布尔型


对象身份的比较:

* foo1和foo2指向相同的对象

这里foo1和foo2实际指向的都是在内存创建的唯一一个4.3,如果调用```foo1 is foo2```(等价于```id(foo1) == id(foo2)```) 则返回结果true

```python
foo1 = foo2 = 4.3

foo1 = 4.3
foo2 = foo1
```

 * foo1和foo2指向不同的对象

```python
foo1 = 4.3
foo2 = 1.3 + 3.0
```
虽然两个变量的值相等,但是两个对象指向的是不同的对象.调用```foo1 is foo2```结果返回false

整形对象和字符串对象是不可变对象,所以python会很高效的缓存他们.这会造成我们认为python应该创建新对象时,它却没有创建对象的假象

####4.6标准类型内建函数

1. cmp(obj1,obj2) 比较
2. repr(obj)或'obj' 返回一个对象的字符串表示
3. str(obj) 返回对象适合可读性好的字符串表示
4. type() 得到一个对象的类型,并返回相应的type对象

repr()和str()的区别:

* str()对用户更友好,repr()对python更友好
* 当在python解释器中直接输入a时,调用的是repr()函数!而print语句调用的是str()函数
* repr()和``做的时完全一样的事情,他们返回一个对象的官方字符串表示,也就是说绝大多数情况下可以通过求值运算(使用内建函数eval()重新得到该对象),str()致力于生成一个对象的可读性更好的字符串表示,它返回的结果通常无法用于eval()求值.

```python
>>> a = 'haha'
>>> print a
haha
>>> a
'haha'

```

isinstance():检查一个对象是否是一个类的实例.(第13章详细研究).

这里知道函数type()的一个调用,就是```type(obj).__name__```,它返回obj的类型的字符串表示.

对于判断一个变量类型的方法还是用isinstance()比较好.

####4.7类型工厂函数

工厂函数:int(),type(),list()虽然他们看上去有点像函数,实质上他们是类.当你调用他们时,实际上是生成了该类的一个实例,就像工厂生产货物一样.

####4.8标准类型的分类

不怎么太实用,略去.

####4.9不支持的类型

Python中一切都是指针.

Python的整形实现等同于C语言的长整型.Python为用户解决了整型范围的烦恼,用户不用再担心两个大数相乘会溢出

Python的浮点型实际上时C语言的双精度浮点型.Python不支持单精度浮点型.(如果要处理十进制浮点型类型,必须decimal模块,如计算金钱等)


###第五章 数字

####5.1数字简介

注意删除数字对象的方法:

```python
del number
```

在这里要说明一下:del仅仅是删除一个引用,而不是删除对象本身,删除对象的操作还是留给了Python内存释放机制(引用计数机制),用户无法干预!!

####5.2整型

这里的整型概念很简单,python不区分整型和长整型的区别,对程序员来说方便了很多.

####5.5操作符

真正的除法:
python中默认的除法是这样滴

```python
>>> 5/2
2
```

但是如果引入division就变成了正常滴

```python
>>>form __future__ import division
>>> 5/2
2.5
```
####5.6内建函数与工厂函数

这里讲了一个coerce()函数,返回类型转换完毕的两个数值元素的元组

```python
>>>coerce(1,134L)
(1L,134L)
```

pow(x,y,z)第三个参数可选,用来将运算结果和第三个参数进行取余运算.并且比pow(x,y)%z性能好.

int(),floor(),round()区别:

* int()直接截去小数部分(返回值为整型)
* math.floor()得到最接近原数但小于原数的整数(返回浮点型)
* round()得到最接近原数的整型(返回值为浮点型)

####5.7其他数字类型

布尔:没有```__nonzero__()```方法的对象的默认值时True.(其实判断一个对象的bool值就是调用该对象的```__nonzero()```方法)






































