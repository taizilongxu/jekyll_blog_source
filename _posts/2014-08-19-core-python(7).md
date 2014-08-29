---
layout: post
title:  "Core Python programming（7）"
category: Core Python programming
tags: python
---

###第12章 模块

####12.1 什么是模块

>模块是Pyhon最高级别的程序组织单元，它将程序代码和数据封装起来以便重用。实际的角度，模块往往对应Python程序文件。每个文件都是一个模块，并且模块导入其他模块之后就可以使用导入模块定义的变量名。

####12.2 模块和文件

一个文件被看做是一个独立模块,一个模块也可以被看做是一个文件.模块的文件名就是模块的名字加上扩展名.py.在Python中,你导入的是模块或模块属性.

#####模块名称空间

每个模块都有一个自己的命名空间,这是为了防止名称冲突的发生.如果想调用模块里的函数,如下所示:

```python
import string

string.atoi()
```

#####搜索路径和路径搜索

模块路径搜索主要在文件系统的'预定义区域'中查找模块,这些'预定义区域'不过是你的Python搜索路径的合集:

1. 程序的主目录
2. PTYHONPATH目录（如果已经进行了设置）
3. 标准连接库目录（一般在/usr/local/lib/python2.X/）
4. 任何的.pth文件的内容（如果存在的话）.新功能，允许用户把有效果的目录添加到模块搜索路径中去

这四个组建成Python的搜索路径,查看搜索路径如下:

```python
import sys
print sys.path

```

如果想要啊增加搜索路径,那么:

```python
sys.path.append('/home/limbo/test/')
```

想要删除,那么就对列表进行删除操作就可以了.

####12.3 名称空间

按照Python解析器加载顺序,总共有3个名称空间:

1. 内建名称空间:由```__builtins__```模块中的名字构成.
2. 全局名称空间:加载的执行模块的全局名称空间
3. 内建名称空间:在执行期间调用了某个函数,那么将创建这个内建名称空间,很好理解,函数局部变量的作用域就在这个空间里.

解释一下,这个内建就是指在程序或者解析器运行阶段自动进行import的模块,不用手动调用,比如说我们直接调用的list,dict都不用我们import,而全局名称空间就是我们要手动import的了.

可以用如下方法查看内建空间的加载模块:

```python
dir(__builtin__) #足足100多
```


下面这篇文章讲述了```__builtins__``` 和 ```__builtin__``` 的区别,非常详细:
[Python中的内建模块](http://www.52ij.com/jishu/665.html)




#####名称查找

对于名称的查找是按着如下的顺序:

局部->全局->内建->NameError异常

也就是说函数里的变量名会优先在局部名称空间里查找,这也就导致了局部名称空间会**覆盖**同名的全局和内建名称空间变量.

#####无限制的名称空间

Python和Java不一样,那就是无限制名称空间,简单来说,就是你可以在函数外给函数添加属性(在Java里这绝对是开玩笑的),比如说:

```python
def foo():
	pass
doo.__doc__ = 'haha'
foo.version = 0.2

mymodule.foo()
mymodule.version
```

####12.4 导入模块

导入模块有以下三种方法:

1. import: 使客户端（导入者）以一个整体获取一个模块。
2. from ... import ...:容许客户端从一个模块文件中获取特定的变量名。
3. import ... as ...或form ... import ... as ...:使用自己的名字替换模块原始名称

#####import如何工作

import执行步骤:

1. 找到模块文件
2. 编译成位码（需要时）
3. 执行模块的代码来创建其所定义的对象。

在之后导入相同的模块时候，会跳过这三个步骤，而只提取内存中已加载模块对象。

#####import和from import的区别

许多初学者(譬如我~)分不清这两者的区别,起初只认为区别只是获取整个模块和模块中方法的区别,其实不然,这还涉及到名称空间的问题.

看以下列子:

```python
import foo
foo.method()
```

如果用from import:

```python
from foo import method
method()
```

看清了吗?如果用from import会使method加入到全局名称空间,可以直接调用method方法,但是这也容易使用起来发生混淆,所以书上说不太建议这样用,要用一般在如下场合用:

1. 目标模块中的属性非常多,反复键入起来不是很方便;
2. 交互解析器下,避免输入次数.

####12.5 模块导入特性

#####载入时执行模块

当一个模块被导入时,这个模块会被执行.也就是被导入模块的顶层代码将直接被执行.通常包括设定全局变量以及类和函数的声明.

所以当我们写程序的时候,尽量要把代码封装到函数中,只把函数和模块定义放入模块的顶层.

例如:

```python
def foo():
	'doc'
    print 'test'

def main():
	print 'test'

if __name__ == '__main__':
	main()
```

上例中,我们要把次程序编程日后需要使用的模块,所以要把程序要执行的部分写入main()函数中,当一个模块被调用的时候会检查```__name__```,这时因为模块被载入```__name__```会被赋予模块名,所以不会执行main()函数;相反,如果直接执行此模块,则```__name__```会变成```__main__```,就是我们要执行的部分了.

**还有一点需要特别注意,千万不要把自己文件的名字改成要载入模块的名字,这会发生一个nameerror导致模块不会被加载,已经中枪好几回了.**

#####关于```__future__```

```__future__``` 提供了下一个版本将要改进的新特性,但是我们不能这么使用:


```python
import __future__
```

必须要显示地导入指定特性.

#####globals()和locals()

分别返回调用者的全局和局部名称空间的字典.

```python
In [1]: locals().keys()
Out[1]: 
['_dh',
 '__',
 '_15',
 '__builtin__',
 '_16',
 '_iii',
 '_i15',
 'quit',
 '_i9',
 '_i8',
 '_i7',
 '_i6',
 '_i5',
 '_i4',
 '_i3',
 '_i2',
 '_i1',
 '__package__',
 '_i10',
 'exit',
 'get_ipython',
 '_i',
 '_i14',
 '__doc__',
 '_i16',
 '_18',
 '_ii',
 '__builtins__',
 '_10',
 'sys',
 '__name__',
 '___',
 '_',
 '_sh',
 'In',
 '_5',
 '_i13',
 '_i12',
 '_i11',
 '_14',
 '_i17',
 '_12',
 '_11',
 '_ih',
 '_17',
 '_i19',
 '_i18',
 '_oh',
 'Out']

```

#####reload()

reload()内建函数可以重新导入一个已经导入的模块.语法如下:

```python
reload(module)
```

注意:

1. 模块必须全部导入(不是使用form import)
2. 这个模块必须在之前被import

####12.7 包

包是一个由层次的文件目录结构,它定义了一个由模块和子包组成的Python应用程序环境.

它主要解决以下问题:

1. 允许程序员把有联系的模块组合在一起
2. 为平坦的名称空间加入有层次的组织结构
3. 允许分发者使用目录结构而不是一大堆混乱的文件
4. 帮助解决有冲突的模块名称

#####目录结构

```sh
# limbo at Pig in /usr/lib/python2.7/xml [20:11:44]
$ tree
.
├── dom
│   ├── domreg.py
│   ├── expatbuilder.py
│   ├── __init__.py
│   ├── __init__.pyc
│   ├── xmlbuilder.py
│   └── xmlbuilder.pyc
├── etree
│   ├── cElementTree.py
│   ├── cElementTree.pyc
│   ├── __init__.py
│   └── __init__.pyc
├── __init__.py
├── __init__.pyc
├── parsers
│   ├── expat.py
│   ├── expat.pyc
│   ├── __init__.py
│   └── __init__.pyc
└── sax
    ├── handler.py
    ├── handler.pyc
    ├── __init__.py
    ├── __init__.pyc
    ├── xmlreader.py
    └── xmlreader.pyc

```
只看例子,我们发现每个层级下都有一个```__init__.py```文件,这些是初始化模块,from import语句导入子包时需要用到它.如果没有用到,他们可以使空文件,但必须存在.

因为/usr/lib/python2.7/已经在我们的sys.path中,我们可以这样调用:

```python
import xml.dom.xmlbuilder
xml.dom.xmlbuilder.method()
```

还可以这样:

```python
from xml import dom
dom.xmlbuilder.method()
```

也可以这样:

```python
from xml.dom import xmlbuilder
xmlbuilder.method()
```

最后的最后你还可以这样:

```python
from xml.dom.xmlbuilder import method
method()
```

#####使用from-import导入包

from * 语句的行为：

作为一个高级功能，可以在```__init__.py```文件中使用```__all__```列表来定义目录以```form *```语句形式导入时，需要导出什么。```__all__```列表是指出当包（目录—）名称使用```from *```的时候，应该导入的子模块名称清单。

例如xml包中的```__init__.py```文件如下:

```python
__all__ = ["dom", "parsers", "sax", "etree"]
_MINIMUM_XMLPLUS_VERSION = (0, 8, 4)

try:     
    import _xmlplus
except ImportError:
    pass 
else:    
    try: 
    ¦   v = _xmlplus.version_info
    except AttributeError:
    ¦   # _xmlplus is too old; ignore it
    ¦   pass
    else:                                                                                                                                          
    ¦   if v >= _MINIMUM_XMLPLUS_VERSION:
    ¦   ¦   import sys
    ¦   ¦   _xmlplus.__path__.extend(__path__)
    ¦   sys.modules[__name__] = _xmlplus
    ¦   else:
    ¦   ¦   del v  

```

当```from xml import * ```时它会导入``` ["dom", "parsers", "sax", "etree"]```这几个子模块.

**绝对导入**:指明顶层 package 名。比如 import a，Python 会在 sys.path 里寻找所有名为 a 的顶层模块。
**相对导入**:在不指明 package 名的情况下导入自己这个 package 的模块，比如一个 package 下有 a.py 和 b.py 两个文件，在 a.py 里 from . import b 即是相对导入 b.py。

注意:

1. import语句总是绝对导入,只有from-import语句才可以使用相对导入
2. 目前还没使用到相对导入,具体的用法还不是太清楚,平常都使用绝对导入

####12.8 模块的其他特性

#####自动载入的模块

```python
import sys
sys.modules.keys()
```

这个命令和```dir(__builtin__)```不同之处是前者是已载入模块,而后者是自动加载的模块.

#####阻止属性导入

上面提到过用```__all__```初始化"from module import *"导入,其实也可以在不想导入的属性名前加一个下划线(_).

#####源代码编码

```python
#-*- encoding: UTF-8 -*-  
```

一般我们在python中加中文注释,就得在源代码头部加入上面的注释,告诉解析器用UTF-8进行编码(python默认使用ASCII的Unicode编码的)

#####循环导入问题

书上说的不太明白,下面这篇文章比较好理解:
[解决循环import的问题](http://blog.csdn.net/handsomekang/article/details/19010407)

解决办法:

1. 延迟导入(lazy import)
即把import语句写在方法或函数里面，将它的作用域限制在局部。
这种方法的缺点就是会有性能问题。

2. 将from xxx import yyy改成import xxx;xxx.yyy来访问的形式

3. 组织代码
出现循环import的问题往往意味着代码的布局有问题。
可以合并或者分离竞争资源。
合并的话就是都写到一个文件里面去。
分离的话就是把需要import的资源提取到一个第三方文件去。
总之就是将循环变成单向。

