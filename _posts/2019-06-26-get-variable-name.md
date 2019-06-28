---
layout: post
title: "Python之谜：如何获取变量名？"
subtitle: ""
author: "肖哥shelwin"
header-img: "img/ta-summary1.jpg"
header-mask: 0.4
tags:
---
## 初步尝试
今天我们探讨Python中一个看似很简单，实则并不容易的问题。这个问题是：如何获得变量的名字？

举例说明，给定一个变量`var`，给它赋值字符串"foo"。即
```python
In [1]: var = "foo"
```
现在我们需要得到变量`var`的名字，即"var"。

尝试下面两种方法，我们得到的都是变量的值"foo"，而不是变量的名字"var"。
```python
In [2]: print var
foo

In [3]: print "%s" % var
foo
```
怎么办呢？我们接着探索变量`var`的全部属性和方法：
```python
In [4]: print dir(var)
['__add__', '__class__', '__contains__', ..., 'capitalize', 'center', ...]
```
经过尝试后发现，在`var`的71个(这个数字通过`print len(dir(var))`得到)属性和方法中，没有一个能够让我们得到`var`的名字。

由此可见，**得到变量值(value)容易，得到变量名(name)看起来十分困难**。
## 两个示例
在进一步探索其他方案之前，我们先看下"获取变量名"在Python编程中到底有没有实际意义。这里，举两个小例子，说明"获得变量名"的好处。
### 示例1
第一个例子是关于异常处理的。如果能够得到变量名，那么Python异常的错误消息(Error Message)将变得更加有意义。例如：
```python
      1 first_num, second_num = 1, 2
      2 if first_num != second_num:
----> 3     raise RuntimeError("%s is not equal to %s" % (first_num, second_num))

RuntimeError: 1 is not equal to 2
```
这里错误消息`RuntimeError: 1 is not equal to 2`的可读性是比较差的，1和2分别是什么？我们需要结合源码才能明白。假如能够在Error Message中输出1和2各自的变量名`first_num`和`second_num`，无疑是一件好事情。
### 示例2
第二个例子是基于一组变量创建字典。已知若干变量，`name, address, age, gender`，我们希望基于它们创建一个字典`person`。一般可以这样实现：
```python
person = {}
person["name"] = name
person["address"] = address
person["age"] = age
person["gender"] = gender
```
虽然这种实现完全可行，但是对于像我这样有"洁癖"的Python爱好者来说，有两点觉得不舒服。一是变量的名字出现了重复；二是赋值语句(不管是`=`赋值还是`:`赋值)出现了重复。当变量数量越多时，重复情况就越多。

如果能够使用类似下面这种循环来实现，代码就能更加简洁：
```python
for i in [name, address, age, gender]:
    person[i] = i # 由于i是变量值，因此不可行
```
以上两个示例说明获得变量名对于Python编程是有好处的。
## 一种间接方案
然而，在前文我们也看到了，得到Python变量名是困难的。那么，为什么无法直接得到Python变量名呢？查阅相关资料后发现，在Python中，变量名字是对象(object)的单向(而不是双向)引用。也就是说，根据变量名，能够直接得到它所指向的对象；反之，根据对象，是无法得到指向它的变量名的。

之所以有这么一个规定，是基于成本考虑的。在程序中，往往存在大量的变量(整型，字符串，列表，字典，布尔...)。如果每一个变量都需要有一个包含指向它的变量名的列表，那么这些列表的创建和维护的成本(实现成本，执行成本...)将变得难以承受。

既然直接方法不可行，那么有没有间接的方法呢？Python的`inspect`模块提供了一种间接方案。以示例2为例，基于`inspect`的For循环实现为：
```python
import inspect

def retrieve_name(var):
    callers_local_vars = inspect.currentframe().f_back.f_locals.items()
    return [var_name for var_name, var_val in callers_local_vars if var_val is var]

name, address, age, gender = "bob", "hangzhou", 21, "man"
person = {}

for i in [name, address, age, gender]:
    person[retrieve_name(i)[0]] = i

print person
```
执行脚本，结果符合预期：
```shell
root@hzettv53:~# python test.py
{'gender': 'man', 'age': 21, 'name': 'bob', 'address': 'hangzhou'}
```
这种方案的基本思路是：通过`inspect`模块的`currentframe()`方法得到一个包含调用者的所有局部变量的名字和值的列表，然后从列表中搜索变量值与目标值相同的元素，并返回该元素对应的变量名。

虽然这种方案在一些情况下能够工作，但是其弊端是显而易见的：1) 实现的复杂度较高 2) 如果存在多个变量值相同的情况，那么retrieve_name()函数将返回多个变量名，从而无法得到精确结果。基于这两点因素，这个方法中并没有多大的实际应用意义。
## 总结
综上所述，我们可以看到，Python编程中并不存在一种高效和可靠的方法去获得变量名。所幸的是，尽管获得变量名在某些场景下有好处，但是这种好处往往并不显著，并且一般都存在替代方案。因此，尽管Python无法支持获得变量名这一需求，但是日常的Python开发工作并不会因此受到多大影响。