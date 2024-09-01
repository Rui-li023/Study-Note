# Python

```python
print('12','12312')
name = input('请输入名字：') #输入元素
```

**采用缩进来确定代码块**，约定是四个空格为缩进，**不要混合使用tab和空格**

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-//通常在代码前面加上这两行
```



## 基础

### 数值

使用`_`分割长数字：

```python
i = 1_000_000_000
```

使用decimal模块解决浮点数精度问题



### 字符串

字符串格式化：

```python
username, score = 'piglei', 100
# 1. C 语言风格格式化
print('Welcome %s, your score is %d' % (username, score))
# 2. str.format
print('Welcome {}, your score is {:d}'.format(username, score))
# 3. f-string，最短最直观
print(f'Welcome {username}, your score is {score:d}')
# 输出：
# Welcome piglei, your score is 100
```

#### `.partition(sep)`

根据传入的分隔符进行分割

返回的内容包含：`(part_before, sep, part_after)`

#### `.join()`

`join()` 是字符串方法，用于将一个可迭代对象（如列表、元组）中的元素连接成一个字符串，并在每个元素之间插入指定的字符串分隔符。

##### 语法
```python
separator = " "
result = separator.join(iterable)
```

- `separator`：分隔符字符串，决定元素之间的连接符。
- `iterable`：可迭代对象，如列表、元组等，其中的元素必须是字符串类型。

##### 示例

1. **使用 `list` 作为参数**：
   
   ```python
   words = ["hello", "world"]
   result = " ".join(words)
   print(result)  # 输出 "hello world"
   ```
   
2. **使用自定义分隔符**：
   ```python
   words = ["2024", "08", "31"]
   result = "-".join(words)
   print(result)  # 输出 "2024-08-31"
   ```



### 集合

```python
classmate = ['Tim','Nick','Alex']
s = ['python', 'java', ['asp', 'php'], 'scheme'] #多维数组
classmates = ('Michael', 'Bob', 'Tracy')		#tuple,无法改变的数组
s = (1,)								   #消除歧义，是一个tuple
#tuple里面可以内嵌list，list的内容是可以改变的
```

#### `list`

使用字符串生成一个list，能够让每个字符成为一个元素。



#### `dit`

```python
#类似于map，key和value对应
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
#检查是否存在
'Bob' in d
d.get('Thomas')
d.get('Thomas', -1)  #如果不存在可以返回none，或者自己指定的value
d.pop('Bob') #删除对应的value
```

##### 字典合并

在 Python 中，可以通过多种方法将两个字典合并为一个新的字典。以下是一些常见的合并字典的方法：

###### 1. 使用 `update()` 方法
`update()` 方法直接在原字典上进行修改，将另一个字典的键值对更新到原字典中。

```python
dict1 = {'a': 1, 'b': 2}
dict2 = {'b': 3, 'c': 4}
dict1.update(dict2)
print(dict1)  # 输出 {'a': 1, 'b': 3, 'c': 4}
```

###### 2. 使用 `**` 解包运算符（Python 3.5+）
可以通过解包 `**` 将两个字典合并，并生成一个新的字典。

```python
dict1 = {'a': 1, 'b': 2}
dict2 = {'b': 3, 'c': 4}
merged_dict = {**dict1, **dict2}
print(merged_dict)  # 输出 {'a': 1, 'b': 3, 'c': 4}
```

###### 3. 使用 `dict()` 构造函数和解包
与上面的方法类似，但使用 `dict()` 构造函数来创建新的字典。

```python
dict1 = {'a': 1, 'b': 2}
dict2 = {'b': 3, 'c': 4}
merged_dict = dict(dict1, **dict2)
print(merged_dict)  # 输出 {'a': 1, 'b': 3, 'c': 4}
```

###### 4. 使用 `|` 运算符（Python 3.9+）
在 Python 3.9 及更高版本中，可以使用 `|` 运算符来合并两个字典，生成一个新的字典。

```python
dict1 = {'a': 1, 'b': 2}
dict2 = {'b': 3, 'c': 4}
merged_dict = dict1 | dict2
print(merged_dict)  # 输出 {'a': 1, 'b': 3, 'c': 4}
```

###### 5. 使用 `collections.ChainMap`（临时合并）
`ChainMap` 允许将多个字典组合在一起，但不会真正合并字典，而是提供一个视图。

```python
from collections import ChainMap

dict1 = {'a': 1, 'b': 2}
dict2 = {'b': 3, 'c': 4}
merged_dict = ChainMap(dict1, dict2)
print(dict(merged_dict))  # 输出 {'a': 1, 'b': 2, 'c': 4}
```

**注意**：`ChainMap` 是临时的，只提供字典的合并视图，不会创建新字典。

###### 6. 使用字典推导式
可以通过字典推导式遍历两个字典的键值对来合并。

```python
dict1 = {'a': 1, 'b': 2}
dict2 = {'b': 3, 'c': 4}
merged_dict = {k: v for d in (dict1, dict2) for k, v in d.items()}
print(merged_dict)  # 输出 {'a': 1, 'b': 3, 'c': 4}
```



#### `collections`

`collections` 是Python内建的一个集合模块，提供了许多有用的集合类和方法。

可以把它理解为一个容器，里面提供Python标准内建容器 `dict`, `list` , `set` 和 `tuple` 的替代选择。



#### `tuple`

元组是**有序**且**不可更改**的集合。在Python中，元组使用圆括号 `()` 编写的。



### 条件判断

布尔值 `and`,`or`,`not`运算，`:`区分代码块

```python
if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
elif <条件判断3>:
    <执行3>
else:
    <执行4>
```



### 循环

```python
for i in range(5):	#i为0-4
```



### 函数

- 定义函数要用`def`

- 导入函数 `from abstract import my_abs`

- 数据类型检查：

```python
if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
```

- 返回多个值

```python
import math

def move(x, y, step, angle=0):
    nx = x + step * math.cos(angle)
    ny = y - step * math.sin(angle)
    return nx, ny

x, y = move(100, 100, 60, math.pi / 6)
```

返回的值是`tuple`，但是可以用多个变量按位置接受

- 设置默认参数：

与c++类似，但是要把默认的值放后面，需要赋值的值放前面，不然python会给出编译错误

- **默认参数要牢记一点：默认参数必须指向不变对象**

为什么要设计`str`、`None`这样的不变对象呢？因为不变对象一旦创建，对象内部的数据就不能修改，这样就减少了由于修改数据导致的错误。此外，由于对象不变，多任务环境下同时读取对象不需要加锁，同时读一点问题都没有。我们在编写程序时，如果可以设计一个不变对象，那就尽量设计成不变对象。

- 把函数参数设计成可变参数

```python
def calc(*numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum
```

- 关键字参数

而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个`dict`：

```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
    
person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}

>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
>>> person('Jack', 24, **extra)
name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
```

如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符`*`了：

```python
def person(name, age, *args, city, job):
    print(name, age, args, city, job)
```

命名关键字参数必须传入参数名，这和位置参数不同。如果没有传入参数名，调用将报错



**在Python中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用。但是请注意，参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。**



- 递归

解决递归调用栈溢出的方法是通过**尾递归**优化，事实上尾递归和循环的效果是一样的

尾递归是指，**在函数返回的时候，调用自身本身，并且，return语句不能包含表达式。**这样，编译器或者解释器就可以把尾递归做优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出的情况。



#### `*args, **kwargs `代表什么

##### 1. `*args`
`*args` 代表"任意数量的非关键字参数"。它允许你传递任意数量的参数给一个函数，这些参数会被作为一个元组接收。

- **全称**：arguments
- **用法示例**：

```python
def example_function(*args):
    for arg in args:
        print(arg)

example_function(1, 2, 3)  # 输出 1, 2, 3
```

在这个例子中，`example_function` 可以接收任意数量的参数，并在内部使用它们。

##### 2. `**kwargs`
`**kwargs` 代表"任意数量的关键字参数"。它允许你传递任意数量的键值对参数给一个函数，这些参数会被作为一个字典接收。

- **全称**：keyword arguments
- **用法示例**：

```python
def example_function(**kwargs):
    for key, value in kwargs.items():
        print(f"{key} = {value}")

example_function(a=1, b=2, c=3)  # 输出 a = 1, b = 2, c = 3
```

在这个例子中，`example_function` 可以接收任意数量的关键字参数，并在内部使用它们。



### 错误

- `IOError `：输入输出异常
- `AttributeError` ：试图访问一个对象没有的属性
- `ImportError` ：无法引入模块或包 / 对象
- `IndentationError` ：代码缩进错误
- `IndexError` ：下标索引超出边界
- `KeyError` ：试图访问映射中不存在的键
- `SyntaxError` ：Python语法错误
- `NameErrir` ：未声明/初始化对象 (没有属性)
- `TabError` ：Tab和空格混用
- `ValueError` ：传入无效的参数
- `OverflowError` ：数值运算超出最大限制



### 列表生成式

```python
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
```

通过这种方法可以创建一个列表，但是收到内存限制，列表的容量是有限的

所以就需要生成器：generator



### 关键字

#### `is`和`==`的关系

`Python` 中的对象包含三个要素：`id`、`type` 和 `value`。`is` 比较的是两个对象的 `id`。`==` 比较的是两个对象的 `value`。



## 高级特性

### 切片

```python
L[0:3] #取前三个元素
L[:3] 	#如果是0可以省略
L[-2:] #倒数两个 注意倒过来是第一个元素是-1
```



### 迭代

```python
for ch in 'abc'
#判断方法
>>> from collections.abc import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```



### 列表生成式

```python
[x*x for x in range(1,4)] #[1,4,9,16]
#筛选偶数的平方
>>> [x * x for x in range(1, 11) if x % 2 == 0]
[4, 16, 36, 64, 100]
```



### 生成器

使用next算出下一个值

编写函数时使用yield返回，调用next时会从yield开始



第一种方法：

```python
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
```

`generator`保存的是算法，而不是数据





## 面向对象编程

### 类（Class）

- 类是创建对象的模板或蓝图。它定义了对象的属性（数据）和方法（行为）。
- 例如，在 Python 中可以通过 `class` 关键字定义一个类：

  ```python
  class Car:
      def __init__(self, make, model, year):
          self.make = make
          self.model = model
          self.year = year
  
      def display_info(self):
          print(f"{self.year} {self.make} {self.model}")
  ```

#### 类的生命周期

##### 1. 类的定义
   - 类的生命周期始于类的定义。在 Python 中，使用 `class` 关键字定义一个类。
   - 当 Python 解释器遇到 `class` 关键字时，它会创建一个新的类对象，这个对象包含了类的所有方法和属性。

   ```python
   class MyClass:
       class_variable = 0  # 类属性

       def __init__(self, value):
           self.instance_variable = value  # 实例属性
   ```

##### 2. 类的实例化
   - 实例化是创建类的对象的过程。这一步涉及调用 `__new__` 方法和 `__init__` 方法。

1. `__new__` 方法

- **职责**：创建对象实例。
- **调用时机**：在创建对象之前调用。
- **返回值**：必须返回一个类的实例（通常是通过调用 `super().__new__(cls)` 来获取）。
- **用法**：主要用于控制类的实例化过程，比如实现单例模式、定制元类等。

`__new__` 是一个类方法，而不是实例方法。它的第一个参数是类本身（通常命名为 `cls`），而不是实例（实例是在 `__new__` 里创建的）。

- **示例**：

```python
class MyClass:
    def __new__(cls, *args, **kwargs):
        print("Calling __new__")
        instance = super().__new__(cls)
        return instance
    
    def __init__(self, value):
        print("Calling __init__")
        self.value = value

obj = MyClass(10)
```

在这个例子中，`__new__` 在 `__init__` 之前被调用，用于创建对象。

2. `__init__` 方法

- **职责**：初始化对象实例。
- **调用时机**：在对象创建之后调用（即在 `__new__` 之后）。
- **返回值**：不需要返回值，它只能返回 `None`。
- **用法**：用于为对象分配初始属性值。

`__init__` 是一个实例方法，它的第一个参数是实例本身（通常命名为 `self`）。

- **示例**：

```python
class MyClass:
    def __new__(cls, *args, **kwargs):
        instance = super().__new__(cls)
        return instance
    
    def __init__(self, value):
        print("Calling __init__")
        self.value = value

obj = MyClass(10)
```

##### 3. 对象的使用
   - 创建对象后，你可以访问和修改它的属性，调用它的方法。
   - 对象会在其生命周期内处理各种任务，并且可能会被其他代码引用或传递。

   ```python
   print(obj.instance_variable)  # 访问对象属性
   obj.instance_variable = 20     # 修改对象属性
   ```

##### 4. 对象的销毁
   - 当一个对象的引用计数变为 0 时，即不再有任何变量引用它，Python 会自动销毁这个对象。
   - **`__del__` 方法**：
     - 如果对象定义了 `__del__` 方法，这个方法将在对象即将被销毁时调用。这个方法通常用于清理资源（例如关闭文件、释放网络连接等）。
   - 对象的内存会被回收，供未来使用。

   ```python
   del obj  # 手动删除对象引用
   ```

##### 5. 垃圾回收
   - Python 使用垃圾回收机制来管理内存，主要通过引用计数机制。当对象的引用计数为 0 时，垃圾回收器会销毁该对象。
   - 在某些情况下（例如循环引用），垃圾回收器会通过标记-清除或分代回收来处理这些对象。

### 对象（Object）

- 对象是类的实例。每个对象都有属于自己的属性和方法。
- 例如，使用上面的 `Car` 类，可以创建一个对象：

  ```python
  my_car = Car("Toyota", "Corolla", 2020)
  my_car.display_info()  # 输出: 2020 Toyota Corolla
  ```

### 属性（Attributes）和方法（Methods）

- 属性是对象的数据字段，用来存储对象的状态。方法是对象的函数，用来定义对象的行为。
- 属性通常在类的 `__init__` 方法中定义，方法是类中定义的函数。

### 封装（Encapsulation）

- 封装是将对象的状态（属性）和行为（方法）打包在一起的过程，同时对外界隐藏对象的具体实现。
- 通过使用访问控制（例如私有属性）来限制外部对对象内部状态的直接访问。

1. **继承（Inheritance）**:
   - 继承是一种创建新类的机制，新类（子类）可以继承现有类（父类）的属性和方法。
   - 子类可以扩展或修改父类的功能，同时保持代码的复用性。

     ```python
     class ElectricCar(Car):
         def __init__(self, make, model, year, battery_size):
             super().__init__(make, model, year)
             self.battery_size = battery_size
     
         def display_battery_info(self):
             print(f"This car has a {self.battery_size}-kWh battery.")
     ```

2. **多态（Polymorphism）**:
   - 多态性是指同一方法在不同对象上具有不同的表现形式。它允许在不改变代码的情况下使用不同的对象。
   - 例如，多个子类可以重写父类的方法，每个子类实现不同的行为。

     ```python
     class Dog:
         def speak(self):
             return "Woof!"
     
     class Cat:
         def speak(self):
             return "Meow!"
     
     animals = [Dog(), Cat()]
     for animal in animals:
         print(animal.speak())  # 输出: "Woof!" 和 "Meow!"
     ```

3. **抽象（Abstraction）**:
   - 抽象是指只暴露对象的必要功能，而隐藏其实现细节。它帮助简化复杂系统的表示和使用。
   - 在 Python 中，可以使用抽象类和方法来实现这一点。抽象类不能实例化，只能被继承，并且其抽象方法必须在子类中实现。



## 函数式编程

### 高级函数

#### `map`

```python
def f(x):
	return x*x

>>>list[map(f,[1,2,3,4,5])]
[1,4,9,16,25]
```

map传入两个值，第一个是函数，一个是`Iterable`，最终返回一个`Iterator`

#### `reduce`

```python
>>> from functools import reduce
>>> def fn(x, y):
...     return x * 10 + y
...
>>> reduce(fn, [1, 3, 5, 7, 9])
13579
```

操作两个数，返回值与下一个数继续操作

#### `filter`

筛选函数，可以对任何序列进行操作

可以理解为将一个筛选方法加入到对象中，如果是一个`Iterator`则调用next寻找下一个

##### 语法：

以下是 filter() 方法的语法:

filter(function, iterable)

#### `sorted`

`sorted(iterable, key=None, reverse=False)`

- `iterable`：要排序的可迭代对象。
- `key`（可选）：一个函数，指定元素的排序依据（即排序键）。默认是 `None`，表示直接对元素进行排序。
- `reverse`（可选）：布尔值。如果为 `True`，则降序排序；如果为 `False`（默认值），则升序排序。

排序函数，可以加入一个方法决定排序的规则



### 返回函数

```python
>>> f = lazy_sum(1, 3, 5, 7, 9)
>>> f
<function lazy_sum.<locals>.sum at 0x101c6ed90>
>>> f()
25
```

```python
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum
```

> 在这个例子中，我们在函数`lazy_sum`中又定义了函数`sum`，并且，内部函数`sum`可以**引用外部函数`lazy_sum`的参数*和局部变量，当`lazy_sum`返回函数`sum`时，相关参数和变量都保存在返回的函数中，这种称为“闭包（Closure）”的程序结构拥有极大的威力。

**返回闭包时牢记一点：返回函数不要引用任何循环变量，或者后续会发生变化的变量。**

## 问题与解决

1. vscode无法使用python

   - ubuntu默认使用的不是python3版本，需要建立链接

   ```python
   sudo ln -s /usr/bin/python3.8 /usr/bin/python
   ```

