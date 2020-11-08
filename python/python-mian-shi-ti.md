# 语言基础

## Python 是静态还是动态类型？是强类型还是弱类型？

Python 属于动态强类型语言

* 动态类型语言是指在运行期间才去做数据类型检查的语言
* 强类型不会发生隐式转换

例如：Python 与 JS 对比计算 `1+"a"` 运算结果

{% tabs %}
{% tab title="Python" %}
```python
In [1]: 1 + "a"
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-1-be43d224e41a> in <module>()
----> 1 1 + "a"

TypeError: unsupported operand type(s) for +: 'int' and 'str'
```
{% endtab %}

{% tab title="JS" %}
```javascript
> 1 + "a"
< "1a"
```
{% endtab %}
{% endtabs %}

## 什么是鸭子类型？

> 当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子

在鸭子类型中，关注点在于对象的行为，能作什么，而不是关注对象所属的类型

```python
class Duck:
    def quack(self):
        print("这鸭子正在嘎嘎叫")

    def feathers(self):
        print("这鸭子拥有白色和灰色的羽毛")


class Person:
    def quack(self):
        print("这人正在模仿鸭子")

    def feathers(self):
        print("这人在地上拿起1根羽毛然后给其他人看")


def in_the_forest(duck):
    duck.quack()
    duck.feathers()


def game():
    donald = Duck()
    john = Person()
    in_the_forest(donald)
    in_the_forest(john)


game()
```

## 什么是 monkey patch？

所谓 monkey patch 就是运行时替换

```python
# ==================== 1

import socket
from gevent import monkey

print(socket.socket)  # <class 'socket.socket'>
monkey.patch_socket()
print(socket.socket)  # <class 'gevent._socket3.socket'>

# ==================== 2
import json  
import ujson  

def monkey_patch_json():  
    json.__name__ = 'ujson'  
    json.dumps = ujson.dumps  
    json.loads = ujson.loads  

monkey_patch_json()  
```

## 什么是对象自省？

运行是判断一个对象的类型能力，Python 中可以通过 id、type、inspect 模块进行查看对象信息。

```python
l = [1,2,3]
d = {'a':1}

# 运行期间判断类型
print(type(l)) # <class 'list'>
print(type(d)) # <class 'dict'>
```

## Python 2 / 3 差异问题？

{% tabs %}
{% tab title="print" %}
```python
# py2
print "hello world"

# py3
print("hello world")
```
{% endtab %}

{% tab title="编码" %}
```python
# py2
s = u'中文'
type(s) # unicode
s2 = "中文"
type(s) # str

# py3
s = u'中文'
type(s) # str
s = '中文'
type(s) # str
```
{% endtab %}

{% tab title="/" %}
```python
# py2
print(10/3) # 3

# py3
print(10/3) # 3.3333333333333335
print(10//3) # 3
```
{% endtab %}
{% endtabs %}

### Python3 改进

* 类型注解

```python
def hello(name: str) -> str:
    return "hello" + name

print(hello("world"))
```

* 解包

```python
a, b, *c = range(10)
print(a) # 0
print(b) # 1
print(c) # [2, 3, 4, 5, 6, 7, 8, 9]
```

* 限定关键字参数

```python
def func(a, b, *, c):
    return a + b + c

ret = func(1, 2, c=10) # * 号之后需要使用关键字参数
print(ret)
```

### Python3 新增

* yield from 链接子生成器
* asyncio 内置库，async、await 原生协程

## 说一下迭代器和生成器的区别

**迭代器**：符合迭代器的对象需要实现 `__next__` 方法，通过 `__iter__` 方法达到自身可迭代 

**生成器**：实现生成器需要定义 `yield` 关键字，通过 `yield` 的特性逐个产出值，达到可迭代。由于生成器拥有迭代器的特性，即生成器属于特殊的迭代器。

## 什么是线程安全，如何保证？

**线程安全**：多个线程访问共享资源的时候，不会对资源造成错乱。 

**如何保证**：通过 _**锁机制、条件变量、信号量**_ 等保证线程同步以此来达到线程安全。

## 谈一谈 GIL 对 Python 多线程的影响

**GIL 全称全局解释器锁：**作用是保证同一时刻只能有一个线程执行同一条字节码文件。即便在多核心处理器上，使用 GIL 的解释器也只允许同一时间执行一个线程。 

**GIL 的切换策略：**一种情况在该线程进入IO操作之前，会主动释放GIL；另一种情况是解释器不间断运行了**1000字节码（Py2）**或运行**15毫秒（Py3）**后，该线程也会放弃GIL。 所以 GIL 在底层字节码的层面保证了线程安全，但是限制了多线程处理任务的能力，不过对于 I/O 密集的操作，多线程仍然能够发挥优势。 

**如何避免：**替换 CPython 解释器为 PyPy 解释器，或者关键代码可以通过 C 语言编写。

## Python 是如何进行内存管理的？

## 深拷贝和浅拷贝的区别

**深拷贝**：深拷贝可以理解是完整的复制，新数据的修改不会影响原始数据 

**浅拷贝**：浅拷贝是外层拷贝，当数据存在嵌套可变数据时，比如：`[1,2,[5,6]]`，通过浅拷贝的得到的新数据对外层列表的修改不会影响原始数据，但是对于内层数据的修改会影响元素数据。

## \_\_new\_\_\(\) 和 \_\_init\_\_\(\)区别

## 什么是装饰器以及使用场景

**装饰器：**装饰器本质是一个函数，可以动态的修改某个函数运行前或运行后的行为的函数 

**使用场景：**日志装饰器、权限装饰器、Flask中的钩子函数

## 谈一谈 Python 中的多继承关系

## Python代码执行原理

1. 编写 Python 程序代码 
2. 运行 Python 程序，Python 解释器\(Cpython\)会将 xx.py 文件编译成一个字节码对象 PyCodeObject ，并且只会存在内存中，代码执行结束，会把字节码对象保存在 pyc 中，pyc 文件只是 PyCodeObject 对象在硬盘上的表现形式，通过 dis 这个 Python 标准库来获得代码对应的字节码 

**CPU识别字节码文件进行处理 编译过程：词法分析====&gt;语法分析====&gt;生成 .pyc 文件\(字节码文件\)====&gt; CPU 识别字节码文件进行处理**

## 如何自己实现一个字典

```python
class MyDict(object):

    def __getitem__(self, item):
        if hasattr(self, item):
            return getattr(self, item)
        raise KeyError

    def __setitem__(self, key, value):
        setattr(self, key, value)


if __name__ == '__main__':
    my_d = MyDict()
    my_d["a"] = 10
    print(my_d.__dict__)
    print(my_d["a"])
    print(my_d["b"])
```

## 什么是协程

协程不是被操作系统内核所管理，而完全是由程序所控制（用户态执行） 

通过生成器机制结合一定的方法来接受和产出值，并且可以在 `yield` 上下文文暂停和恢复，因此一个线程可以操作多个协程。

## Python的IO多路复用是怎么实现的

## 什么是上下文管理器以及实现方式

**上下文管理器：**用于管理对象精准的分配资源和释放资源。上下文管理器对象目的在于管理 with 语句。 

**with 语句做了那些事：**目的是简化 `try/finally` 模式。 

**实现方式：**

*  在 Python 中实现上下文管理器对象，自定义的管理器类需要实现 \_\_**enter\_\_** 方法和 \_\_**exit\_\_** 方法。
*  使用 `contextmanager` 装饰器对一个生成器函数进行装饰

## 你知道右加么（`__radd__`）

`__radd__` ：当可计算对象处于 + 号右侧时会调用

```python
class MyRadd(object):
    def __init__(self, value):
        self.val = value

    def __add__(self, other):
        return self.val + other

    def __radd__(self, other):
        return other + self.val


if __name__ == '__main__':
    my_r = MyRadd(10)

    ret1 = 10 + my_r # 执行 __radd__ 方法

    ret2 = my_r + 10 # 执行 __add__ 方法
```

## 谈谈元类、元类的应用

## 用 Python 写一个单例模式

```python
class Singleton(object):
    _instance = None

    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super().__new__(cls, *args, **kwargs)

        return cls._instance

    def __init__(self):
        pass


s1 = Singleton()
s2 = Singleton()
assert s1 is s2
```

## 谈谈 super 原理





