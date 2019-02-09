## 基础
1. 每一行都是一个语句，当语句以冒号:结尾时，缩进的语句视为代码块
    - 缩进有利有弊。好处是强迫你写出格式化的代码，但没有规定缩进是几个空格还是Tab。按照约定俗成的管理，应该始终坚持使用**4个空格**的缩进。
    - 缩进的另一个好处是强迫你写出缩进较少的代码，你会倾向于把一段很长的代码拆分成若干函数，从而得到缩进较少的代码。
    - Python程序是**大小写敏感**的
    
## 数据类型和变量
在Python中，能够直接处理的数据类型有以下几种（整数，浮点数，字符串)

1. 整数 
    - Python可以处理任意大小的整数
    - 十六进制用0x前缀和0-9，a-f表示（`0xff00` `0xa5b4c3d2`）
    - ==整数运算永远是精确的==
2. 浮点数
    - 对于很大或很小的浮点数，就必须用科学计数法表示，把10用e替代。1.23x10<sup>9</sup>就是`1.23e9`或者`12.3e8`
3. 字符串
    - 可以用转义字符\来标识`'`或者`"`
    
	    ```
	    'I\'m \"OK\"!'
	    ```
    - r''表示''内部的字符串默认不转义
    
	    ```
	    >>> print('\\\t\\')
	    \       \
	    >>> print(r'\\\t\\')
	    \\\t\\
	    ```
    - 用'''...'''的格式表示多行内容(注意...是提示符，不是代码的一部分)
    
	    ```
	    >>> print('''line1
	    ... line2
	    ... line3''')
	    line1
	    line2
	    line3
	    ```
4. 布尔值
    - 布尔值可以用`and`、`or`和`not`运算
5. 空值
 - 空值是Python里一个特殊的值，用`None`表示
6. 变量
    - python是动态类型语言，同一个变量可以反复赋值，而且可以是不同类型的变量。
    - 创建变量时 python 解析器干了两件事情
        - 在内存中创建了一个`'ABC'`的字符串
        - 在内存中创建了一个名为`a`的变量，并把它指向`'ABC'`
7. 常量


## 字符串和编码
1. 在最新的Python 3版本中，字符串是以Unicode编码的，也就是说，Python的字符串支持多语言
2. 对于**单个字符**的编码，Python提供了ord()函数获取字符的整数表示，chr()函数把编码转换为对应的字符：

	```
	>>> ord('A')
	65
	>>> ord('中')
	20013
	>>> chr(66)
	'B'
	>>> chr(25991)
	'文
	```

3. 由于Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。
4. 以Unicode表示的str通过`encode()`方法可以编码为指定的bytes，`decode()`把`bytes`变成`str`

	```
	# encode
	>>> 'ABC'.encode('ascii')
	b'ABC'
	>>> '中文'.encode('utf-8')
	b'\xe4\xb8\xad\xe6\x96\x87'
	>>> '中文'.encode('ascii')
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
	
	# decode
	>>> b'\xe4\xb8\xad\xff'.decode('utf-8')
	Traceback (most recent call last):
	  ...
	UnicodeDecodeError: 'utf-8' codec can't decode byte 0xff in position 3: invalid start byte
	
	# 可以传入errors='ignore'忽略错误的字节
	b'\xe4\xb8\xad\xff'.decode('utf-8', errors='ignore')
	```
5. 在bytes中，无法显示为ASCII字符的字节，用`\x##`显示
6. `len()`计算`str`包含多少个字符，计算`bytes`包含多少字节数

		```
		# 计算 str
		>>> len('ABC')
		3
		>>> len('中文')
		2
		
		# 计算 bytes
		>>> len(b'ABC')
		3
		>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
		6
		>>> len('中文'.encode('utf-8'))
		6
		```

7. 为了让python解释器按照UTF-8读取
    - 第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；
    - 第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。
    - 申明了UTF-8编码并不意味着你的.py文件就是UTF-8编码的，必须并且要确保文本编辑器正在使用UTF-8 without BOM编码：
    
		```
		#!/usr/bin/env python3
		# -*- coding: utf-8 -*-
		```

8.格式化字符串

- `%`运算符格式化字符串.
    - 有几个`%?`占位符，后面就跟几个变量或者值，顺序对应。
    - 如果只有一个%?，括号可以省略。
    - 用%%来表示一个%
    - 格式化整数和浮点数还可以指定是否补0和整数与小数的位数
   	
	   占位符 | 替换内容
	    ---|---
	    %d | 整数
	    %f | 浮点数
	    %s | 字符串(永远起作用，它会把任何数据类型转换为字符串)
	    %x | 十六进制整数
	   ```
		print('%2d-%02d' % (3, 1)) # 3-01
		print('%.2f' % 3.1415926) # 3.14			```
		
- 字符串的format()方法格式化字符串

	```
	>>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
	'Hello, 小明, 成绩提升了 17.1%'
	```

## list和tuple
1. list是一种有序的集合，可以随时添加和删除其中的元素
	 - `len()`函数获得list元素的个数
	 
		```
		>>> classmates = ['Michael', 'Bob', 'Tracy']
		>>> classmates
		['Michael', 'Bob', 'Tracy']
		>>> len(classmates)
		3
		```
	- 索引访问 `classmates = ['Michael', 'Bob', 'Tracy']`
	
	 - 正整数索引表示从左到右
	 
		    ```
		    >>> classmates[0]
		    'Michael'
		    >>> classmates[1]
		    'Bob'
		    >>> classmates[3]
		    Traceback (most recent call last):
		      File "<stdin>", line 1, in <module>
		    IndexError: list index out of range
		    ```
	 - 负整数索引表示从右到左
	
			```
			>>> classmates[-1]
			'Tracy'
			>>> classmates[-2]
			'Bob'
			>>> classmates[-3]
			'Michael'
			```
	- `append()`追加元素
	
		```
		>>> classmates.append('Adam')
		>>> classmates
		['Michael', 'Bob', 'Tracy', 'Adam']
		```
	- `insert()`指定位置插入元素
	
		```
		>>> classmates.insert(1, 'Jack')
		>>> classmates
		['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
		```
	- `pop()`删除list末尾元素，`pop(i)`删除指定索引元素(返回被删除元素)
	
		```
		>>> classmates.pop()
		'Adam'
		>>> classmates
		['Michael', 'Jack', 'Bob', 'Tracy']
		
		>>> classmates.pop(1)
		'Jack'
		>>> classmates
		['Michael', 'Bob', 'Tracy']
		```
2. tuple是另一种有序列表叫元组	
 
	```
	>>> classmates = ('Michael', 'Bob', 'Tracy')
	```
	- 只有1个元素的tuple定义时必须加一个逗号`,`，来消除歧义
	
		```
		>>> t = (1,)
		>>> t
		(1,)
		```
		
## 条件判断

```
# 列1
age = 20
if age >= 6:
    print('teenager')
elif age >= 18:
    print('adult')
else:
    print('kid')
# 结果为 adult

# 列2
s = input('birth: ')
birth = int(s)
if birth < 2000:
    print('00前')
else:
    print('00后')
int(s)
```

## 循环

1. `for...in` 循环

	```
	names = ['Michael', 'Bob', 'Tracy']
	for name in names:
	    print(name)
	```
2. `while` 循环

	```
	sum = 0
	n = 99
	while n > 0:
	    sum = sum + n
	    n = n - 2
	print(sum)
	
	```
	
`break`语句可以在循环过程中直接退出循环，而`continue`语句可以提前结束本轮循环，并直接开始下一轮循环。这两个语句通常都必须配合if语句使用

不要滥用`break`和`continue`语句。break和continue会造成代码执行逻辑分叉过多，容易出错。大多数循环并不需要用到`break`和`continue`语句	

## `dict`和`set`
1. dict
	1. Python内置了字典：dict的支持,使用键-值（key-value）存储，具有极快的查找速度
	
		```
		>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
		>>> d['Michael']
		95
		
		# 多次对一个key放入value，后面的值会把前面的值冲掉
		>>> d['Jack'] = 90
		>>> d['Jack']
		90
		>>> d['Jack'] = 88
		>>> d['Jack']
		88
		```
	
	2. 如果key不存在，dict就会报错,避免key不存在的错误的两种方法：
		- 通过`in`判断
		
			```
			>>> 'Thomas' in d
			False
			``` 
		- 通过dict提供的get()方法，如果key不存在，可以返回None，或者自己指定的value：
		
			```
			>>> d.get('Thomas')
			>>> d.get('Thomas', -1)
			-1
			```
			注意：返回None的时候Python的交互环境不显示结果。
	
	3. 删除元素
	
		```
		>>> d.pop('Bob')
		75
		>>> d
		{'Michael': 95, 'Tracy': 85}
		```
	
	4. 对比`list`
	
	    | list        | dict    |
	    | --------   | -----:   |
	    | 查找和插入的时间随着元素的增加而增加  | 查找和插入的速度极快，不会随着key的增加而变慢 |
	    | 需要占用大量的内存，内存浪费多 | 占用空间小，浪费内存很少  |
	所以，dict是用空间来换取时间的一种方法。
	
2. set
set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key

 - 要创建一个set，需要提供一个list作为输入集合：
 
	```
	>>> s = set([1, 1, 2, 2, 3, 3])
	>>> s
	{1, 2, 3}
	```
 - `add(key)` 添加元素，`remove(key)`删除元素
 - set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作：
 
	 	```
		>>> s1 = set([1, 2, 3])
		>>> s2 = set([2, 3, 4])
		>>> s1 & s2
		{2, 3}
		>>> s1 | s2
		{1, 2, 3, 4}
	 	```
 
	