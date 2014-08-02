---
layout: post
title:  "Core Python programming（5）"
category: Core Python programming
tags: python
---

chapter 9

###第九章 文件和输入输出

这章比较基础,主要讲一下比较容易弄混的地方,在pythoncookbook这本书中第二章已经详细的介绍过了,不再重复了.

```python
file_object = open(file_name,access_mode = 'r',buffering = -1)
```

access_mode是一个字符串:

* r:读取
* w:写入
* a:追加
* U:通用换行符支持

其中要注意的几点:

* 使用r或U模式打开的文件必须是已经存在的.
* 使用w模式打开的文件若存在则首先清空,然后创建.
* 以a模式打开的文件是为追加数据作准备的,所有写入的数据都将追加到文件的末尾

print语句默认在输入内容末尾加一个换行符,而在语句后加一个逗号就可以避免这种行为.

read(),readline(),readlines()之间的差异:

* read() 每次读取整个文件，它通常用于将文件内容放到一个字符串变量中.
* readlines()自动将文件内容分析成一个行的列表，该列表可以由 Python 的 for... in ... 结构进行处理,它也是一次性的将所有文件全部读入,对于很大的文件不太适合这种方法.
* readline()每次只读取一行，通常比readlines()慢得多。仅当没有足够内存可以一次读取整个文件时，才应该使用readline().



writelines()行结束符并不会自动加入,所以调用前给每个结尾加上行结束符.

####文件对象的方法:

|文件对象方法|操作|
|:---|:---|
|file.close()|关闭文件|
|file.read(size=-1)|从文件读取size个字节,当未给定size或给定负值的时候,读取剩余的所有字节,然后作为字符串返回|
|file.readline(size=-1)|从文件中读取并返回一行(包括行结束符),或返回最大size个字符|
|file.readlines(sizhint=0)|读取文件的所有行作为一个列表返回(包括所有的行结束符);如果给定的sizhint大于0,那么将返回总和大约为sizhint字节的行(以最小缓冲器大小为主)|
|file.seek(off,whence=0)|在文件中移动文件指针,从whence(0代表文件启示,1代表当前闻之,2代表文件末尾)便宜off字节|
|file.write(str)|向文件写入字符串|
|file.writelines(seq)|向文件写入字符串序列seq;seq应该是一个返回字符串的可迭代对象|
|file.fileno()|返回文件的描述符|
|file.tell()|返回当前在文件中的位置|
|file.truncate(n)|从文件的首行首字符开始截断，截断文件为n个字符；无n表示从当前位置起截断；截断之后n后面的所有字符被删除。其中win下的换行代表2个字符大小。|

####文件内建属性:

|文件对象属性|描述|
|:---|:---|
|file.closed|表示文件已经被关闭,否则为False|
|file.encoding|文件所使用的编码--当Unicode字符串被写入数据时,他们将自动使用file.encoding转换为字符串;若file.encoding为None时使用系统默认编码.尽量将编码保持一直,建议UTF-8编码|
|file.mode|文件打开时使用的访问模式|
|file.name|文件名|
|file.newlines|如果文件刚被打开, 程序还没有遇到行结束符, 那么文件的newlines 为 None .在第一行被读  取后, 它被设置为第一行的结束符. 如果遇到其它类型的行结束符, 文件的newlines 会成为一个  包含每种格式的元组.|
|file.softspace|为0表示在输出一数据后,要加上一个空格符,为1表示不加.这个属性一般不用

####OS模块总结

有助于跨平台开发的 os 模块属性:

|os 模块属性  |   描述|
|:---|:---|
|linesep    |     用于在文件中分隔行的字符串 例如，Windows使用’\r\n’，Linux使用’\n’而Mac使用’\r’ |
|sep        |     用来分隔文件路径名的字符串  |
|pathsep     |    用于分隔文件路径的字符串  |
|curdir      |     当前工作目录的字符串名称  |
|pardir      |    (当前工作目录的)父目录字符串名称  |

不管你使用的是什么平台, 只要你导入了 os 模块, 这些变量自动会被设置为正确的值, 减少了你的麻烦.  

os模块常用函数:

|函数|描述|
|:---|:---|
|文件处理|
|remove()/unlink()|删除文件|
|rename()/renames()|重命名文件|
|stat()|返回文件信息|
|walk()|生成一个目录下的所有文件|
|目录/文件夹|
|chdir()/fchdir()|改变当前工作目录/通过一个文件描述符改变当前工作目录|
|chroot()|改变当前进程的根目录|
|listdir()|列出制定目录文件|
|getcwd()/getcwdu()|返回当前工作目录/功能相同,但返回一个Unicode对象|
|rmdir()/removedirs()|删除目录/删除多层目录|
|getenv()/putenv()|分别用来读取和设置环境变量|
|makedirs(path) |多层创建目录|
|mkdir(path) |创建目录|
|执行系统命令|
|system()|用来运行shell命令。|

os.path模块:

|函数|描述|
|:---|:---|
|os.path.isdir(name)|判断name是不是一个目录，name不是目录就返回false|
|os.path.isfile(name)|判断name是不是一个文件，不存在name也返回false|
|os.path.exists(name)|判断是否存在文件或目录name|
|os.path.getsize(name)|获得文件大小，如果name是目录返回0L|
|os.path.abspath(name)|获得绝对路径|
|os.path.normpath(path)|规范path字符串形式|
|os.path.split(name)|分割文件名与目录（事实上，如果你完全使用目录，它也会将最后一个目录作为文件名而分离，同时它不会判断文件或目录是否存在）|
|os.path.splitext()|分离文件名与扩展名|
|os.path.join(path,name)|连接目录与文件名或目录|
|os.path.basename(path)|返回文件名|
|os.path.dirname(path)|返回文件路径|


####Python创建目录文件夹

主要涉及到三个函数:

 1. os.path.exists(path) 判断一个目录是否存在
 2. os.makedirs(path) 多层创建目录
 3. os.mkdir(path) 创建目录

```python
# 引入模块
importos
defmkdir(path):
	path=path.strip()# 去除首位空格
	path=path.rstrip("\\")# 去除尾部 \ 符号
	isExists=os.path.exists(path)# 判断路径是否存在
    
	ifnotisExists:# 存在     True
		printpath+' 创建成功'
		os.makedirs(path)# 创建目录操作函数
		return True
	else:# 如果不存在则创建目录
		printpath+' 目录已存在'
		returnFalse

mkpath="d:\\qttc\\web\\"# 定义要创建的目录
mkdir(mkpath)# 调用函数
```

说明

在以上DEMO的函数里，我并没有使用os.mkdir(path)函数，而是使用了多层创建目录函数os.makedirs(path)__。这两个函数之间最大的区别是当父目录不存在的时候os.mkdir(path)不会创建，os.makedirs(path)则会创建父目录__

比如：例子中我要创建的目录web位于D盘的qttc目录下，然而我D盘下没有qttc父目录，如果使用os.mkdir(path)函数就会提示我目标路径不存在，但使用os.makedirs(path)会自动帮我创建父目录qttc，请在qttc目录下创建子目录web。




