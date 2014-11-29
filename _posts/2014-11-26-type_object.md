---
layout: post
title:  "为什么要有type和object?"
category: python
tags: python
---

图片来源 http://wiki.woodpecker.org.cn/moin/PyTypesAndObjects

![](https://raw.githubusercontent.com/taizilongxu/taizilongxu.github.io/master/img/types_map.png)

先解释一下图片,虚线表示一个对象的type(类型),实线表示一个对象的base(基类/父类).

这个图片很有意思的一个地方是`<type 'type'>`和`<type 'object'>`之间的关系,可以看到`<type 'object'>`是`<type 'type'>`的基类,而`<type 'type'>`是`<type 'object'>`的type(类型),这到底为什么?(这里先拿新式类来说,旧式类后面在说.)

我觉得最主要的原因是来源于Python **一切皆为对象** 的概念,在Python中所有的类或实例(比如list,int,1,'abc')都是对象.

如果要你设计会怎么设计?那就是设计一个超类,所有的对象的超集都是一个`<type 'object'>`类(也就是图上的`<type 'object'>`),即所有的对象都继承这个object类.这就是`<type 'object'>`类的由来.

再看看`<type 'type'>`是怎么来的.看图,先撇开`metaclasses`,来看右边两个矩形.可以看出对象分两种:

1. classes(类)
2. instances(实例)

类和实例有什么关系呢?类创造了实例,而不是继承关系.那么问题来了,是什么创造了类呢?于是在Python里引进了`metaclasses`,`<type 'type'>`就是元类,它可以创建类.记住 **Python中的一切都是对象，它们要么是类的实例，要么是元类的实例** .

这样我们就统一了对象的创建,所有的对象都是`metaclasses`创建的,而所有的对象都是`<type 'object'>`的子集,这就是为什么我们分别需要`type`和`object`这两个东西~~最后,`<type 'type'>`和`<type 'object'>`之间的实线部分,那是因为 **一切皆为对象** ,所以`type`自然是`object`的子集了.

总之, **`type`是所有对象的妈或者奶奶,创建所有对象(包括自己,它的`__class__`为`<type 'type'>`),`object`是所有对象的祖宗(不包括自己,它的`__base__`为空),所有对象继承祖宗的血统** ,就酱紫.

ps:

* 不能通过形式上判断类的类型,应当从元类的类型来确定:古典类的元类为`types.ClassType`,新式类的元类为`type`,可以通过设置`__metaclass__`来设置一个类的元类.
* `type(*)`等同于`*.__class__`
* 每个对象都有class，并且等于该对象的type.
* 每个类(classes)或者type有bases属性，而实例(instances)则没有.只有`<type 'object'>`的bases是空的.
* 要通过子类化构建对象,我们使用class关键字，并指定新对象的基类bases (或者可选的 type) . 这样通常创建出的是type object.
* 要通过实例化构建对象, 需要使用在类对象上使用调用操作符即小括号 (())

对于上一个问题可以引出为什么要加入新式类?

```python
class A:  # 旧式类
	pass

class B(object):  # 新式类
	pass
```

假设a,b分别是类A,类B的实例,则:

| 对象 | `type<*>` | `*.__class__` | `*.__bases__` | `isinstance(*, object)` | `isinstance(*, type)`|
|--------|--------|--------|--------|--------|--------|
| object | `<type 'type'>` | `<type 'type'>` | () | √ | √ |
| type | `<type 'type'>` | `<type 'type'>` | (`<type 'type'>`,) | √ | √ |
| a | `<type 'instance'>` | `<type 'type'>` | () | √ | ×|
| b | `<class '__main__.B'>` | `<class '__main__.B'>` | (`<type 'type'>`,) | √ | × |

可以看到对于旧式类,它的`type`和`*.__class__`还有`*.__bases__`比较混乱,而新式类就是为了解决这个问题,做到了大一统.

新式类有很多优势:基于内建类型构建新的用户类型,支持property和描述符特性等.作为新式类的祖先,Object类中还定义了一些特殊方法,如:`__new__()`,`__init__()`,`__delattr__()`,`__getattribute__()`,`__setattr__()`,`__hash__()`,`__repr__()`,`__str__()`等等.Object的子类可以对这些方法进行覆盖以满足自身的特殊需求.

另外，新式类和旧式类还有一个区别就是在多继承的时候，查找要调用的方法。新式类是广度优先的查找算法。旧式类的查找方法是深度优先的.
