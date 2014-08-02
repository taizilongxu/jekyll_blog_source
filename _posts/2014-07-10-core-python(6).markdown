---
layout: post
title:  "Core Python programming（6）"
category: Core Python programming
tags: python
---

###第十一章 函数和函数式编程

####11.1 什么是函数?

>函数是对程序逻辑进行结构或过程化的一种编程方法.

我的理解就是函数完成了一种封装,其实过程化编程完全没有函数也没有问题,但是你必须付出相当大的代码量~~而Python的过程就是函数,因为解释器会隐式的返回默认值None.

python中的返回值:

|返回的对象数目|Python实际返回对象|
|:---|:---|
|0|None|
|1|object|
|>1|tuple|

Python并不像许多静态语言主张一个函数的类型就是其返回值的类型.在Python中,由于Python是动态确定类型而且函数能返回不同类型的值,所以没有进行直接的类型关联.

####11.2 调用函数

#####关键字参数:

```python
def foo(host,port):
	foo_suite
```

在调用的时候直接可以用

```python
foo('kappa',8080)
```

也可以不按顺序,但是要输入对应的参数名

```python
foo(port = 8080,host = 'chino')
```

####11.3 创建函数

def语句用来创建函数

```python
def function_name(arguments):
	"funciton_documentation_string"
    function_body_suite
```
python不允许在函数未声明之前,对其进行引用和调用.

函数属性:

* 函数声明后第一个没有赋值的字串就是这个函数的帮助文档,可以用```help(function)```或```者function.__doc__```调用
* 函数的属性可以在函数的外部定义,如下例

```python
def foo():
	pass
foo.version = 0.1
foo.__doc__ = 'doc'
```

#####函数装饰器

[Python装饰器与面向切面编程](http://www.cnblogs.com/huxi/archive/2011/03/01/1967600.html),很好的文章,对装饰器介绍的很通俗易懂.

装饰器的用途:

* 引入日志
* 增加记时逻辑来检测性能
* 给函数加入事务的能力

####11.4 传递函数

可以用其他的变量来作为函数的别名,因为所有的对象都是通过引用来传递的,函数也不例外.
例如:

```python
def foo():
	print 'in foo()'
```

foo是函数对象的引用,而foo()是函数对象的调用
例如:

```python
def convert(func,seq):
	return [func(eachNum) for eachNum in seq]
myseq = (123,45.67,-6.2e8,99999999L)

print convert(int,myseq)
print convert(long,myseq)
print convert(float,myseq)
```

####11.5 Formal Arguments

位置参数:必须以在被调用函数中准确顺序来传递.

默认参数:如果在函数调用时没有为参数提供值则使用预先定义的默认值.
Python中默认值声明必须在位置参数之后

```python
def func(posargs,defarg1 = dval1,defarg2 = dval2,...):
	function_body_suite
```

每个默认参数都紧跟着一个默认值语句,如果在函数调用时没有给出值,那么这个赋值就会实现.

####11.6 可变长度的参数

1.非关键字可变长参数(元组)

```python
def tupleVarArgs(arg1,arg2 = 'defultB',*theRest):
```

2.关键字变量参数(字典)
字典中键为参数名,值为对应的参数值.

关键字参数应为函数下定义的最后一个参数

```python
def tupleVarArgs(arg1,arg2 = 'defultB',*theRest,**vargsd):
```

####11.7 函数式编程

[知乎里的解答](http://www.zhihu.com/question/22039726)

自我理解函数式编程最主要的特点就是去掉了中间值,用函数来表示,最重要的就是lambda函数了.python的内置函数式编程方法,上一篇已经介绍了,就不多说了,不过书上的图片还是解释的相当明白的.

####11.8 变量作用域

全局变量的一个特征是除非被删除掉,否则他们的存货到脚本运行结束,且对于所有的函数,他们的值都是可以被访问的;然而局部变量,及iuxiangtamen存放的栈,暂时的存在,仅仅是只依赖于定义他们的函数现阶段是否处于活动.

>Python搜索变量的过程:先从局部作用域开始搜索,如果在局部作用域内没有找到变量名字,那么就一定会在全局找到这个变量,否则的话就会跑出NameError异常.也就是意味着局部变量可以'隐藏'一个全局变量.

global语句:为了明确的引用一个已命名的全局变量,必须使用global语句.

```python
is_this_global = 'xyz'
def foo():
	global is_this_global
```

#####闭包

>若果在一个内部函数里,对在外部作用域的变量进行引用,那么内部函数就被认为是闭包的.定义在外部函数内的但由内部函数引用或者使用的变量被称为自由变量.闭包的词法变量不属于全局名称空间域或者局部的,而属于其他的名称空间.

通俗解释:子函数可以使用福函数中的局部变量


####11.10 生成器

>可以暂停或者挂起,并从程序离开的地方继续或者重新开始,从语法上讲,生成器是一个带yield语句的函数.一个函数或者子程序只返回一次,但一个生成器能暂停执行并返回一个中间变量.

和迭代器最大的差别就是能够暂停并返回,而不用事先完全迭代完毕,有利于速度的提升

#####番外篇:偏函数
以下引自http://www.pythoner.com/54.html

偏函数是将所要承载的函数作为partial()函数的第一个参数，原函数的各个参数依次作为partial()函数后续的参数，除非使用关键字参数。

通过语言描述可能无法理解偏函数是怎么使用的，那么就举一个常见的例子来说明。在这个例子里，我们实现了一个取余函数，对于整数100，取得对于不同数m的100%m的余数。

```python
from functools import partial
 
def mod( n, m ):
  return n % m
 
mod_by_100 = partial( mod, 100 )
 
print mod( 100, 7 )  # 2
print mod_by_100( 7 )  # 2
 
```

由于之前看到的例子一般选择加法或乘法来讲解，无法体会偏函数参数的位置问题，容易给人造成partial的第二个参数也是原函数的第二个参数的假象，所以我在这里选择mod来讲解。

而对于有关键字参数的情况下，就可以不按照原函数的参数位置和个数了。下面再看一个例子，讲的是如何进行不同的进制转换。

```python
from functools import partial
 
bin2dec = partial( int, base=2 )
print bin2dec( '0b10001' )  # 17
print bin2dec( '10001' )  # 17
 
hex2dec = partial( int, base=16 )
print hex2dec( '0x67' )  # 103
print hex2dec( '67' )  # 103
```

偏函数的这些应用看似简单，用途却很大，可以很好的执行DIY原则，节省编程成本。

