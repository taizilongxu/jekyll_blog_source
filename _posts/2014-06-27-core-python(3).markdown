---
layout: post
title:  "Core Python programming（3）"
category: Core Python programming
tags: python
---
6 chapter
###第六章 序列:字符串,列表和元组

为什么把字符串,列表和元组叫做序列呢,因为这些类型其实都是由一些成员共同组成的一个序列整体,并且可以通过下标偏移量访问到它的一个或者几个成员.序列是Python很重要的数据结构,方便又快捷,干活基本都靠他们了.

####6.1序列

标准类型操作符:详见4.5

序列类型操作符:

1. 成员关系操作符(in,not in)
2. 连接操作符(+)
3. 重复操作符(*)
4. 切片操作符([],[:],[::])

内建函数:

1. 类型转换(工厂函数):list(iter),str(),unicode(),basestring()(str和unicode的父类,不能实例化也不能调用),tuple(iter)
2. 可操作:

| 函数名 | 功能 |
| :--- | :---- |
| enumerate(iter) | 接受一个可迭代对象,返回一个enumerate对象(由iter每个元素的index值和item值组成的元祖) |
| len(seq) | 返回seq的长度 |
| max(iter,key=None) or max(arg0,arg1...key=None) | 返回iter里面的最小值或者返回(arg0,arg1...)里面的最小值;如果指定了key,这个key必须时一个可以传给sort()方法的,用于比较的回调函数 |
| reversed(seq) | 接受一个序列作为参数,返回一个以逆序访问的迭代器 |
| sort(iter,func=None,key=None,reverse=False) | 接受一个可迭代对象作为参数,返回一个有序列表;可选参数func,key和reverse的含义跟list.sor()内建函数的参数含义一样. |
| sum(swq,init=0) | 返回seq和参数init的总和 |
| zip([it0,it1,it2...itN]) | 返回一个列表,其第一个元素是it0,it1...这些元素第一个元素组成的一个元祖,第二个...以此类推 |


####6.2字符串

Python实际上有三类字符串.通常意义的字符串(str)和Unicode字符串(unicode)实际上都是抽象类basestring的子类.这个basestring()不能被调用也不能实例化.

1. 字符串的创建和赋值
2. 访问字符串的值(切片)
3. 如何改变字符串:字符串跟数字类型一样,是不可变类型,要改变一个字符串必须通过创建一个新串的方式来实现.
4. 删除字符和字符串(del):没有必要显示的删除字符串!

####6.4只适用于字符串的操作符

格式化操作符(%):非常类似C语言里面的printf()函数的字符串格式化,并且支持所有printf()式的格式化操作.格式字符串既可以跟print语句一起用来向终端用户输出数据,又可以用来合并字符串形成新字符串,而且还可以直接显示到GUI界面上去.

原始字符串操作符(r/R):在原始字符串里,所有的字符都是直接按照字面的意思来使用,没有转义特殊或不能打印的字符.

Unicode字符串操作符(u/U):把标准字符串或者是包含Unicode字符串转换成完全的Unicode字符串对象.

字符串类型函数:

1. raw_input():输入函数
2. str()和unicode():工厂函数
3. chr(),unichr(),ord():

* chr():用一个范围在range(256)内的整数做参数,返回一个对应的字符.
* unichr():与chr()一样,但返回的是unicode字符
* ord():它以一个字符作为参数,返回对应的ASCII数值,或者Unicode数值.

####6.8Unicode
一直对Unicode迷惑不解,想不明白为什么Python的Unicode编码写入的时候既要编码,又要解码,直到我看了这个文章~~[字符编码笔记：ASCII，Unicode和UTF-8][1]

这里需要注意的是区分Unicode和UTF-8之间的区别:

>Unicode只是一个符号集,它只规定了符号的二进制代码.而UTF-8则是Unicode的一种编码实现,它规定了Unicode编码以怎样的二进制编码进行存储.

下面看例子来区分Unicode和UTF-8:

UTF-8的编码规则很简单，只有二条：

1. 对于单字节的符号，字节的第一位设为0，后面7位为这个符号的unicode码。因此对于英语字母，UTF-8编码和ASCII码是相同的。
2. 对于n字节的符号（n>1），第一个字节的前n位都设为1，第n+1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个符号的unicode码。

| Unicode符号范围 (十六进制) |  UTF-8编码方式（二进制）|
| :---- | :----  |
| 0000 0000-0000 007F | 0xxxxxxx(可以装7位) |
| 0000 0080-0000 07FF | 110xxxxx 10xxxxxx(11位) |
| 0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx(17位) |
| 0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx(23位) |

上表总结了UTF-8编码规则，字母x表示可用编码的位,也就是说Unicode编码可以填充进x中.

比如:

```python
100111000100101 #"严"的unicode编码

11100100 10111000 10100101 #"严"的UTF-8编码
```

已知"严"的unicode是4E25（100111000100101），根据上面的表，可以发现4E25处在第三行的范围内（0000 0800-0000 FFFF），因此"严"的UTF-8编码需要三个字节，即格式是"1110xxxx 10xxxxxx 10xxxxxx"。然后，从"严"的最后一个二进制位开始，依次从后向前填入格式中的x，多出的位补0。这样就得到了，"严"的UTF-8编码是"11100100 10111000 10100101"，转换成十六进制就是E4B8A5。


####6.14列表类型的内建函数

我们可以在一个列表对象上应用```dir()```方法来得到它所有的方法和属性.

列表类型内建方法:

| 列表函数  |  作用 |
| :---- | :---- |
| list.append(obj) |  列表添加obj对象 |
| list.count(0bj) | 返回obj在列表中出现次数 |
| list.extend(seq) | 把序列seq的内容添加到列表中 |
| list.index(obj,i=0,j=len(list)) | 返回list[k]==obj的k值,并且k的范围在```i<=k<j```;否则引发ValueError异常 |
| list.insert(index,obj) | 在索引量为index的位置插入对象obj |
| list.pop(index==-1) | 删除并返回制定位置的对象,默认是最后一个对象 |
| list.remove(obj) | 删除obj对象 |
| list.reverse() | 原地翻转列表 |
| list.sort(func=None,key=None,reverse=False) | 以制定方式排序列表中的成员,如果func和key参数制定,则按照指定方式比较各个元素,如果reverse标志被置为True,则列表以反序排列 |

这里总结一下sorted和sort的用法:

sorted:
```python
sorted(data, cmp=None, key=None, reverse=False)  
```

list.sort(这里的默认排序方法是归并排序,时间复杂度为O(lg(n!))):

```python
list.sort(cmp=None,key=None,reverse=False)  
```

* cmp(e1, e2) 是带两个参数的比较函数, 返回值: 负数: e1 < e2, 0: e1 == e2, 正数: e1 > e2. 默认为 None, 即用内建的比较函数. 
* key 是带一个参数的函数, 用来为每个元素提取比较值. 默认为 None, 即直接比较每个元素. 
* 通常, key 和 reverse 比 cmp 快很多, 因为对每个元素它们只处理一次; 而 cmp 会处理多次. 

```python
>>> students = [('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10),] 

>>> sorted(students,key=lambda student:student[2]) # sort by age 
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]

>>> sorted(students, cmp=lambda x,y : cmp(x[2], y[2])) # sort by age  
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]  

```
用 operator 函数来加快速度, 上面排序等价于:

```python
>>> from operator import itemgetter, attrgetter  
>>> sorted(students, key=itemgetter(2))  
```

用 operator 函数进行多级排序:

```python
>>> sorted(students, key=itemgetter(1,2))  # sort by grade then by age  
[('john', 'A', 15), ('dave', 'B', 10), ('jane', 'B', 12)]  
```


对由字典排序 :

```python
>>> d = {'data1':3, 'data2':1, 'data3':2, 'data4':4}  
>>> sorted(d.iteritems(), key=itemgetter(1), reverse=True)  
[('data4', 4), ('data1', 3), ('data3', 2), ('data2', 1)]  
```

__区别__:sorted排序过后返回一个新的列表,而list.sort返回的是原列表并排好序.

####6.15列表的特殊特性

这里列表可以构建堆栈和队列:仅使用append()和pop()方法即可

####6.16元组

注意:只有一个元组需要在元组分割符里面加一个逗号(,),以防止跟普通的分组操作符混淆.

####6.18元组的特殊性

python的3个标准不可变类型:数字,字符串和元组字符串.

元组并不是'不可变':

* 把元组连接成一个大元组:

```python
>>> t=('third','fourth')
>>> t
('third', 'fourth')
>>> t = t + ('fifth','sixth')
>>> t
('third', 'fourth', 'fifth', 'sixth')

```

* 元组中包含可变的对象:

```python
>>>t = (['xyz',123],23,456)
>>>t[0][0] = 'abc'
>>>t
 (['abc',123],23,456)
```

所有的多对象的,逗号分割的,没有明确用符号定义的,这些集合默认的类型都是元组.

```python
>>> 123,'23',345
(123, '23', 345)
```

所有函数返回的多对象都是元组类型.注意,有符号封装的多对象集合其实是返回的一个单一的容器对象.

```python
def foo1():
    :
    return obj1,obj2,obj3
```

####6.20浅拷贝和深拷贝

序列类型的对象的浅拷贝是默认拷贝类型,有以下几种方法:

1. 完全切片[:]
2. 利用工厂函数,如list(),dict()等
3. 使用copy模块的copy函数

如果使用深拷贝则要用copy.deepcopy()函数.

>注意:
1. 非容器类型(数字,字符串)没有被拷贝一说,浅拷贝是用完全切片操作来完成的.
2. 如果元组变量只包含原子类型对象,对它的深拷贝将不会进行.







