---
title: python2与python3主要的区别
date: 2020-11-10 19:09:34
tags:
---

# python2 与python3 主要的区别

今天了解了一下python2 与python3的区别，看看python迭代的历史。

1. print

```python
# python 2 中 print是一条语句，打印后面的全部内容
print("hello", "world") # （"hello", "world"）

# python 3 中 print是一个函数，打印接受的参数
print("hello", "world") # hello world
```

2. 编码

```python
# python 2 中默认编码方式为 ascii
所以在py文件中经常会发现文件顶部都有 # coding=utf-8

# python 3 中默认编码方式为 utf-8
```

3. 字符串

```python
# 在python2 中 字符串有两个类型：1（unicode）文本字符串，2（str）字节序列

# 在python3 中 改变了字符串的类型：1（str）字符串，2（byte）字节序列
```

4. True和False

```python
# python 2 中 这两个是全局变量，可以指向其他对象，重新赋值

# python 3 中 这两个是关键字，永远固定
```

5. 对象类型

```python
# python 2 中，字典对象的方法，高阶函数map、filter、返回的列表，并且迭代器必须实现next方法

# python 3 中，字典对象的方法，比如dict.keys()，类型为dict_key对象，dict.value(),类型为dict_value对象，map函数返回的也是一个迭代器对象，并且迭代器实现改成__next__方法
```

6. 类

```python
# python 2 中，默认经典类，继承object才是新式类

# python 3 中，默认为新式类，不需要声明object
新式类采用广度优先搜索，而旧式类是采用深度优先搜索
```

7. 整除

```python
# python 2 中， /计算结果为整型  1/2 ==> 0

# python 3 中， /计算结果为浮点型	1/2 ==>0.5
```

8. input()

```python
# python 2 中 用户输入有两种 raw_input() 和 input()
raw_input()---将所有输入作为字符串看待，返回字符串类型； input()只能接收"数字"的输入，

# python 3 中 用户输入只有一种 input()
将所有输入默认为字符串处理，并返回字符串类型
```

9. xrange，

```python
# python 2 中 for循环用xrange()创建一个可迭代对象，它类似一个生成器。 
#					range()生成一个列表

# python 3 中，将xrange()和range()合并为range()，返回一个list形式的对象，type类型为(class 'range')，运行速度比python2要慢一些
```

10. _contains_方法

```python
# python 3 中 range()有了一个新的__contains__方法。
# __contains__方法可以有效的加快Python 3 中整数和布尔型的“查找”速度。
```

11. 不等运算符

```python
# python 2 中不等于有两种写法 != 和 <>
# python 3 中去掉了<>, 只有 != 一种写法
```

12. 数据类型

```python
#python 3 中去除了long类型，现在只有一种整型——int，但它的行为就像python 2版本的long
#新增了bytes类型，对应于 python 2 版本的八位串
```

13. 异常处理

```python
# python 2 中 用逗号隔开异常的内容和名字
try:
 let_us_cause_a_NameError
except NameError, err:
 print err

# python 3 中 必须使用“as”关键字。
try:
 let_us_cause_a_NameError
except NameError as err:
 print(err)
```





