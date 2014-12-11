---
layout: post
title:  "javascript基础语法笔记"
category: javascript
tags: javascript
---

javascript的语法相比于c和python来说真是简单不少.下面是javascript的基础语法,作为笔记记录,这篇笔记假设有Python和C的基础.只列出一些需要记忆和区别的地方.


## 1 语法

### 1.1 语句

三种形式:

每条语句放在不同行:

```javascript
first statement
second statement
```

放在同一行:

```javascript
first statement; second statement
```

建议写法,通用写法:

```javascript
first statement;
second statement;
```

### 1.2 注释

单行:

```javascript
//注释
```

多行:

```javascript
/*注释
  注释
  注释*/
```

### 1.3 变量

javascript允许程序员直接对变量赋值而无需事先声明.

```javascript
mod = "happy";
age = 33
```

但提前声明变量是一种良好的编程习惯.

```javascript
var mod;
var age;
或者
var mod, age;
```

在javascript里变量和其他语法元素的名字都是**区分字母大小写**的.

### 1.4 数据类型

1. 字符串:单双引号都可以(同Python一样),个人感觉双引号更好,不过随便,程序保持一致最重要.
	
    ```javascript
    var mood = 'happy';
    var mood = "happy";
    var mood = "don't ask";
    var mood = 'don\'t ask';
    var mood = "about 5'10\" tall";
    ```
2. 数值:整数,浮点数,负数.
	
    ```javascript
    var age = 33.25;
    var temperature = -20;
    var temperature = -20.3333333;
    ```
3. 布尔值:和Python不一样的是关键字是小写,`true`和`false`

	```javascript
    var sleeping = true;
    ```

### 1.5 数组

4个元素的数组:

```javascript
var beatles = Array(4);
```

无法预支元素个数:

```javascript
var beatles = Array();
```

在声明数组时进行赋值:

```javascript
var beatles = Array("John", "Paul", "George", "Ringo");
```

还有一种方法创建,和python列表一样:

```javascript
var beatles = ["John", "Paul", "George", "Ringo"];
```

数组元素可以混合类型:

```javascript
var lennon = ["John", 1940, false]
```

数组元素还可以是变量:

```javascript
var name = "John";
beatles[0] = name;
```

元素赋值:

```javascript
array[index] = element;
```

数组基本和Python语法一致.

### 1.6 对象

与数组类似.对象的每个值都是对象的一个属性.

```javascript
var lennon = Object();
lennon.name = "John";
lennon.year = 1940;
lennon.living = false;
```

还有一种方法,和Python的字典一个样:

```javascript
var lennon = {name:"John", year:1940, living:false};
或者
var lennon = {};
lennon.name = "John";
lennon.year = 1940;
lennon.living = false;
```

这里的属性名和Javascript的变量名命名规则相同,属性值可以是任何Javascript值,包括对象.

### 1.7 算术操作符

这个没什么好说的了:

```javascript
var total = (1 + 4) * 5;
year = year + 1;
year++;
year += 1;
```

数值和字符串拼接,这里数值将会自动转换为字符串进行相加:

```javascript
var year = 2005;
var message = "The year is " + year;
```

## 2 控制语句

### 2.1 条件语句

基本语法:

```javascript
if (condition){
    statements;
} else {
    statements;
}
```

#### 2.1.1 比较操作符

还是那几种:`>`,`<`,`<=`,`>=`,`==`,`!=`.

最后还有一种和别的语言不一样的:`===`,这个全等操作符会执行严格的比较,不仅比较值,而且会比较变量的类型.例如:

```javascript
var a = false;
var b = "";
a == b; // true
a === b; //false
```

#### 2.1.2 逻辑操作符

逻辑与`&&`,逻辑或`||`,逻辑非`!`.

### 2.2 循环语句

#### 2.2.1 while循环

```javascript
while (condition) {
    statements;
}

do{
	statements;
} while(condition);
```

例子:

```javascript
var count = 1;
while(count < 11){
	alert(count);
    count++;
}
```

#### 2.2.2 for循环

```javascript
for (initial condition; test condition; alter condition){
	statements;
}
```

例子:

```javascript
for (var count = 1; count < 11; count++){
	alert(count);
}
```

## 3 函数

函数的作用就是重用和封装,每个语言都差不多.先定义,后调用.

```javascript
function name(arguments){
	statements;
}
```

例子:

```javascript
function shout(){
	var beatles = ["John", "Paul", "George", "Ringo"];
    for (var count = 0; count < beatles.length; count++){
    	alert(beatles[count]);
    }
}
```

调用:

```javascript
shout();
```

还可以return:

```javascript
function converToCelsius(temp){
	var result = temp - 32;
    result = result / 1.8;
    return result;
}
```

#### 命名规则:

1. 变量:下划线分隔各个单词.例如`temp_celsius`.
2. 函数:从第二个单词开始把每个而单词的第一个字母写成大写形式(驼峰命名).例如`convertToCelsius()`

#### 变量的作用域

* 全局变量:可以在脚本中任何位置引用.注意:在函数里也可以引用.
* 局部变量:只存在于声明它的那个函数内部,在函数外部是无法引用的.

所以,我们在函数里既可以使用全局变量,也可以用局部变量.

BUG:如果我们在函数里的变量和全局变量重名,那么我们不能轻易使用,必须提前声明,所以声明是个好习惯.

```javascript
total = 100
function square(num){
	var total = num * num;
    // 不能这样写 total = num * num;
    return total;
}
```

## 4 对象

属性,方法,实例自不必说了.

#### 4.1 内建对象

提供的预先定义好的对象,这些可以拿来就用的对象称为内建对象(native object).

比如,Array对象,Math对象和Date对象.

#### 4.2 宿主对象

由浏览器提供的预定义对象被称为宿主对象(host object).

宿主对象包括Form,Image和Element等.




