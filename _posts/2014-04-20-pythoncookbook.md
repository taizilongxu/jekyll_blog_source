---
title: "python-cookbook"
layout: post
category: python-cookbook
tags: [python,python-cookbook]
---


#第一章 文本

##1.1每次处理一个字符

```python
1. thelist = list(thestring)
     for c in tehstring:
        do_something_with(c)
2. results = [do_something_with(c) for c in thestring]
3. results = map(do_something,thestring)
```

##1.2字符和字符值之间的转换

将一个字符转化为相应的ASC或Unicode

```python
>>>print ord('a') 97 >>>print chr(91)
a

>>>print ord(u'\u2020')
8224
>>>print repr(unichr(8224))
u'\u2020'

```

##1.3测试一个对象是否是类字符串

```python
isinstance 和 basestring
basestring是str 和unicode的共同基类

def isAString(anobj):
    return isinstance(anobj,basestring)


```
##1.4字符串对齐

```python
>>>print '|','hej'.ljust(20),'|','hej'.rjust(20),'|','hej'.center(20),'|'
 |hej                  |                  hej |         hej          |
```

##1.5去掉字符串两端的空格

```python
>>>x = '    hej    ' >>>print '|',x.lstrip(),'|',x.rstrip(),'|',x.strip(),'|'
 | hej     |     hej | hej |
```

##1.6合并字符串

1.大量字符优先,可以用中间数据list来容纳后来的字符，最后再一并处理

```python
largeString = ''.join(pieces)
```
2.少量字符优先

```python
largeString = '%s%s something %s yet more' % (small1,small2,small3)
```

3.效率低下，会产生许多字符串的中间结果

```python
largeString = small1 + small2 + 'something' + small3 + 'yet more' 4. import operator
largeString = reduce(operator.add,pieces,'')
```

##1.7将字符串逐个字符或逐个词进行反转

1.字符，切片方法

    revchars = astring[::-1]
2.单词

```python
revwords = astring.split()
revwords.reverse()
revwords = ' '.join(revwords)
revwords = ' '.join(astring.split()[::-1])
```

reversed返回一个迭代器

##1.8检查字符中是否包含某字符集合中的字符

```python
def containsAny(seq,aset):
    for c in aset:
    	if c inaset:
        	return True
    return False
```

##1.9简化字符串的translate方法的使用

##1.10过滤字符串中不属于指定集合的字符（translate）

__Perl的判定方法，如果字符串中包含了空值或者其中有超过30%的字符的高位被置为1或是奇怪的控制码，我们就认为这段数据是二进制数据__

```python

from __future__ import division
import string
text_characters = ''.join(map(chr,range(32,127))) + '\n\r\t\b'
_null_trans = string.maketrans('','')

def  istext(s,text_characters = text_characters,threshold=0.30):
    if '\0' in s:
    return False

    if not s:

    return True

t = s.translate(_null_trans,text_characters)

return len(t)/len(c) <= threshold
```

##1.12控制大小写

big = little.upper()
little = big.lower()

```python
>>>print 'one tWo thrEe'.capitalize()
One two three

>>>print 'one tWo threEe'.title()
One Two Threee
```

##1.13访问子字符串

1.切片方式

```python
afield = theline[3:8]
```

2.struct.unpack方法

```python
import struct
format = '5s 4x 3s'
#取前5个字符，跳过4个字符，再取3个字符
print ''.join(struct.unpack(format,'Test astring'))

结果: Testing
```

##1.14改变多行文本字符串的缩进
1.统一的缩进

```python
defa reindent(s,numSpace):
    leading_space = numSpaces * ' '
    lines = [leading_space + line.strio() for line in s.splitlines()]
    return '\n'.join(lines)
```

2.相对缩进

```python
def addSpaces(s,numAdd):
    white = " " * numAdd
    return white + white.join(s.splitlines(True))
def numSpaces(s):
    return [len(line) - len(line.lstrip()) for line in s.splitlines()]
def delSpaces(s,numDel):
    if numDel > min(numSpaces(s)):
        raise ValueError,"removing more spaces than there are!"
    return '\n'.join([line[numDel:] for line in s.splitlines()])
```

3.让缩进最小的行与左端对齐

```python
def unIndentBlock(s):
    return delSpaces(s,min(numSpaces(s)))
```

#第二章 文本

##2.1读取文件

最方便的办法

```python
all_the_text = open(‘thefile.txt').read()
```

保证文件被关闭

```python
file_object = open(‘thefile.txt') try: all_the_text = file_object.read() finally: file_object.close()
```

逐行读取

```python
list_of_all_the_lines = file_object.readlines()
```

这样读取每行会带一个\n 用这个方法解决

```python
line = line.rstrip(‘\n')
```

也可以用一个for循环

```python
for line in file_object:
	process line
```

##2.2写入文件

最方便的方法：

```python
open('thefile.txt','w').write(all_the_text)
```

不过最好还是要创建个名字，最后才能调用close() 如果想写入的数据是一个列表，那么可以这样：

```python
file_object.writelines(list_of_text_strings)
```

也可以先把字串拼接成大字符串，再调用write写入，或者在循环中写入，但是没有这种方法方便快捷

##2.3搜索和替换文件中的文本

```python
output_file.write(input_file.read().replace(stext,rtext))
```

##2.4从文件中读取指定的行

如果文件不大的话：

```python
import linecache
theline = linecache.getline(thefilepath,desired_line_number)
```

如果文件太大则：

```python
def getline(thefilepath,desired_line_number):
	if desired_line_number < 1:
    	return ‘ ‘
    for current_line_number,line in enumerate(open(thefilepath,'rU')):
    	if current_line_number == desired_line_number - 1:
    		return line return ‘ ‘
```

##2.5计算文件行数

小文件：

```python
count = len(open(thefilepath,'rU').readlines())
```

对于大文件：

```python
count = -1
for count,line in enumerate(open(thefilepath,'rU')):
	pass count += 1
```

还有一种更巧妙的方法，寻找\n结尾的次数:

```python
count = 0
thefile = open(thefilepath,'rb')
while True:
	buffer = thefile.read(8192*1024)
    if not buffer:
    	break
    count + = buffer.count(‘\n')
thefile.close()
```

##2.6处理文件中的每个词

使用两重循环

```python
for line in open(thefilepath):
	for word in line.split():
    	dosomethingwith(word)
```

使用正则表达式(词被定义为数字字母，连字符或单引号构成的序列)：

```python
import re
re_word = re.compile(r”[\w'-]+”)
for line in open(thefilepath):
	for word in re_word.finditer(line):
    	dosomethingwith(word.group(0))
```

##2.7随即输入/输出

2进制文件

```python
thefile.seek(offset) buffer = (thefile.read(size))
```

##2.16遍历目录树

glob.glob
当前目录下的所有文件！！！
os.walk
返回一个三元组：

* dirpath是以string字符串形式返回该目录下所有的绝对路径；
* dirnames是以列表list形式返回每一个绝对路径下的文件夹名字；
* filesnames是以列表list形式返回该路径下所有文件名字。

```python
for i in os.walk(‘/home/limbo/code/python'):
	for name in i[2]:
    	print i[0] + ‘/‘ + name
```

##2.17在目录树中改变文件扩展名

```python
import os
def sqapextensions(dir,before,after):
    if before[:1]  != '.':

        before = '.' + before #如果没有'.'加

    thelen = -len(before)

    if after[:1] != '.':

        after = '.' + after

    for path,subdirs,files in os.walk(dir):#迭代

        for oldfile in files:

            if oldfile[thelen:] == before

                oldfile = os.path.join(path,oldfile)

                newfile = oldfile[:thelen] + after

                os.rename(oldfile,newfile) #重命名

if __name__ == '__main__'
    import sys

    if len(sys.argv) == 4

        print "Usage:sqapext rootdir befor after"

        sys.exit(100)

    swapextensions(sys.argv[1],sys.argv[2],sys.argv[3])
```

##2.18从指定搜索路径搜索文件

```python
import os
def search_file(filename,search_path,pathsep = os.sep):
    for path in search_path.split(pathsep):
        candiate = os.path.join(path,filename)
        if  os.path.isfile(candiate):
            return os.path.abspath(candidate)
    return None

if __name__ == '__main__'
    search_path =
    find_file = search_file('ls',search_path)
    if find_file:
        print "File 'ls' found at %s" % find_file
    else:
        print "File 'ls' not found"
```

##2.19根据指定的搜索路径和模式寻找文件

```python
import glob,os

def all_file(pattern,search_path,pathsep = os.sep):

    for path in search_path.split(pathsep):

        for match in glob.glob(os.path.join(path,pattern)):

            yield match #生成迭代器
```

