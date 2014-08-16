---
layout: post
title:  "知乎面试题"
category: python
tags: python
---

百度上搜了搜知乎面试题,就搜到了一个,汗!原地址(http://blog.csdn.net/liushuaikobe/article/details/9199789)

于是就动手做了做,发现还是很基础的东西

![](http://img.blog.csdn.net/20130628212639453?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1c2h1YWlrb2Jl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

按照作者的思路做了下,看了看HTMLParse的源码,于是就不用自己造轮子了

```python
#---------------------------------import---------------------------------------
from HTMLParser import HTMLParser
#------------------------------------------------------------------------------

class MyHTMLParser(HTMLParser):
	
    def __init__(self):
        HTMLParser.__init__(self)
        self.test = ['a','abbr','acronym','b','blockquote','cite','code','del','em','i','q','strike','strong','pre']
        self.newdata = []
        self.flag = 1

    def handle_starttag(self, tag, attrs):
        #处理开始标签,如果为选中标签则添加原始标签到self.newdata,否则pass掉
        if tag in self.test:
            s = ''
            for i in attrs:
                s += ' ' + i[0] + '=' + '"' + i[1] + '"'
            self.newdata.append("<%s%s>" % (tag,s))
        else:
            self.flag = 0

    def handle_data(self,data):
    	#对原始数据进行添加
        if self.flag:
            print data.strip()
            self.newdata.append(data)
        else:
            self.flag = 1

    def handle_endtag(self,tag):
    	#处理开始标签同样的方法,加入self.newdata
        if tag in self.test:
            self.newdata.append("</%s>" % tag)
###############################################################################
if __name__ == "__main__":
    html_code = """
    <abbr>This is legal abbr text.</abbr>
    <a href="http://www.zhihu.com" id="bad_id">
    <strong name="bad_name" id="bad_id" class="bad_class">This is a legal strong href</strong>
    </a>
    <head>This is a bad head</head>
    <acronym title="acronym titel"></acronym>
    </br>
    </em>
    <body>This is bad body.</body>
    <title class="bad_class">This is bad title.</title>
    """
    hp = MyHTMLParser()
    hp.feed(html_code)
    print hp.newdata
    for i in hp.newdata:
        print i,

```

**思路是这样的,因为HTMLParse已经帮助解析了HTML的标签,提供了handle_*函数来让我们重写,这道题我们只需要重写以下handle_starttag,handle_endtag,handle_data就可以了,然后对我们要求的标签进行过滤**

![](http://img.blog.csdn.net/20130718165853078?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl1c2h1YWlrb2Jl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**这道题原作者用的是hash来处理,其实python的字典已经是hash过的,这里直接用字典来处理,代码如下**

```python
#-*- encoding: UTF-8 -*-
"""This code is to deal with the log"""
#---------------------------------import------------------------------------
import glob
#---------------------------------------------------------------------------
class Parse(object):
    """To deal with the log"""
    def __init__(self):
        """
        files to store the path of files everyday,
        id_list to store the answer of the first,
        user is a dict like that : {day : {Id : [topic]}}.
        """
        self.files = []
        self.id_list = []
        self.user = {}

    def last(self):
        """The fist question"""
        id_list = []
        tmp = []
        for users in self.user:
            for ids in self.user[users]:
                tmp.append(ids)
            if id_list:
                id_list = list(set(id_list) & set(tmp))
            else:
                id_list = tmp
            tmp = []
        self.id_list = id_list
        print id_list

    def last2(self):
        """The second question"""
        path_list = []
        tmp = []
        for users in self.user:
            for ids in self.id_list:
                if ids in self.user[users]:
                    for path in self.user[users][ids]:
                        tmp.append(path)
            tmp = [path for path in tmp if tmp.count(path) > 1]
            if path_list:
                path_list = list(set(path_list) & set(tmp))
            else:
                path_list = tmp
            tmp = []
        print path_list

    def day_file(self, i):
        """get the path of the day files"""
        self.files = glob.glob(r'/home/limbo/test/test2013-1-%d-*.log' % i)
        self.day_control(i)

    def day_control(self, i):
        """To collect the Id and the topic satisfied require by day"""
        day_dict = {}
        for day_file in self.files:
            with open(day_file) as open_file:
                for line in open_file:
                    line_info = line.split(' ')
                    ids = line_info[3]
                    style = line_info[5]
                    path = line_info[6]
                    if style == 'GET' and path[:7] == '/topic/':
                        if ids in day_dict:
                            day_dict[ids].append(path)
                        else:
                            day_dict[ids] = [path]
        #end for
        day_dict_new = {}
        for lst in day_dict:
            list1 = day_dict[lst]
            list2 = {}.fromkeys(list1).keys()
            if len(list2) > 1:
                day_dict_new[lst] = list2
        self.user[i] = day_dict_new

    def month_control(self):
        """To collect the Id and the topic by month"""
        for day in range(1, 31):
            self.day_file(day)
            self.day_control(day)
def main():
    html = Parse()
    html.month_control()
    html.last()
    html.last2()


############################################################################
if __name__ == '__main__':
    main()
```
1. **思路是这样的,用day_file函数处理文件写入的问题,这里用到了glob,然后对同一天的日志进行筛选,选出符合第一问的用户ID和路径(其中ID是键,路径是值),最后存到一个字典中,这个字典是self.user,这里键是一个月的哪一天,值是对应的每天的ID和路径这样30天的用户和路径都存到了一个列表里**

2. **最后用last函数处理第一问,按照作者的思路用集合比较方便,所以沿用作者的方法,对30天的用户ID用了一个集合交集处理,最后得出来的用户ID就是所求ID**

3. **last2函数处理第二问,和last函数功能差不多,不过中间的ID取值在上一个保存的Id_list中取出,能减少处理所需时间.**

4. **通过作者给出的日志生成文件,结果一致**