---
layout: post
title: "Python基础教程第二版笔记"
date: 2013-10-11 16:49:57 +0800
categories: scripting
comments: false
sharing: false
published: false
---

## 基础知识

#### 交互式解释器
当启动IDLE或者从命令行执行python命令的时候，会打开Python的交互式解释器

    Python 2.7.5 (default, May 15 2013, 22:43:36) [MSC v.1500 32 bit (Intel)] on win32
    Type "copyright", "credits" or "license()" for more information.
    >>> 

解释器可以用来直接执行Python语句，并看到效果；`>>>`是解释器的提示符，表示可以输入语句或者表达式。（本文示例大多都在解释器中进行）

#### 数字和表达式
解释器可以当作功能强大的计算器使用，如：

    >>> 2 + 2
    4
    >>> 1 / 2        # 一个整数除另外一个整数，计算结果的小数部分将被截除，得到一个整数
    0
    >>> 1.0 / 2.0    # 实数在Python中被称为浮点数，参与除法的两数中有浮点数，结果为浮点数
    0.5

如果希望Python只执行普通的除法，可以在程序前加上（或直接在解释器里执行）`from __future__ import division`，如：

    >>> from __future__ import division		# future前后各2个下划线
    >>> 1 / 2
    0.5
    >>> 1 // 2								# Python中用于实现整除的操作，双斜线 //
    0

Python中还有诸如取余、幂等运算，如：

    >>> 10 / 3
    3
    >>> 10 % 3
    1
    >>> 2.75 % 0.5
    0.25
    >>> 2 ** 3
    8

#### Python中的长整型
长整型和普通整形书写方法一样，但是结尾有个L（小写也可以，不过为了便于与1区分，习惯上用大写）

    >>> 100000000000000000000000
    100000000000000000000000L

#### 十六进制和八进制

    >>> 0xAF			# 16进制整数，以0x开头
    175
    >>> 010				# 8进制整数，以0开头
    8

#### 变量
Python中的变量代表某个值的名字，变量名可包括字母、数字或者下划线，变量名不能以数字开头。如果希望x代表3，则

    >>> x = 3			# 赋值语句，将值3赋给x，或者说将x绑定到3

#### 语句
语句与表达式的区别在于，语句表示做某事，而表达式表示是某事，如

    >>> print 2 * 2		# 语句，打印4
    4
    >>> 2 * 2			# 表达式， 2 * 2是4
    4

<!-- more -->
#### 函数
函数可以理解为用来实现特定功能的小程序，如之前的幂运算可以用函数`pow`来代替

    >>> 2 ** 3
    8
    >>> pow(2, 3)
    8

使用函数的方式叫做调用函数，可以给其提供参数，它会返回值给用户；`pow`是Python中的标准函数，也称为内建函数；相对的还有自定义函数（即用户自己定义的函数）。

内建函数

    >>> abs(-1)				# 求绝对值
    1
    >>> round(1.0/2.0)		# 四舍五入求近似值
    1.0

#### 模块
可以将模块想象增强功能的扩展，可通过`import`命令导入模块

    >>> import math
    >>> math.floor(32.9)			# 模块中的函数可以通过 模块.函数 的形式来调用
    32.0
    >>> from math import sqrt		# 在不会导入同名函数的情况下，还可以
    >>> sqrt(9)						# 以from 模块 import 函数形式导入后，可以直接调用函数
    3.0

#### `__future__`
通过导入`__future__`可以获得那些在未来会成为标准的Python的部分特性。

#### 注释
Python的注释以`#`号开头，`#`号之后的一切将被解释器忽略。

#### 字符串
字符串是一系列由引号（`"`或者`'`）引起来的字符序列，字符串开始和结束的引号必须一致；Python中`'`和`"`没有本质的不同，通常可以：

    >>> "Let's go!"
    >>> '"Hello, world!" she said'

转义字符串，即用反斜线（`\`）对字符串中的符号进行转义

    >>> 'Let\'s go!'
    >>> 'Let's go!'
    SyntaxError: invalid syntax

字符串可以用加法运算进行拼接

    >>> "Hello, " + "World!"
    'Hello, World!'

**`repr`和`str`**  
Python中值可以被转换成字符串，有2种机制：

    >>> print repr("Hello, World!")		# repr会创建一个字符串，以合法的表达式的形式表示值
    'Hello, World!'
    >>> print repr(10000L)
    10000L
    >>> print str("Hello, World!")		# str把值转换为合理形式的字符串，便于用户理解
    Hello, World!
    >>> pritn str(10000L)
    10000

**长字符串**  
如果需要一个很长的字符串，例如跨多行的字符串，则，可以用3个引号代替普通引号。

    print '''This is a very long string.
    It continues here.
    And it's not yet over...'''

普通字符串也可以跨行，如果一行之中的最后一个字符是反斜线（`\`），那么换行符就转义了，可以被忽略，如：

    print "Hello, \
    World!"

**原始字符串**  
原始字符串以`r`开头，不会把反斜线当成特殊字符，原始字符串中的每一个字符都与书写保持一致，如：

    >>> print r'Let\'s go!'
    Let\'s go!

**`Unicode`字符**  
类似原始字符串，以`u`开头。


***
## 列表和元组

**数据结构**  
数据结构是通过某种方式组织在一起的数据元素的集合。在Python中，最基本的数据结构是序列（sequence）。

序列中的每个元素被分配一个序号，即元素的位置，也称为索引，第一个索引是0，第二个是1，依次类推...

> 序列也可以从最后一个元素开始计数，最后一个元素标记为-1，倒数第二个为-2，依次类推...

#### 序列概览  
Python中有6中内建的序列，分别是*列表*、*元组*、*字符串*、*Unicode字符串*、*buffer对象*和*xrange对象*，这里着重讨论列表和元组。

列表和元组的主要区别在于，列表可以修改，而元组不能。

构建一个人员信息的列表，如：

	>>> edward = ['Edward Gumby', 42]
	>>> john = ['John Smith', 50]
	>>> database = [edward, john]
	>>> database
	[['Edward Gumby', 42], ['John Smith', 50]]


#### 通用序列操作  
所有序列类型都可以进行某些特定的操作，如*索引（indexing）*、*分片（sliceing）*、*加（adding）*、*乘（multiplying）*以及检查某个元素是否是序列的成员等。

Python中还有计算序列长度、找出最大元素和最小元素的内建函数。

**索引**  
例：

	>>> greeting = 'Hello'		# 字符串序列
	>>> greeting[0]				# 从头开始索引，始于0
	'H'
	>>> greeting[-1]			# 从末尾开始索引，始于-1
	'o'
	>>> 'Hello'[1]				# 字符串字面值可直接使用索引
	'e'
	

**分片**  
分片操作可以访问一定范围内的元素，通过冒号相隔的两个索引来实现，如：

	>>> tag = '<a href="http://www.python.org">Python web site</a>'
	>>> tag[9:30]
	'http://www.python.org'
	>>> tag[32:-4]
	'Python web site'
	
分片操作中，第一个索引是需要提取部分的第一个元素的编号，第二个索引则是分片之后剩下的第一个元素的编号。

例：

	>>> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
	>>> numbers[3:6]
	[4, 5, 6]
	>>> numbers[0:1]
	[1]
	
*优雅的捷径*  
分片也可以从序列的结尾开始计数，如：

	>>> numbers[-3:-1]			# 访问不到最后一个元素
	[8, 9]
	>>> number[-3:0]			# 第一个元素比第二个元素晚出现，得到空序列
	[]
	>>> number[-3:]				# 置空第二个索引，访问到序列结尾
	[8, 9, 10]
	>>> numbers[:3]				# 也可置空开始的索引，表示从0开始
	[1, 2, 3]
	>>> numbers[:]				# 两个索引都置空，复制整个序列
	[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
	
*更大的步长*  
分片操作中还有第三个参数－步长（step length），步长通常是隐式设置的，默认为1。分片操作按照这个步长遍历序列的元素，返回开始和结束点之间的所有元素，如：

	>>> numbers[0:10:1]
	[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
	>>> numbers[0:10:2]
	[1, 3, 5, 7, 9]
	>>> numbers[3:6:3]
	[4]
	
> 步长不能为0

步长可以为负数，表示从右开始向左提取元素，如：

	>>> numbers[8:3:-1]
	[9, 8, 7, 6, 5]
	>>> numbers[10:0:-2]
	[10, 8, 6, 4, 2]
	>>> numbers[0:10:-2]		# 步长为负数时，第一个索引位置需要大于第二个索引位置
	[]
	>>> numbers[::-2]
	[10, 8, 6, 4, 2]
	>>> numbers[5::-2]
	[6, 4, 2]
	
**序列相加**  
可以通过加号来进行序列的连接操作，如：

	>>> [1, 2, 3] + [4, 5, 6]
	[1, 2, 3, 4, 5, 6]
	>>> 'Hello, ' + 'world!'
	'Hello, world!'
	>>> [1, 2, 3] + 'world!'
	Traceback (innermost last):
	…
	TypeError: can only concatenate list ( not "string") to list
	
两种相同类型的序列才能进行连接操作。

**乘法**  
用数字x乘以一个序列会得到一个新的序列，新序列中，原来的序列被重复x次，如：

	>>> 'python' * 5
	'pythonpythonpythonpythonpython'
	>> [42] * 10
	[42, 42, 42, 42, 42, 42, 42, 42, 42, 42]
	
*`None`、空列表和初始化*  
空值`None`表示里面什么也没有；空列表可以简单地通过一对中括号表示（`[]`），里面没有元素。

例：

	>>> sequence = [None] * 3
	>>> seuqence
	[None, None, None]
	
*成员资格*  
`in`运算符可以用来检查一个值是否在序列中，如：

	>>> permissions = 'rw'
	>>> 'w' in permissions				# 在序列中，返回True
	True
	>>> 'x' in permissions				# 不在序列中，返回False
	False
	>>> '$$$' in '$$$ get rich now!'	# Python 2.3开始，可以在字符串中检查是否存在某个子字符串
	True
	
*长度、最小值和最大值*  
内建函数`len`返回序列中元素的数量，`min`函数和`max`函数分别返回序列中值最大和最小的元素，如：

	>>> numbers = [100, 34, 678]
	>>> len(numbers)
	3
	>>> max(numbers)
	678
	>>> min(numbers)
	34
	>>> max(2, 3)
	3
	>>> min(9, 3, 2, 5)
	2
	
#### 列表：Python的“苦力”  
不同于元组和字符串，列表是可变的（mutable），并且有很多专门的方法。

**`list`函数**  
> `list`实际上是一种类型而不是函数

`list`函数可用来创建列表，如：

	>>> list('Hello')			# 根据不可变的字符串创建列表
	['H', 'e', 'l', 'l', 'o']
	>>> ''.join(somelist)		# 将字符组成的列表转化为字符串
	
**基本的列表操作**  

*元素赋值*  
例：

	>>> x = [1, 1, 1]
	>>> x[1] = 2				# 为索引为1的元素赋值
	>>> x
	[1, 2, 1]
	
> 不能为一个位置不存在的元素进行赋值，如果列表长度为2，就不能为索引为10的元素赋值。

*删除元素*  
从列表中删除元素，可以用`del`语句来实现，如：

	>>> names = ['Alice', 'Beth', 'Cecil']
	>>> del names[2]
	>>> names
	['Alice', 'Beth']
	
*分片赋值*  
分片赋值是一种一次为多个元素赋值的操作，值得一提的是，分片赋值时，可以使用与原序列不等长的序列进行替换；例如：

	>>> name = list('Perl')
	>>> name[1:] = list('ython')
	>>> name
	['P', 'y', 't', 'h', 'o', 'n']
	
分片赋值语句还可以用来插入新的元素或者删除元素，如：

	>>> numbers = [1, 5]
	>>> numbers[1:1] = [2, 3, 4]	# 在索引1的位置插入值
	>>> numbers
	[1, 2, 3, 4, 5]
	>>> numbers[1:4] = []			# 同 del numbers[1:4]
	[1, 5]
	
**列表方法**  
方法是一个与某些对象有紧密联系的函数，可用`对象.方法(参数)`的形式调用。

*`append`*  
`append`方法用于在列表末尾追加新的对象，如：

	>>> numbers = [1, 2, 3]
	>>> numbers.append(4)
	>>> numbers
	[1, 2, 3, 4]
	
*`count`*  
`count`方法用于统计某个元素在列表中出现的次数，如：

	>>> ['to', 'be', 'or', 'not', 'to', 'be'].count('to')
	2
	
*`extend`*  
`extend`方法可以在列表的末尾一次性追加另一个序列中的多个值，如：

	>>> a = [1, 2, 3]
	>>> b = [4, 5, 6]
	>>> a.extend(b)
	>>> a
	[1, 2, 3, 4, 5, 6]
	
> `extend`方法看起来和连接操作很像，区别在于`extend`方法修改了被扩展的序列（即是一个原地操作），而连接操作返回一个全新的列表。

*`index`*  
`index`方法用于从列表中找出某个值第一个匹配项的索引位置，如：

	>>> knights = ['We', 'are', 'the', 'knights']
	>>> knights.index('the')
	2
	>>> knights.index('who')
	Traceback (innermost last):
	…
	ValueError: list.index(x): x not in list
	
*`insert`*  
`insert`方法用于将对象插入到列表中，如：

	>>> numbers = [1, 2, 3, 5, 6, 7]
	>>> numbers.insert(3, 'four')
	>>> numbers
	[1, 2, 3, 'four', 5, 6, 7]
	
*`pop`*  
`pop`方法会从列表中移除一个元素（默认是最后一个），并且返回该值，如：

	>>> x = [1, 2, 3]
	>>> x.pop()
	3
	>>> x
	[1, 2]
	>>> x.pop(0)
	1
	>>> x
	[2]
	
> `pop`方法是唯一一个既能修改列表又返回元素值（除了None）的列表方法。

*`remove`*  
`remove`方法用于移除列表中某个值的第一个匹配项，如：

	>>> x = ['to', 'be', 'or', 'not', 'to', 'be']
	>>> x.remove('be')			# 原地操作
	['to', 'or', 'not', 'to', 'be']
	>>> x.remove('bee')
	Traceback (innermost last):
	…
	ValueError: list.remove(x): x not in list
	
*`reverse`*  
`reverse`方法将列表中的元素反向存放，如：

	>>> x = [1, 2, 3]
	>>> x.reverse()				# 原地操作
	>>> x
	[3, 2, 1]
	
> 如果要对一个序列进行反向迭代，可以使用`reversed`函数，这个函数返回一个迭代器对象，可以用`list`函数将返回转换成列表。
	
*`sort`*  
`sort`方法用于在原位置对列表进行排序，如：

	>>> x = [4, 6, 2, 1, 7, 9]
	>>> x.sort()
	>>> x
	[1, 2, 4, 6, 7, 9]
	
`sorted`函数可以用来获取排序后的列表副本，如：

	>>> y = sorted(x)
	>>> x
	[4, 6, 2, 1, 7, 9]
	>>> y
	[1, 2, 4, 6, 7, 9]
	
*高级排序*  
如果希望元素按照特定方式排序，可以给`sort`方法自定义比较函数`compare(x,y)`。`compare(x,y)`函数在x<y时返回负数，x>y时返回正数，x=y时返回0。

调用方式是将`compare`函数作为参数传给`sort`方法，内建函数`cmp`是默认的比较函数，例如：

	>>> cmp(42, 32)
	1
	>>> cmp(99, 100)
	-1
	>>> cmp(10, 10)
	0
	>>> numbers = [5, 2, 9, 7]
	>>> numbers.sort(cmp)
	>>> numbers
	[2, 5, 7, 9]
	
`sort`方法还有另外两个可选的参数`key`和`reverse`，如果要使用它们，可以通过名字来指定（这叫做关键字参数）。

参数`key`和参数`cmp`类似，必须提供一个在排序过程中使用的函数；该函数在排序过程中为每个元素提供一个键，所有元素根据键来排序，例如：

	>>> x = ['aardvark', 'abalone', 'acme', 'add']
	>>> x.sort(key=len)				# 根据元素长度来排序
	>>> x
	['add', 'acme', 'abalone', 'aardvark']
	
参数`reverse`是一个简单的布尔值(`True`或者`False`)，表示是否对列表进行反向排序，如：

	>>> x = [4, 6, 2, 1]
	>>> x.sort(reverse=True)
	>>> x
	[6, 4, 2, 1]
	
> `cmp`、`key`、`reverse`参数都可以用于`sorted`函数。


#### 元组：不可变序列  
用逗号分隔一些值，就创建了一个元组，如：

	>>> 1, 2, 3				# 逗号分隔一些值
	(1, 2, 3)
	>>> (1, 2, 3)			# 用圆括号括起来
	(1, 2, 3)
	>>> ()					# 空元素
	>>> 42,					# 长度为1的元组，需要逗号
	(42, )
	>>> (42,)
	(42,)
	
**`tuple`函数**  
`tuple`函数的功能是把一个序列转换为元组，如果序列本来就是元组，则原样返回，如：

	>>> tuple([1, 2, 3])
	(1, 2, 3)
	>>> tuple('abc')
	('a', 'b', 'c')
	>>> tuple((1, 2, 3))
	(1, 2, 3)
	
**基本元组操作**  
序列的一些基本操作，略

**元组的意义**  
通常可以使用元组的地方都可以用列表代替，但是以下两种情况下，元组不可替代：

* 元组在映射中当键使用（键是不可更改的）
* 元组作为很多内建函数和方法的返回值存在



***
## 使用字符串

#### 基本字符串操作
所有标准序列操作（索引、分片、乘法、判断成员资格、求长度、取最小值和最大值）对字符串同样适用，需要注意的是，字符串是不可变的（immutable），因此，一些改变序列的操作在字符串上是不可用的。

#### 字符串格式化：精简版
字符串格式化使用`字符串格式化操作符`（即百分比`%`）来实现，在`%`的左侧放置一个字符串（格式化字符串），右侧放置希望格式化的值，如：

	>>> print "Hello, %s" % "World!"
	Hello, world!
	>>> print "Hello, %s. %s enough for ya?" % ("world", "Hot")
	Hello, world. Hot enough for ya?
	
> `%`右侧的值可以使用一个字符串或者数字，也可以使用多个值的元组或者字典；当使用元组或者字典时，可以格式化多个值。

格式化字符串的`%s`部分称为`转换说明符`（conversion specifier），标记了需要插入转换值的位置。`s`表示值会被格式化为字符串（如果不是字符串，则会用`str`将其转换为字符串）。

如果要格式化实数（浮点数），可以使用`f`说明符，同时提供需要的精度：小数点后面加上希望保留的小数位数，如：

	>>> from math import pi
	>>> print "Pi with three decimals: %.3f" % pi
	Pi with three decimals: 3.142
	
**字符串模版**  
`string`模块提供了另一种格式化值的方法：模版字符串，它的工作方式类似于shell中的变量替换，`substitute`这个模板方法用关键字参数foo替换字符串中的$foo，如：

	>>> from string import Template
	>>> s= Template('$x, glorious $x!')
	>>> s.substitute(x='slurm')
	'slurm, glorious slurm!'
	
> 可以使用$$插入美元符号。

除关键字参数外，还可以使用字典提供值/名称对，如：

	>>> s = Template('A $thing must never $action.')
	>>> s.substitute({'thing': 'gentleman', 'action':'show his socks'})
	'A gentleman must never show his socks.'
	
> `substitute`在缺少值或者不正确使用`$`时会出错，此时可以用`safe_substitute`来代替。

#### 格式化字符串：完整版
如果格式化操作符的右操作数是元组的话，则其中的每个元素都会被单独格式化，每个值都需要一个对应的转换说明符。

需要注意的是，作为转换表达式一部分的元组，必须用括号括起来，如：

	>>> '%s plus %s equals %s' % (1, 1, 2)
	'1 plus 1 equals 2'
	>>> '%s plus %s equals %s' % 1, 1, 2
	Traceback (most recent call last):
	…
	TypeError: not enough arguments for format string
	
转换说明符包含以下部分：

1. `%`字符：标记转换说明符的开始
2. 转换标志（可选）:`-`表示左对齐；`+`表示在转换值之前要加上正负号；` `（空白字符）表示正数之前保留空格；`0`表示若转换值位数不够，则用0填充。
3. 最小字段宽度（可选）：转换后的字符串的最小长度；如果是`*`，则宽度从右侧获取
4. 点(`.`)后跟精度值（可选）：如果转换的是实数，精度值表示出现在小数点后的位数；如果转换的是字符串，那么该数字就表示字符串的最大宽度；如果是`*`，那么精度从右侧获取
5. 转换类型：如下

<pre>
	d, i			带符号的十进制整数
	o				不带符号的八进制
	u				不带符号的十进制
	x				不带符号的十六进制（小写）
	X				不带符号的十六进制（大写）
	e				科学计数法表示的浮点数（小写）
	E				科学计数法表示的浮点数（大写）
	f, F			十进制浮点数
	g				如果指数大于－4或者小于精度值则和e相同，其他情况与f相同
	G				如果指数大于－4或者小于精度值则与E相同，其他情况与F相同
	C				单字符（接受整数或者单字符字符串）
	r				字符串（使用repr转换任意Python对象）
	s				字符串（使用str转换任意Python对象）
</pre>

**简单转换**  
简单的转换只需要写出转换类型，如：

	>>> 'Price of eggs: $%d' % 42
	'Price of eggs: %42'
	>>> 'Hexadecimal price of eggs: %x' % 42
	'Hexadecimal price of eggs: 2a'
	
**字段宽度和精度**  
例：

	>>> '%10f' % pi			# 字段宽 10
	'  3.141593'
	>>> '%10.2f' % pi		# 字段宽 10 精度 2
	'      3.14'
	>>> '%.2f' % pi			# 精度 2
	'3.14'
	>>> '%.5s' % 'Guido van Rossum'
	'Guido'
	>>> '%.*s' % (5, 'Guido van Rossum')
	'Guido'
	
**符号、对齐和0填充**  
如：

	>>> '%010.2f' % pi		# 0 填充
	'0000003.14'
	>>> '%-10.2f' % pi		# - 左对齐
	'3.14      '
	>>> print ('% 5d' % 10) + '\n' + ('% 5d' % -10)
	   10
	  -10
	>>> print ('%+5d' % 10) + '\n' + ('%+5d' % -10)
	  +10
	  -10

#### 字符串方法
一些字符串常量：

* `string.digits`：包含数字0～9的字符串
* `string.letters`: 包含所有字母（大写或小写）的字符串
* `string.lowercase`: 包含所有小写字母的字符串
* `string.printable`: 包含所有可打印字符的字符串
* `string.punctuation`: 包含所有标点的字符串
* `string.uppercase`: 包含所有大写字母的字符串

> Python 3.0中，`string.letters`被移除，使用`string.ascii_letters`代替


**`find`**  
`find`方法可以在一个较长的字符串中查找子字符串，返回子字符串所在位置的最左端索引，如果没有找到则返回－1，例如：

	>>> 'With a moo-moo here.'.find('moo')
	7
	>>> title = 'Monty Python's Flying Circus'
	>>> title.find('Monty')
	0
	>>> title.find('Python')
	6
	>>> title.find('2irquss')
	-1
	
`find`方法还可以有可选的起始点和结束点参数，如：

	>>> subject = '$$$ get rich now!!! $$$'
	>>> subject.find('$$$')
	0
	>>> subject.find('$$$', 1)
	20
	>>> subject.find('!!!', 0, 16)
	-1
	
**`join`**  
`join`方法是`split`方法的逆方法，如：

	>>> seq = [1, 2, 3, 4, 5]
	>>> sep = '+'
	>>> sep.join(seq)		# 连接数字列表
	Traceback (most recent call last):
	…
	TypeError: sequence item 0: expected string. int found
	>>> seq = ['1', '2', '3', '4', '5']
	>>> sep.join(seq)		# 连接字符串列表
	'1+2+3+4+5'
	>>> dirs = '', 'usr', 'bin', 'env'
	>>> '/'.join(dirs)
	'/usr/bin/env'
	
**`lower`**  
`lower`方法返回字符串的小写字母版本，如：

	>>> 'Trondheim Hammer Dance'.lower()
	'trondheim hammer dance'
	
`title`方法，会将字符串转换为标题－即所有单词首字母大写，其他字母小写，如：

	>>> 'that's all folks'.title()
	'That'S All Folks'
	
`string`模块的`capwords`函数，如：

	>>> import string
	>>> string.capwords("that's all. folks")
	"That's All. Folks"
	
**`replace`**  
`replace`方法返回某字符串的所有匹配项都被替换之后的字符串，如：

	>>> 'This is a test'.replace('is', 'eez')
	'Theez eez a test'
	
**`split`**  
`split`方法是`join`的逆方法，用来将字符串分割成序列，如：

	>>> '1+2+3+4+5'.split('+')
	['1', '2', '3', '4', '5']
	>>> '/usr/bin/env'.split('/')
	['', 'usr', 'bin', 'env']
	>>> 'using the default'.split()
	['using', 'the', 'default']
	
> 如果`split`方法不提供任何分隔符，会把所有空格作为分隔符（空格、制表符、换行等）

**`strip`**  
`strip`方法用于去除字符串两侧（不包括内部）的空格，如：

	>>> '    internal whitespace is kept   '.strip()
	'internal whitespace is kept'
	>>>'**** SPAM * for * everyone!!! ****'.strip(' *!')		# 去除指定的字符，本例是' '、'*'和'!'
	'SPAM * for * everyone'
	
**`translate`**  
`translate`方法和`replace`方法一样，可以替换字符串中的某些部分；`translate`方法只处理单个字符，优势在于可以同时进行多个替换。

在使用`translate`之前，需要先完成一张转换表，转换表是某字符替换某字符的对应关系，可以使用`string`模块的`maketrans`函数创建，`maketrans`接受2个等长的字符串，表示第一个字符串中的每个字符都用第二个字符串中对应位置的字符替换。

例：

	>>> from string import maketrans
	>>> table = maketrans('cs', 'kz')
	>>> len(table)		# 转换表是包含256个字符的字符串
	256
	>>> maketrans('', '')[97:123]
	'abcdefghijklmnopqrstuvwxyz'
	
创建转换表之后，可以将它作为`translate`方法的参数，同时`translate`还有一个可选的参数，用来指定需要删除的字符，如：

	>>> 'this is an incredible test'.translate(table)
	'thiz iz an inkredible tezt'
	>>> 'this is an incredible test'.translate(table, ' ')
	'thizizaninkredibletezt'

	

***
## 字典
字典（`dict`）是一种通过名字引用值的数据结构，这种结构称为映射（mapping），其中名字称为字典的键；字典是Python中唯一的内建映射类型。

#### 字典的键
字典的键可以是数字、字符甚至元组，字典的键必须是不可更改的（immutable）。  


#### 创建字典
字典可以通过多种不同的方式来创建，如：

**直接定义**  
定义空字典d和字典phonebook

    d = {}
    phonebook = { 'Snipper': '1234', 'Anti Magic': ' 5678' }

**用类型`dict`，通过其他映射或者（键，值）序列得到**  

    d = dict([('name', '2x'), ('gender', 'not sure')])
    d = dict(name='Tom', age='18')			# 也通过关键参数来构造

**使用字典方法`fromkeys`来构造，默认值为`None`**  

    d = {}.fromkeys(['name', 'age'])			# d = {'name': None, 'age':None}
    d = dict.fromkeys(['name', 'age'])
    d = dict.fromkeys(['name', 'age'], 'unknown')    # 以'unknown'作为默认值

#### 字典操作
**基本操作**  
（对于字典d）  

    len(d)				# 键值对的个数（字典长度）  
    d[k]				# 返回键k对应的值（如果k不在字典中，KeyError异常）  
    d[k] = v			# 将值v关联到键k上  
    del d[k]			# 删除键位k的项（如果k不在字典中，KeyError异常）  
    k in d				# 检查k是否为d的一个键

**格式化字符串**  
格式：转换说明符`%`后面，加上键（用括号括起来），再跟上说明符（s、d等）

    phonebook = { 'Tom': '1234', 'Bob': '5678' }
    "Tom's phone number is %(Tom)s." % phonebook

参阅`string`的[Template](http://docs.python.org/2/library/string.html#template-strings)

**字典方法**  

    clear			# 清除所有项，原地操作，无返回  
    copy			# 返回具有相同键值对的新字典（浅拷贝（shallow copy））
    fromkeys		# 建立新字典，参阅创建字典部分
    get				# 更宽松的访问字典项的方法，当键不存在时，返回给定的默认值，或者None，如
    	phonebook.get('Alice')    # None
     	phonebook.get('Alice', 'No phone')		# 'No phone'
    has_key			# 检查键是否在字典里
    items			# 得到一个（键，值）列表，无序
    keys			# 字典键的列表
    pop				# 获取给定键的值，并将该项从列表中移除
    popitem			# 随机移除一个项
    setdefault		# 类似于get，当键不存在时，设置该键，值为给定值或者默认的None
    update			# 用另外一个字典来更新此字典；对于不存在的键，添加；已经存在的键，覆盖
    values			# 类似于keys，返回字典值的列表

####深拷贝（`deep copy`）

    from copy import deepcopy
    dc = deepcopy(d)




---
## 条件、循环和列表推导式

#### `print`和`import`
**使用逗号输出**  
`print`可以打印多个表达式，表达式之间用逗号分隔开

    >>> print "Age:", 42			# 多个值并不会构成元组
    Age: 42
    >>> print ("Age:", 42)
    ('Age:', 42)

**导入模块或者函数**  
导入模块可以使用以下几种方式

    import somemodule
    from somemodule import somefunction
    from somemodule import somef, anotherf, yetanotherf
    from somemodule import *        # 只有想导入模块所有功能时，才使用该法

如果两个模块都有同一函数（open函数），则

    import module1
    import module2
    module1.open(...)
    module2.open(...)				# 使用'import 模块'时，用模块.函数调用
    import math as mod1				# 给模块起别名
    mod1.sqrt(4)
    from math import sqrt as fun1	# 给模块中的函数起别名
    fun1(4)


#### 赋值魔法

**序列解包**   
Python中，多个赋值操作可以一次完成，如：

    x, y, z = 1, 2, 3		#  x、y、z分别赋值为1、2、3
    x, y = y, z				# 交换x、y的值

当函数或者方法返回元组时，序列解包非常好用，如在字典调用`popitem`时：

    key, value = d.popitem()

序列解包要求所解包的元素数量必须与放置在赋值符号`＝`左边的变量数量一致，否则在赋值时引起异常（`ValueError`）。

Python 3.0中，解包可以像函数的参数列表一样使用`*`号运算符，如：

    a, b, rest* = [1, 2, 3, 4]        # rest = [3, 4]

**链式赋值**  
Python里可以用链式赋值将一个值赋值给多个变量，如：

    x = y = somefunction()

**增量赋值**  

    x = 2
    x += 1
    x * = 2
    fnord = 'foo'
    fnord += 'bar'            # 增量赋值不仅用于数值，其他适用二元运算符的类型也可以
    fnord *= 2


#### 语句块
语句块是在条件为真（条件语句）时执行或者执行多次（循环语句）的一组语句，在代码前来缩进即可创建语句块，见如下伪代码：

    this is a line
    this is another line:				# : 标识语句块的开始
        this is another block			# 推荐用空格缩进，一般4个空格
        continue the same block			# 每一个语句缩进量一样
        last line of the block			# 语句块结束
    phew. there we escaped the inner block


#### 条件和条件语句

**布尔值**  
真值也叫布尔值，根据在真值上做过大量研究的`George Boole`命名。

Python中所有值都能被解释成布尔值，“标准”的布尔值为`True`与`False`；事实上`True`与`False`只是1和0的一种“华丽”的说法而已。

标准值`False`和`None`、所有类型的数字0、空序列（字符串、元组和列表等）和空字典都为假

    False None 0 "" () [] {}		# 假（False）

bool函数可以用来转换其他值为True或者False，事实上Python会自动转换。

**`if`语句和`else`子句**  
`if`语句可以实现条件执行，如果条件为真，则后面的语句块会执行；否则，执行`else`子句，如：

    name = raw_input('What is your name?')
    if name.endswith('Gumby'):
        print 'Hello, Mr. Gumby'
    else:
        print 'Hello, stranger'

**`elif`子句**  
如果需要检查多个条件，可以使用`elif`子句，它是”else if“的简写。

**更复杂的条件**  

    x == y					# 相等运算符
    x < y
    x > y
    x >= y
    x <= y
    x != y
    x <> y					# 同 x != y，不推荐这样写
    x < y < z				# Python中比较运算符和赋值运算一样是可以链接的
    x is y					# 同一性操作符，判断同一性而非相等性
    x is not y
    x in y					# 成员资格运算符
    x not in y
    'alpha' < 'beta'		# True 字符串比较，按照字母顺序排列
    [1, 2] < [2, 1]			# True 序列比较
    
**布尔运算符**  
`and`，`or`和`not`运算符就是所谓的布尔运算符，3个运算符可以随意结合真值。

布尔运算符是短路逻辑，如：

    name = raw_input("Enter your name: ") or 'unknown'    # 如果raw_input返回真，则它的值会赋给name，否则'unknown'赋值给name

**断言**  
如果需要确保程序在某个条件一定为真时才能正常工作，可以使用断言（`assert`）语句。

    age = 10
    assert 0 < age < 100
    assert 0 < age < 100, 'The age must be realistic'	# 条件后添加字符串，用来解释断言


#### 循环
常见的循环有`while`循环和`for`循环

**`while`循环**  

    name = ''
    while not name or name.isspace():
        name = raw_input('Please enter your name: ')
    print 'Hello, %s!' % name

**`for`循环**  

    for number in range(1, 101):		# range工作方式类似于分片，包含下限，不包含上限
        print number
    d = {'x': 1, 'y': 2, 'z': 3}
    for key, value in d.items():		# for循环遍历字典
        print key, 'corresponds to', value

`xrange`的行为类似于`range`，区别在于`range`函数一次创建整个序列，而`xrange`一次只创建一个数。Python 3.0中，`range`会被转换为`xrange`风格的函数。


#### 一些迭代工具
**并行迭代**  
内建的`zip`函数可以用来进行并行迭代，可以把两个序列“压缩”在一起，获得一个元组的列表

    names = ['Alice', 'Bob', 'Tom']
    ages = [12, 14, 11]
    zip(names, ages)					# [('Alice', 12), ('Bob', 14), ('Tom', 11)]
    for name, age in zip(names, ages):
        print name, 'is', age, 'years old'

`zip`函数可以作用于任意多的序列；`zip`也可以应付不等长的序列，当最短的序列“用完”的时候就停止

    >>> zip(range(5), xrange(100000))
    [(0, 0), (1, 1), (2, 2), (3, 3), (4, 4)]

**编号迭代**  
有时候想要迭代序列中的对象，还想同时获得当前对象的索引，这个时候可以用内建的`enumerate`函数来处理，如：

    for index, string in enumerate(strings):
        if 'xxx' in string:
            strings[index] = 'chagned'

**翻转与排序迭代**  
`reversed`和`sorted`，类似于列表的`reverse`和`sort`，区别在于，前2者作用于任何序列或者可迭代对象上，不是原地修改，返回翻转或者排序后的版本，如：

    >>> sorted([4, 3, 6, 8, 3])			# sorted 返回列表
    [3, 3, 4, 6, 8]
    >>> ''.join(reversed('Hello'))		# reversed 返回可迭代对象，可以在for和join中使用
    'olleH'
    >>> list(reversed('Hello'))[0]		# 使用索引、分片或者调用list方法时，需要用list将reversed返回的迭代对象转化为列表
    'o'

**跳出循环**  

    break				# 跳出循环
    continue			# 结束当前迭代，跳到下一轮循环的开始

**循环的`else`子句**  

    for n in xrange(10):
        print n
    else:				# 当循环中没有调用到break的时候，循环结束时会调用else子句
        print "end"


#### 列表推导式
列表推导式（list comprehension）是利用其他列表创建新列表的一种方式，它的工作方式类似于`for`循环，如：

    >>> [x*x for x in range(10)]
    [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
    >>> [x*x for x in range(10) if x % 3 == 0]			# 带条件判断
    [0, 9, 36, 81]
    >>> [(x, y) for x in range(2) for y in range(2)]	# 多个for语句一起使用
    [(0, 0), (0, 1), (1, 0), (1, 1)]


#### 其他 

**`pass`语句**  
在Python中空代码块是非法的，这时可以使用`pass`语句，表示什么也没有发生。

**`del`语句**  
del语句可以用来删除对象，不仅删除对像的引用，还删除对象本身，如

    x = 1
    del x			# 删除x的引用，并且删除x
    print x			# NameError: name 'x' is not defined

**`exec`**  
`exec`用来执行一个字符串，略

**`eval`**  
`eval`是一个类似于`exec`的内建函数，其会计算Python表达式并且返回结果值，详略。




***
## 函数

#### 创建函数  
函数会执行某种行为并且返回一个值。  
内建的`callable`函数可以用来判断一个变量是否是函数（即是否可被调用），如：

    import math
    callable(math.sqrt)    # True    Python 3.0中，callable不可再用，需要使用hasattr表达式
    hasattr(math.sqrt, '__call__')    # True

**定义函数**  
函数使用`def`语句来定义，如：

    def hello(name):
        return 'Hello, ' + name + '!'    # return 语句用来从函数返回值

**文档字符串**  
为了便于理解函数，除了可以使用注释（以`＃`开头）外，还可以在函数开头直接写下字符串，作为函数的一部分进行存储，此处的字符串叫*文档字符串*，如：

    def square(x):
        'Calculates the square of the number x.'        # 可使用多行字符串
        return x*x

    square.__doc__        # __doc__是函数的一个属性，用来访问文档字符串

**函数的返回值**  
Python中所有的函数都有返回值，`return`语句用来从函数中返回值；没有return语句或者return语句后面没有跟任何值的函数返回None。


#### 函数的参数
`def`语句中函数名后面（括号内）的变量通常叫做函数的形式参数，调用函数时提供的值是实际参数，或者称为参数；通常形式参数和实际参数不需要太严格的区分开来，可统一称为参数。

函数的局部名称（包括参数）并不和函数外面的名称冲突，因为他们有不同的作用域。

**关键字参数和默认值**  
调用函数时，参数通常按照定义时给定的顺序传值，这个时候参数的位置很重要，因此我们也称之为位置参数。  
当参数很多的时候，很难记住参数的位置；为了使事情更简单，可以提供参数的名字，即使用关键字参数，如：

    def hello(greeting, name):
        print '%s, %s!' % (greeting, name)
    hello('Hello', 'world')                            # 使用位置参数
    hello(greeting='Hello', name='world')    # 使用关键字参数
    hello(name='world', greeting='Hello')    # 关键字参数顺序不影响函数的调用

**收集参数**
有些时候，函数需要处理任意数量的参数，这个时候可以使用收集参数。收集参数有2种类型，一种在参数名之前加`*`号，一种加`**`号。

1、参数名之前加`*`号，表示收集其余的位置参数，将他们放在一个元组里，如：

    def print_params(*params):
        print params
    print_params('test')		# ('test',)    长度为1的元组
    print_params(1, 2, 3)		# (1, 2, 3)
    def print_params_2(title, *params):    # 收集参数和普通参数联合使用
        print title
        print params

2、参数名之前加`**`号，表示收集其余的关键字参数，将他们放在一个字典里，如：

    def print_params_3(**params):
        print params
    print_params_3(x=1,y=2,z=3)    # {'z': 3, 'x': 1, 'y': 2}


**反转收集过程**  
`*`和`**`不仅可以将参数收集为元组和字典，还可以执行相反的操作，即将元组和字典分配成位置参数和关键字参数，如：

    def add(x, y):
        return x + y
    params = (1, 2)
    add(*params)        # *在调用时使用，将元组分配成位置参数使用


#### 作用域
在执行语句x=1之后，变量x引用到值1，就好像字典一样，键引用值。变量和所对应的值用的是一个“不可见”的字典，内建函数`vars`可以返回这个字典，如：

    x = 1
    scope = vars()
    scope['x']				# 1
    scopre['x'] += 1		# 一般来说，vars所返回的字典是不能修改的，其结果未定义
    x						# 2

vars返回的“不可见字典“叫做命名空间，或者作用域；除了全局作用域外，每个函数调用都会创建一个新的作用域。

**局部变量和全局变量**  

 - 函数内的变量（包括形式参数）被称为局部变量，对应的则是不在任何函数内的全局变量；  
 - 局部变量不会影响全局变量；   
 - 函数想读取全局变量的值，可以直接在函数内部使用全局变量；慎重使用，通常会引起一些错误
 - 如果局部变量或者参数名与全局变量相同，则不能直接访问该全局变量，全局变量被屏蔽；  
 - 在函数内部将值赋给一个变量，它会自动变成局部变量；  
 - 可在函数内部用`global`来声明变量，告诉Python这是一个全局变量；

global声明全局变量，如：

    x = 1
    def change_global():
        global x
        x = x + 1

当全局变量被局部变量屏蔽时，可以使用`globals`函数来间接访问全局变量，如：

    def combine(parameter):
        print parameter + globals()['parameter']    # globals类似于vars，返回全局变量的字典
    parameter = 'berry'
    combine('hello')        # helloberry


#### 一些函数

    map(func, seq [, seq, ...])			# 对序列中的每个元素应用函数
    filter(func, seq)					# 返回其函数为真的元素的列表
    reduce(func, seq [, initial])		# 等同于func(func(func(seq[0], seq[1]), seq[2]), ...)
    sum(seq)							# 返回seq中所有元素的和
    apply(func[, args[, kwargs]])		# 调用函数，可以提供参数

示例：

    map(str, range(5))			# 等同于[str(i) for i in range(5)]

    def func(x):
        return x.isalnum()
    seq = ['foo', 'x41', '*']
    filter(func, seq)    # ['foo', 'x41']    等同于 [x for x in seq if x.isalnum()]
    filter(lambda x: x.isalnum(), seq)    # lambda实现    匿名函数

    numbers = [1, 2, 3, 4]
    reduce(lambda x, y: x + y, numbers)    # 10
    




***
## 类和对象
#### 对象的魔力

面向对象程序设计中的术语对象（object）通常是由数据和操作这些数据的方法组成（有点类似于ADT），面向对象有3个重要的优点：

> 封装（Encapsulation）：对外部世界隐藏对象的工作细节
> 
> 继承（Inheritance）：以普通的类为基础，构建专门的类对象
> 
> 多态（Polymorphism）：不同类的对象可以使用同样的操作

此处省略1000字……

#### 类和类型
类，可以看成是某一个种类或者某一个类型，而对象则是属于某一个类的，称为类的一个实例（instance）。

**创建类**  

    __metaclass__ = type		# 确定使用新式类
    
    class Person:				# Person是类名，习惯上使用单数名字，首字母大写； class语句会创建自己的命名空间
        
        def setName(self, name):	# self是对象自身的引用
            self.name = name		# self.name中的name是类的属性（attribute）

        def getName(self):
            return self.name

        def greet(self):
            print "Hello, world! I'm %s." % self.name

    # TEST
    foo = Person()
    foo.setName('Tom')			# foo将自己作为第一个参数传入函数，self命名很形象啊^_^
    foo.name					# 'Tom' 默认情况下，Python可以从外部访问一个对象的属性

> 新式类是在2.2版本中引进来，目的主要是为了统一类（class）和类型（type）；
>
> 新式类的语法中，需要在模块或者脚本开始的地方放置赋值语句`__metaclass__ = type`或者继承新式类（比如object）

**属性和方法**
方法和函数的区别主要在于方法的第一个参数是`self`，方法将第一个参数`self`绑定到所属的实例上，因此在调用的时候不需要提供。

在默认情况下，可以从外部访问一个对象的属性的；Python不直接支持私有方式，一个小技巧是，在属性或者方法的名字前加上双下划线（`__`）之后，他们不能从对象外部直接访问；同时使用`from module import *`时，也不会被导入。


**类的命名空间**  
其实Python类的定义就是一个执行代码块，如：

    >>>class C:
                print 'Class C being defined...'

    Class C being defined...    # 输出的信息

class语句创建特殊的类命名空间，该命名空间可由类所有成员访问。


**子类和超类**  
当一个对象所属的类是另外一个类的子集时，称前者为后者的子类（`subclass`），后者为前者的超类（`superclass`），例如“百灵鸟”类，它是“鸟”类的一个子集，因此说“百灵鸟”类是“鸟”类的一个子类，“鸟”类是“百灵鸟”类的一个超类。

通常我们称子类继承超类，如：

    class Filter:
        def init(self):
            self.blocked = []
        def filter(self, sequence):
            return [x for x in sequence if x not in self.blocked]

    class SPAMFilter(Filter):			# SPAMFilter是Filter的子类，定义类时，超类写在圆括号内
        def init(self):					# 重写超类中的init方法
            self.blocked = ['SPAM']


**`issubclass`函数**  
`issubclass`函数用来产看一个类是否是另外一个类的子类，如：

    >>> issubclass(SPAMFilter, Filter)		# True
    >>> issubclass(Filter, SPAMFilter)		# False
    >>> SPAMFilter.__bases__				# 特殊的属性__bases__，访问超类（返回一个元组）
    (<class __main__.Filter at 0x171e40>, )
    >>> Filter.__bases__
    ()

**`isinstance`函数**  
`isinstance`函数可以用来检查一个对象是否是一个类的实例，如：

    >>> s = SPAMFilter()
    >>> isinstance(s, SPAMFilter)		# True
    >>> isinstance(s, str)				# False
    
    >>> s.__class__						# 查看一个对象属于哪个类，可用__class__属性；对于使用__metaclass__ = type或者从object继承方式定义的新式类，可以用type(s)来查看实例的类

使用isinstance并不是一个好习惯，尽可能使用多态。

**多个超类**  
Python中，一个子类的超类可以有多个，即可以多重继承，如：

    class Calculator:
        def calculate(self, expression):
            self.value = eval(expression)

    class Talker:
        def talk(self):
            print 'Hi, my value is', self.value

    class TalkingCalculator(Calculator, Talker):
        pass                # 子类自己不做事，所有行为都从超类继承

使用多重继承时，如果一个方法从多个超类中继承，则先继承的超类中的那个方法会重写后继承类中方法。

**`hasattr`函数**  
`hasattr`函数可以用来检查类或者对象中是否存在某个属性或者方法，如：

    tc = TalkingCalculator()
    hasattr(tc, 'talk')						# True
    hasattr(TalkingCalculator, 'talk')		# True
    hasattr(tc, 'invalid')					# False

    callcable(getattr(tc, 'talk', None)	# True 检查talk是否可调用；getattr与字典的get用法很相似
    hasattr(tc.talk, '__call__')			# True
    



---
## 异常
#### 什么是异常
Python用异常对象（exception object）来表示异常；程序遇到错误，会引发异常，如果异常没有捕获或者处理，程序会用回溯（Traceback，是一种错误信息）终止程序，如：

	>>> 1 / 0
	Traceback (most recent call last):
		File "<pyshell#0>", line 1, in <module>
			1 / 0
	ZeroDivisionError: integer division or modulo by zero

每个异常都是一个类的实例，这些实例可以被引发，并且可以被捕获，从而可以对错误进行处理。

#### 按自己的方式出错

**`raise`语句**  
可以使用一个类或者实例参数调用`raise`语句来引发一个异常，使用类时，程序会自动创建实例，如：

	>>> raise Exception
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	Exception
	>>> raise Exception('error message')
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	Exception: error message

Python内建的异常很多，可以在exceptions模块中找到，可以使用dir函数列出模块内容，如：

    >>> import exceptions
    >>> dir(exceptions)
    'ArithmeticError', 'AssertionError', ...]

常见的异常有：
<pre>
Exception					所有异常的基类
AttributeError				特殊引用或赋值失败时引发
IOError						试图打开不存在的文件（或者其他情况）时引发
IndexError					使用序列中不存在的索引时引发
KeyError					使用映射中不存在的键时引发
NameError					找不到名字（变量）时引发
SyntaxError					语法错误时引发
TypeError					内建操作或者函数应用于错误类型时引发
ValueError					对象使用不合适时引发
ZeroDivisionError			除法或者模运算中第二个参数为0时引发
</pre>

**自定义异常**    
Python中可以根据需要创建自己的异常，即创建一个异常类；异常类需要直接或者间接从Exception类继承，如：

    class SomeCustomException(Exception): pass    # 啥都不做就可以了

#### 捕获异常
异常可以通过`try/except`来捕获、处理，如：

    try:
        print 1 / 0
    except ZeroDivisionError:		# 捕捉ZeroDivisionError异常
        print 'divide by zero error'

没有被捕捉的异常，会“传播”到调用它的函数中，如果那里依旧没有处理，则异常会依次往上“浮”，直至到达程序最顶层，终止程序。

**重新抛出异常**  
有时候捕捉到异常，但是又想重新抛出，则可以使用不带参数的raise语句，如：

    try:
        print 1 / 0
    except ZeroDivisionError:
        raise                        # 直接抛出


#### 处理多个异常
**多个except子句**  
有时候，一段程序代码可能会引起多个异常，此时可以使用多个except子句来分别捕获，如：

    try:
        x = input('Enter the first number: ')
        y = input('Enter the second number: ')
        print x / y
    except ZeroDivisionError:
        print 'The second number couldn't be zero!'
    except TypeError:
        print 'That wan't a number, was it?'

**用一个快捕获多个异常**  
如果需要用一个快来捕获多个异常，可以将异常作为元组列出，如：

    <!-- lang: python -->
    try:
        x = input('Enter the first number: ')
        y = input('Enter the second number: ')
        print x / y
    except (ZeroDivisionError, TypeError, NameError):    # 这里的元组需要显式的使用括号，否则得不到想要的结果
        print 'Your numbers were bogus...'

#### 捕获对象
可以在except子句中捕获异常对象本身，此时except子句需要一个额外的参数，如：

    try:
        x = input('Enter the first number: ')
        y = input('Enter the second number: ')
        print x / y
    except (ZeroDivisionError, TypeError) e:    # Python 3.0中，语法为 except (ZeroDivisionError, TypeError) as e
        print e

#### 全捕获
在try/except的except子句没有参数的时候，会捕获所有的异常，如：

    try:
        print 1 / 0
    except:            # 捕获try子句中的所有异常，是一种比较危险的做法；它也会捕获Ctrl-C或者sys.exit等操作，此时用 except Exception, e会更好一些
        print 'error'

#### 万事大吉
可以像条件和循环语句一样，给try/except语句加上else子句；当没有异常发生时，执行else子句，如：

    try:
        print 'A simple task'
    except Exception, e:
        print 'What? Something went wrong!'
    else:
        print 'Ah, I am coming'                    # 执行

#### 最后
`finally`子句可以处理异常的清理工作，和try联合使用，如：

    x = None
    try:
        x = 1 / 0
    finally:					# 程序奔溃之前，对变量x的清理就完成了
        print 'Cleaning up...'
        del x

`finally`子句肯定会执行，所以通常可以用来关闭文件或者网络套接字。





---
## 魔法方法、属性和迭代器

Python中，有一些方法名前后都带有两个下划线（比如`__future__`)，这种拼写表示名字有特殊含义，Python中称之为*魔法方法*。

*一般地，程序中自己定义的方法不要使用这种格式。*

> Python 3.0中没有“旧式”类，也不需要显示地子类化object或者将元类设置为type，所有的类都会隐式地成为object的子类。

#### 构造方法
当一个对象被创建之后，会立刻调用对象的构造方法，Python中的构造方法是魔法方法`__init __`，如：

	class FooBar:
		def __init__(self):		# 第一个参数是self，之后也可有其他参数
			self.somevar = 42

	f = FooBar()
	f.somevar				# 42

> Python中还有一个`__del__`魔法方法，也就是析构函数，它在对象要被垃圾回收之前调用；但是因为具体的调用时间不可知，建议尽量避免是用`__del__`函数


#### 重写一般方法和特殊的构造方法
重写是继承机制的一个重要内容，在构造方法里特别重要；大多数子类的构造方法中不仅要有自己的初始化代码，还要有其超类的初始化代码，如：

	class Bird:
		def __init__(self):
			self.hungry = True

	class SongBird(Bird):
		def __init__(self):
			self.sound = 'haha!'

	b = Bird()
	b.hungry			# True
	sb = SondBird()
	sb.sound			# 'haha!'
	sb.hungry			# AttributeError: SondBird instance has no attribute 'hungry'

因为在SondBird里没有调用超类的构造方法，所以SondBird中没有属性hungry。

因此，SondBird的构造方法中必须调用其超类的构造方法以确保进行正确的初始化；有两种方法来达到这个目的：其一调用超类构造函数的为绑定版本，其一使用`super`函数。

**调用未绑定的超类构造方法**  
> 这是比较旧的方法，目前基本上都使用`super`函数（Python 3.0更是如此）。

Python中，在调用实例的方法时，该方法的`self`参数会自动banding到实例上（称为绑定方法）；但是如果直接调用类的方法（比如，`Bird.__init__`），那么就没有实例会被绑定，这时可以提供需要的`self`参数，这样的方法称为为绑定方法。

修改上述的SondBird的构造方法（只显示`__init__`方法，其他忽略），如：

	def __init__(self): 
		Bird.__init__(self)		# 通过调用超类的构造方法，实现超类属性的初始化
		self.sound = 'haha!'

**使用`super`函数**  
`super`函数只能在新式类中使用。 当前的类和对象可以作为`super`函数的参数使用，`super`调用会返回一个super对象，该对象的任何方法都是超类的方法。

使用`super`函数，上述SondBird的构造方法可以写为：

	def __init__(self):
		super(SondBird, self).__init__()
		self.sound = 'haha!'

> `super`函数还有一个比较智能的优点，如果子类继承多个超类，也只需要使用一次`super`函数（前提是所有的超类构造方法都使用了`super`函数）

#### 成员访问

可以通过一些魔法方法的集合，创建行为类似于序列或映射的对象。

> 规则（protocol）是管理某种形式的行为的规则，与接口（interface）类似，规则说明了应该实现哪些方法和这些方法应该做什么。Python中的多态是基于对象的行为的（而不是基于祖先），只是简单的要求它遵守几个给定的规则。

**基本的序列和映射规则**  
序列和映射是对象的集合，为了实现他们的基本行为（规则），对于可变对象，需要实现2个魔法方法；不可变对象，4个：

* `__len__(self)`：返回集合中所含项目的数量
* `__getitem__(self, key)`：返回与所给键对应的值；对于序列来说，键就是索引；对于映射来说，可以使用任意的键
* `__setitem__(self, key, value)`：存储和key对应的value
* `__delitem__(self, key)`：删除和元素相关的键，该方法在对一部分对象使用`del`语句时被调用

对于上述的魔法方法而言，要求：

* 序列的键如果是负数，则表示从末尾开始计数，即`x[-n]`和`x[len[x]-n]`是一样的；
* 如果键和类型不合适，会引发IndexError异常（如使用字符串作为序列的键时）；
* 如果序列的键类型正确，但是超出了范围，应该引发一个IndexError异常。

以创建一个无穷序列为例：

	def checkIndex(key):
		'''此处键是一个非负整数。如果不是整数，引发TypeError异常，如果是负数，引发IndexError异常'''

		if not isinstance(key, int(long, int)): raise TypeError
		if key < 0: raise IndexError

	class ArithmeticSequence:
		def __init__(self, start = 0, step = 1):
			self.start = start
			self.step = step
			self.changed = {}	# 用户修改值的字典

		def __getitem__(self, key):
			checkIndex(key)

			try: return self.changed[key]
			except KeyError:
				return self.start + key * self.step

		def __setitem__(self, key, value):
			checkIndex(key)

			self.changed[key] = value
	
	# 调用
	>>> s = ArithmeticSequence(1, 2)
	>>> s[4]					# 调用__getitem__
	9
	>>> s[4] = 2				# 调用__setitem__
	>>> s[4]
	2
	>>> s[5]
	11
	>>> del s[4]				# 没有实现 __delitem__，报异常
	Traceback (most recent call last)
	...
	AttributeError: ArithmeticSequence instance has no attribute '__delitem__'

> 尽量避免使用`isinstance`函数，因为类或类型检查与Python中多态的目的背道而驰；由于Python的语言规范上明确指出索引必须是整数（包括长整数），所以上面才会用`isinstance`函数。遵守标准是使用类型检查的（很少的）正当理由之一。

**子类化列表、字典和字符串**  
可以通过子类化内建类的方式来实现列表、映射等相似行为的类。

> 当子类化一个内建类时，也就间接地将object子类化，因此该类就自动成为一个新式类，也就意味着可以使用`super`函数这样的特性了。

一个带有访问计数的列表可以实现为：

	class CounterList(list):
		def __init__(self, *args):
			super(CounterList, self).__init__(*args)
			self.counter = 0
		
		def __getitem__(self, index):
			self.counter += 1
			return super(CounterList, self).__getitem__(index)

#### 更多魔力
大多数魔法方法都是为高级用法准备的，如果有兴趣可以参考[魔法方法](http://docs.python.org/2/reference/datamodel.html#special-method-names)

#### 属性
Python中通过访问器（通常说的getter、setter）定义的特性称为属性，属性可以用来隐藏访问器方法，使所有特性看起来一样。

**`property`函数**  
`property`函数创建一个属性，可以用0、1、2、3或者4个参数来调用，它的4个参数分别叫做fget、fset、fdel和doc；如果没有参数，产生的属性既不可读，也不可写；如果用一个参数来调用，产生的属性只读；第三个参数是一个用于删除特性的方法，第四个参数是文档字符串。

在新式类中，推荐使用`property`函数而不是访问器；`property`使用实例：

	__metaclass = type
	class Rectangle:
		def __init__(self):
			self.width = 0
			self.height = 0

		def setSize(self, size):
			self.width, self.height = size

		def getSize(self):
			return self.width, self.height

		size = property(getSize, setSize)

> `property`其实不是真正的函数，它是拥有很多特殊方法的类，涉及的方法是`__get__`、`__set__`、`__delete__`，3个方法一起完成了`property`的工作

**静态方法和类方法**  
静态方法和类方法可通过将对象的方法包装进`staticmethod`类型和`classmethod`类型得到；静态方法的定义中没有`self`参数，能够被类本身直接调用；类方法在定义时需要类似于`self`的`cls`参数，类方法可以被类的对象调用，此时`cls`参数自动被绑定到类。例如：

	__metaclass__ = type
	class MyClass:
		def smeth():
			print 'This is a staic method'
		smeth = staticmethod(smeth)
		
		def cmeth(cls):
			print 'This is a class method of', cls
		cmeth = classmethod(cmeth)
		
	# 调用
	MyClass.smeth()		# 'This is a static method'
	MyClass.cmeth()		# 'This is a class method of …'
	
**`__getattr__`、`__setattr__`和它的朋友们**  
为了在访问特性的时候可以执行代码，需要使用一些魔法方法：

* `__getattribute__`(self, name): 当特性name被访问时自动调用（只在新式类中使用）；
* `__getattr__`(self, name): 当特性name被访问，切对象没有相应的特性时被自动调用；
* `__setattr__`(self, name, value): 当试图给特性name赋值时自动调用；
* `__delattr__`(self, name): 当试图删除特性name时自动掉用。

#### 迭代器
魔法方法`__iter__`是迭代器规则的基础。

**迭代器规则**  
迭代的意思是重复做一件事很多次，目前为止我们在`for`循环中只迭代过序列和字典，事实上，所有实现了`__iter__`方法的对象都是可迭代的。

`__iter__`方法返回一个迭代器（iterator），迭代器就是具有`next`方法的对象；在调用`next`方法时，迭代器会返回它的下一个值；如果没有值可以返回，则引发一个`StopIteration`异常。

> Python 3.0中迭代器规则有些变化，新规则中迭代器对象应该实现`__next__`方法，而不是`next`；并且新的内建函数`next`可以用于访问这个方法，即`next(it)`等同于3.0之前版本中的`it.next()`。

为什么使用迭代器而不是用列表？因为迭代器是在需要的时候才获取一个值，而列表是一次性获取所有值，通常占用内存较多。通常迭代器更通用、更简单、更优雅。

下面的例子中不能使用列表，因为要用的话，列表必须是无限的：

	class Fibs(object):
		def __init__(self):
			self.a = 0
			self.b = 1

		def next(self):
			self.a, self.b = self.b, self.a + self.b
			return self.a

		def __iter__(self):
			return self

此处迭代器实现`__iter__`方法，`__iter__`实际上返回迭代器本身；实现了`__iter__`方法的迭代器可以直接在`for`循环中使用。

> 一个实现了`__iter__`方法的对象是可迭代的，一个实现`next`方法的对象是迭代器。

例：

	fibs = Fibs()
	for f in fibs:			# 打印第一个大于1000的斐波那契数，推出循环
		if f > 1000:
 			print f
			break

内建函数`iter`可以从可迭代对象中获得迭代器，如：

	>>> it = iter([1, 2, 3])
	>>> it.next()
	1
	>>> it.next()
	2


**从迭代器得到序列**  
在大部分能使用序列的情况下，都可以使用迭代器；可以将迭代器和可迭代对象转换为列表，如：

	class TestIterator:
		value = 0

		def next(self):
			self.value += 1
			if self.value > 10:
 				raise StopIteration		# 迭代结束时，引发StopIteration异常
			return self.value

		def __iter__(self):				# 该对象是可迭代的，因此可以转换为列表
			return self
	
	# Test
	>>> ti = TestIterator()
	>>> list(ti)
	[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]


#### 生成器
生成器也叫做简单生成器，是一种用普通的函数语法定义的迭代器，使用生成器可以写出非常优雅的代码。

生成器是一个包含`yield`关键字的函数，当它被调用时，函数体内的代码不会执行，而是返回一个迭代器。每次请求一个值，会执行生成器中的代码，直到遇到一个`yield`或者`return`语句。`yield`语句意味着生成一个值，并在此处冻结，直到下次调用；`return`语句意味着生成器将停止执行。

生成器由两部分组成：生成器的函数和生成器的迭代器。生成器的函数使用`def`语句定义的，包含`yield`语句，生成器的迭代器是这个函数的返回。

**创建生成器** 
创建一个简单的生成器，如：

	def simple_generator():
		yield 1
		
	# Test
	>>> simple_generator
	<function simple_generator at . >
	>>> simple_generator()
	<generator object at . >
 
创建一个生成器，用来展开嵌套列表，如：

	def flatten(nested):
		for sublist in nested:
			for element in sublist:
				yield element
	
	# Test
	>>> nested = [[1,2], [3,4], [5]]
	>>> for num in flatten(nested):
	...		print num
	…
	1
	2
	3
	4
	5
	>>>
	
可以通过在生成器上迭代来使用所有的值。

> 生成器推导式和列表推导式的工作方式类似，区别在于生成器推导式返回的是生成器而不是列表，并且不会立刻进行循环。
> 
> 语法上，生成器推导式使用圆括号，而列表推导式使用中括号。

生成器推导式的一个例子：

	>>> g = ((i+2)**2 for i in range(2, 27))
	>>> g.next()
	16
	
生成器推导式可以在当前的圆括号内直接使用，如：

	sum(i**2 for i in range(10))
	
**递归生成器**  
略

**生成器方法**  
略

**模拟生成器**  
生成器是在Python 2.5中引入的，旧版本的Python中可以使用普通的函数来模拟生成器。

用普通的函数重写无限嵌套的flatten生成器，如：

	def flatten(nested):
		result = []
		try:
			try: nested + ''	# 不迭代字符串等对象
			except TypeError: pass
			else: raise TypeError
			
			# 如果nested不是列表，引发TypeError异常
			for sublist in nested:
				for element in flatten(sublist):
					result.append(element)
		except TypeError:
			result.append(nested)
		
		return result

**八皇后问题**  
略
