---
layout: post
title:  "Core Python programming（4）"
category: Core Python programming
tags: python
---

###第7章 映像和集合类型

集合目前还不怎么用,暂时先看字典,用时再回来看.

####7.1 映射类型:字典

字典类型和序列类型容器的区别是存储和访问数据的方式不同.序列类型只用数字类型的键.映射类型可以用其他对象类型做键,一般最常见的是用字符串做键.

字典采用hash存储,任何一个值存储的地址皆取决于它的键(key)

__疑问__:python的字典为什么每次输出都是无序的?

字典的基本形态:
```python
dic={key1:value1, key2:value2...}
```

1.字典的创建

 * 直接型。

```python
dict1={}  
dict2={‘name’:'earth','port':'80'}
```

 * 使用工厂方法dict，通过其他映射（例如字典）或者（键，值）这样的序列对建立

```python
items=[('name','earth'),('port','80')]
dict2=dict(items)
dict1=dict((['name','earth'],['port','80']))
```

 * 使用内建方法fromkeys()创建’默认‘字典，字典中元素具有相同的value（如果没有给出，默认为none）
 
```python
dict1={}.fromkeys(('x','y'),-1) # dict={'x':-1,'y':-1}
dict2={}.fromkeys(('x','y')) # dict2={'x':None, 'y':None}
```

2.访问字典中的值
最常用和基本的莫过于利用key访问value了

* 通过key访问value之get方法

```python
dict1.get('name')#也可以直接是dictionary['key1'],但是当key1不存在其中时，会报错 ；此时用get则返回None 
```

* 随机访问其中键值对
字典中是无序的，利用popitem方法是随机弹出一个键值对

* 返回字典所有值的列表

```python
dict.values()
```

3.访问字典中的key

* 检查是否含有key1

```python
dictionary.has_key(key1) 
key1 in dictionarty # 最好用这种方法
key1 not dictionary
```

* 返回字典中键的列表

```python
dictionary.keys()
```

4.访问键值对

* 遍历方式
 
```python
for r in dicitonary  #r是dictionary中的键值对
```

* 修改(更新)或添加

```python
dictionary[key1]=value1
```

5.删除

* 按key删除

```python
del dictionary[key1]
```

* 删除并返回

```python
dictionary.pop(key1)
```

* 删除所有项

```python
dictionary.clear()
del dictionary
```

6.排序

 ```python
 sorted(dic.iteritems(), key=lambda d:d[1], reverse=False)
 ```

 说明：对字典dic中的元素按照d[1](d[1]是value，d[0]是key，和d没关系，可以改为a什么的)进行升序排序，通过设置reverse的True或False可以进行逆序，并返回排序后的字典（该排序后的字典由元组组成，其形式为[(key1,value1),(key2,value2),...]，且原字典保持不变）

7.其他

```python
len(dictionary) #返回字典项个数
dict.update() # 将整个字典的内容添加到另一个字典
dictionary.items() # 返回字典中键值对的元组
```

>注意:字典中的键必须是可哈希的,所以数字和字符可以作为字典中的键,但是列表和其他字典不行


####7.3 字典的比较

字典的比较不太常用,不过说一下比较的顺序
字典比较的顺序:

 1. 比较字典长度
 2. 比较字典的键
 3. 比较字典的值
 4. Exact Match

到此为止,即,每个字典有相同的长度、相同的键、每个键也对应相同的值,则字典完全匹配,
返回 0 值。


####7.4 映射类型的内建方法



| 方法名字 |     操作 |
| :--- | :--- |
| dict.clear()  | 删除字典中所有元素|
| dict.copy()   | 返回字典(浅复制)的一个副本 |
| dict.fromkeysc(seq,val=None)  | 创建并返回一个新字典，以seq 中的元素做该字典的键，val 做该字典中所有键对应的初始值(如果不提供此值，则默认为None) |
| dict.get(key,default=None)    | 对字典dict 中的键key,返回它对应的值value，如果字典中不存在此键，则返回default 的值(注意，参数default 的默认值为None) |
| dict.has_key(key) | 如果键(key)在字典中存在，返回True，否则返回False. 在Python2.2版本引入in 和not in 后，此方法几乎已废弃不用了，但仍提供一个 可工作的接口。 |
| dict.items()  | 返回一个包含字典中(键, 值)对元组的列表 |
| dict.keys()   | 返回一个包含字典中键的列表 |
| dict.values() | 返回一个包含字典中所有值的列表 |
| dict.iter()   | 方法iteritems(), iterkeys(), itervalues()与它们对应的非迭代方法一样，不同的是它们返回一个迭代子，而不是一个列表。 |
| dict.pop(key[, default])  | 和方法get()相似，如果字典中key 键存在，删除并返回dict[key]，如果key 键不存在，且没有给出default 的值，引发KeyError 异常。 |
| dict.setdefault(key,default=None) | 和方法set()相似，如果字典中不存在key 键，由dict[key]=default 为它赋值。 |


####7.5 字典的键

1. 不允许一个键对应多个值
2. 键必须是可哈希的(值相等的数字表示相同的键,即整数1和浮点数1.0是同一个键),同时,也有一些(很少)可变对象是可哈希的(一个实现了```__hash()__```特殊方法的类.因为它返回的是一个整数值,它是固定不变的)


###第8章 条件和循环

这章比较简单,有编程基础的都能轻松应对,所以没有写的太细,这里需要更好的把握python语言的特点.
python没有switch语句!

####8.1 if语句

python语言有一个很大的优点就是用缩进替代了括号,省空间,美观,避免了"悬挂else"问题.

刚开始学python的时候大多数人可能和我一样,忘记写if和else后面的"__:__"号了

```python
if expression:
    expr_true_suite
else:
    expr_false_suite
```

####8.4 三元操作符

在C中这么用:

```
(C ? X : Y)
```

在python中这么用:

```python
x if x < y else y
```

####8.5 while语句

```python
while expression:
    suite_to_repeat
```

####8.6 for语句

for循环可以遍历序列成员,可以用在列表解析和生成器表达式中,它会自动地调用迭代器的next()方法,捕获StopIteration异常并结束循环.

>我们在说要遍历一个迭代器时,实际上可能我们指的是要遍历一个序列,迭代器或者是一个支持迭代的对象(有next()方法)

range()内建函数:range()实际上先用我们指定的条件生成一个列表,然后把这个列表用于这个for语句.

```python
range(start,end,step = 1) #完整语法
range(end) # 简略语法
range(start,end) # 简略语法
```

xrange()内建函数:当你创建一个较大范围的列表时,xrange()不会在内存里创建完整的列表拷贝,性能更优.

与序列相关的内建函数:

* sorted() : 排序
* reversed() : 翻转
* enumerate() : 带index的列表
* zip() : 打包(在diango用过,可以把两个列表生成一个列表,其中每一项都是两个列表相同位置的值,比如[1,2,3,4,5]和[6,7,8,9,0]就会生成[(1, 6), (2, 7), (3, 8), (4, 9), (5, 0)])

####8.9 pass语句

>作为开发中的小技巧,用pass可以标记你后来要完成的代码


####8.11 迭代器和iter()函数

迭代器为类序列对象提供了一个类序列的的接口.Python的迭代无缝地支持序列对象,而且它还允许程序员迭代非序列类型,包括用户定义的对象.
__从根本上说,迭代器就是一个next()方法的对象,而不是通过索引来计数!__

迭代器的限制:你不能向后移动,不能回到开始,也不能复制一个迭代器.如果再次迭代,只能去创建另一个迭代对象.

如何创建迭代器(13章详解):

```python
iter(obj)
iter(func,sentinel)
```

####8.12 列表解析

列表解析:在一个序列的值上应用一个任意表达式，将其结果收集到一个新的列表中并返回。它的基本形式是一个方括号里面包含一个for语句对一个iterable对象迭代.

```python
>>> res=[ord(x) for x in 'spam']
>>> res
[115, 112, 97, 109]
>>> [x**2 for x in range(10)]
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

这里还需要了解的是map(),filter(),lambda()这三个函数，他们是Python早期的函数式编程，但是都可以被列表解析替代．

1.map():根据提供的函数对指定序列做映射。
map函数的定义：

```python
map(function, sequence[, sequence, ...]) -> list
```

* 通过定义可以看到，这个函数的第一个参数是一个函数，剩下的参数是一个或多个序列，返回值是一个集合。

* function可以理解为是一个一对一或多对一函数，map的作用是以参数序列中的每一个元素调用function函数，返回包含每次function函数返回值的list。

比如要对一个序列中的每个元素进行平方运算：

```python
>>>map(lambda x: x ** 2, [1, 2, 3, 4, 5])

[1, 4, 9, 16, 25] #返回结果
```


在参数存在多个序列时，会依次以每个序列中相同位置的元素做参数调用function函数。

比如要对两个序列中的元素依次求和。

```python
map(lambda x, y: x + y, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10])
```

map返回的list中第一个元素为，参数序列1的第一个元素加参数序列2中的第一个元素(1 + 2)，list中的第二个元素为，参数序列1中的第二个元素加参数序列2中的第二个元素(3 + 4)，依次类推，最后的返回结果为：

```python
[3, 7, 11, 15, 19]
```

要注意function函数的参数数量，要和map中提供的集合数量相匹配。

如果集合长度不相等，会以最小长度对所有集合进行截取。

当函数为None时，操作和zip相似：

```python
>>>map(None, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10])

[(1, 2), (3, 4), (5, 6), (7, 8), (9, 10)] #返回结果
```







2.filter():filter函数会对指定序列执行过滤操作。

filter函数的定义：
```python
filter(function or None, sequence) -> list, tuple, or string
```

* function是一个谓词函数，接受一个参数，返回布尔值True或False。

* filter函数会对序列参数sequence中的每个元素调用function函数，最后返回的结果包含调用结果为True的元素。

* 返回值的类型和参数sequence的类型相同
 比如返回序列中的所有偶数：

```python
def is_even(x):
       return x & 1 != 0

filter(is_even, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
 
[1, 3, 5, 7, 9] #返回结果
```

如果function参数为None，返回结果和sequence参数相同。

3.reduce():对参数序列中元素进行累积。

reduce函数的定义：

```python
reduce(function, sequence[, initial]) -> value
```

* function参数是一个有两个参数的函数，reduce依次从sequence中取一个元素，和上一次调用function的结果做参数再次调用function。

* 第一次调用function时，如果提供initial参数，会以sequence中的第一个元素和initial作为参数调用function，否则会以序列sequence中的前两个元素做参数调用function。

```python
reduce(lambda x, y: x + y, [2, 3, 4, 5, 6], 1)
结果为21 #(  (((((1+2)+3)+4)+5)+6)  )
reduce(lambda x, y: x + y, [2, 3, 4, 5, 6])
结果为20
```

注意function函数不能为None。

4.lambda():lambda函数也叫匿名函数，即，函数没有具体的名称

比如正常情况下：

```python
def f(x):
      return x**2
print f(4)
```

用lambda():

```python
g = lambda x : x**2
print g(4)
```

那么，lambda表达式有什么用处呢？很多人提出了质疑，lambda和普通的函数相比，就是省去了函数名称而已，同时这样的匿名函数，又不能共享在别的地方调用。其实说的没错，lambda在Python这种动态的语言中确实没有起到什么惊天动地的作用，因为有很多别的方法能够代替lambda。同时，使用lambda的写法有时显得并没有那么pythonic。甚至有人提出之后的Python版本要取消lambda。

回过头来想想，Python中的lambda真的没有用武之地吗？其实不是的，至少我能想到的点，主要有：

* 使用Python写一些执行脚本时，使用lambda可以省去定义函数的过程，让代码更加精简。

* 对于一些抽象的，不会别的地方再复用的函数，有时候给函数起个名字也是个难题，使用lambda不需要考虑命名的问题。

* 使用lambda在某些时候让代码更容易理解。

列表解析可以取代以上四个函数，而且效率更高

列表解析扩展版本语法：

```python
 [expr for iter_var in iterable if cond_expr]
```

这个语法迭代时会过滤或＂捕获＂满足条件表达式cond_expr的序列成员．

####8.13 生成器表达式

生成器表达式只要记住一点：它和列表解析的功能一样，但是实现的方法不一样，它允许你返回一个值，然后暂停代码的执行，稍后恢复．意思就是它可以更优化的利用内存，而不是一次全部生成出来，用到的时候自然的进行运算．和文件操作中的readline()比较相像．

列表解析：

```python
[expr for iter_var in iterable if cond_expr]
```

生成器表达式：

```python
(expr for iter_var in iterable if cond_expr)
```

这里两个仅是方括号之差！

