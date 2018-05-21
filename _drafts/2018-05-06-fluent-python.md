---
title : 流畅的Python
layout : page
category : wiki
---

## Python2和3的区别

https://docs.python.org/3.0/whatsnew/3.0.html

使用`doctest`来测试

## 数据模型

obj[key]  => obj.__getitem__(key)

#### collections.namedtuple

```python
>>> from collections import namedtuple
>>> Point = namedtuple('Point', ['x', 'y'])
>>> p = Point(1, 2)
>>> p.x
1
>>> p.y
2
```
它用来创建一个自定义的tuple对象，并且规定了tuple元素的个数，并可以用属性而不是索引来引用tuple的某个元素，不需要class，Point对象是tuple的一种子类。

#### random.choice

choice(list)

### __len__
CPython中，对于内置类型实例，直接获取C结构体的属性

### __getitem__

实现了
- 迭代
- 切片
-

### __contain__

in运算符，没有__contain__的时候，顺序做一次迭代搜索

### sorted

list = sorted(list, key=lambda x: x.value)

### __setitem__

### 继承object
Python3默认，Python2需要显式声明

### __iter__

for i in x:

### __str__
str(obj)和print时调用

### 运算

#### __repr__
对象的字符串表示`<obj object at 0xaaaaa>`
%r 对象类型的标准字符串表示形式
如果没有__str__，用__repr__代替

#### __add__

#### __abs__

#### __mul__
没有考虑交换律

#### __bool__
自己定义类的实例一般为真，不存在__bool__时，调用__len__

list.count(x)

## 序列数组

### 内置序列类型
#### 容器序列
list、tuple 和 collections.deque
这些序列能存放不同类型的数据。
#### 扁平序列
str、bytes、bytearray、memoryview 和 array.array
这类序列只能容纳一种类型。

容器序列存放的是它们所包含的任意类型的对象的引用，而扁平序列里存放的是值而不是引用。换句话说，扁平序列其实是一段连续的内存空间。

可变序列list、bytearray、array.array、collections.deque 和 memoryview。
不可变序列tuple、str 和 bytes。

### 列表推导

codes = [ord(symbol) for symbol in symbols]

Python2.x中存在列表推导的变量泄露问题

tshirts = [(color, size) for color in colors for size in sizes]

### 生成器表达式

遵守了迭代器协议，可以逐个地产出元素，避免占用多余内存
tuple(ord(symbol) for symbol in symbols)

import array
array.array('I', (ord(symbol) for symbol in symbols))

### 元组
拆包
for country, _ in traveler_ids:
    print(country)
可以用 * 运算符把一个可迭代对象拆开作为函数的参数：
```python
>>> divmod(20, 8) (2, 4)
>>> t = (20, 8)
>>> divmod(*t) (2, 4)

>>> a, *body, c, d = range(5)
>>> a, body, c, d
(0, [1, 2], 3, 4)
```

#### 具名元组
collections.namedtuple工厂函数

Card = collections.namedtuple('Card', ['rank', 'suit'])
City = namedtuple('City', 'name country population coordinates')
tokyo = City('Tokyo', 'JP', 36.933, (35.689722, 139.691667))
delhi_data = ('Delhi NCR', 'IN', 21.935, LatLong(28.613889, 77.208889))
直接City(delhi_data)不可行，需要拆包
```python
delhi = City._make(delhi_data)
City(*delhi_data)
```

`_asdict()` 把具名元组以 collections.OrderedDict 的形式返回
```
for key, value in delhi._asdict().items():
         print(key + ':', value)
```

浅拷贝 深拷贝
元组没有 __reversed__ 方法，但是这个方法只是个优化而已，reversed(my_tuple) 这个用法在没有 __reversed__ 的情况下也是合法的。

### 切片
对 seq[start:stop:step] 进 行 求 值 的 时 候，Python 会 调 用 seq.
__getitem__(slice(start, stop, step))

DESCRIPTION = slice(6, 40)
item[DESCRIPTION]

a[i, j] => a.__getitem__((i, j))


Numpy中，如果 x 是四维数组，那么 x[i, ...] 就是 x[i, :, :, :]

如果赋值的对象是一个切片，那么赋值语句的右侧必须是个可迭代对象。即便只有单独一个值，也要把它转换成可迭代的序列。

### 对序列使用+和*
如果在 a * n 这个语句中，序列 a 里的元素是对其他可变对象的引用的话，你就需要格外注意了，因为这个式子的结果可能会出乎意料。比如，你想用my_list = [[]] * 3 来初始化一个由列表组成的列表，但是你得到的列表里包含的 3 个元素其实是 3 个引用，而且这 3 个引用指向的都是同一个列表。


使用列表推导
```
board = [['_'] * 3 for i in range(3)]
```

### 序列的增量赋值
__iadd__
如果一个类没有实现这个方法的话，Python 会退一步调用 __add__。

同时对可变序列（ 例 如 list、bytearray 和 array.array）来说，a 会就地改动，就像调用了 a.extend(b) 一样。但是如果 a 没有实现 __iadd__ 的话，a += b 这个表达式的效果就变得跟 a = a + b 一样了：首先计算 a + b，得到一个新的对象，然后赋值给 a。对象会不会新建和 __iadd__ 有关

不要把可变对象放在元组里面。
增量赋值不是一个原子操作。 我们刚才也看到了， 它虽然抛出了异常， 但还是完成了操作。

### sorted sort

list.sort 方法会就地排序列表，返回None
sorted，它会新建一个列表作为返回值。

sorted(list, reverse=True, key=len)

### bisect

bisect
有 lo 和 hi 两个可选参数用来控制查找的范围
bisect_left 返回的插入位置是原序列中跟被插入元素相等的元素的位置
bisect_right 返回的则是跟它相等的元素之后的位置

```
>>> def grade(score, breakpoints=[60, 70, 80, 90], grades='FDCBA'):
...     i = bisect.bisect(breakpoints, score)
...     return grades[i]
...
>>> [grade(score) for score in [33, 99, 77, 70, 89, 90, 100]]
['F', 'A', 'C', 'C', 'B', 'A', 'A']
```

insort
insort(seq, item)
也有insort_left，这个变体在背后用的是 bisect_left

left和right主要在相等的时候有区别

### 数组
array.array
.frombytes 和 .tofile

array('b')
array('d', (random() for i in range(10**7)))
floats.tofile(fp)
floats2.fromfile(fp, 10**7)

### memoryview
memoryview 是一个内置类，它能让用户在不复制内容的情况下操作同一个数组的不同切片。

```
>>> numbers = array.array('h', [-2, -1, 0, 1, 2])
>>> memv = memoryview(numbers)  
>>> len(memv) 5 >>> memv[0]  
-2
>>> memv_oct = memv.cast('B')
```

### deque
from collections import deque
dq = deque(range(10), maxlen=10)
dq.rotate(3)
dq.appendleft(-1)
dq.extend([11, 22, 33])
dq.extendleft([10, 20, 30, 40])

append 和 popleft 都是原子操作，也就说是 deque 可以在多线程程序中安全地当作先进先出的队列使用，而使用者不需要担心资源锁的问题。

queue
提供了同步（线程安全）类 Queue、LifoQueue 和 PriorityQueue，不同的线程可以利用这些数据类型来交换信息。
如果队列满了，它就会被锁住

multiprocessing
这个包实现了自己的 Queue，它跟 queue.Queue 类似，是设计给进程间通信用的。同时还有一个专门的 multiprocessing.JoinableQueue 类型，可以让任务管理变得更方便。

asyncio
里面有 Queue、LifoQueue、PriorityQueue 和 JoinableQueue，这些类受到 queue 和 multiprocessing 模块的影响，但是为异步编程里的任务管理提供了专门的便利。

heapq
heapq 没有队列类，而是提供了 heappush 和 heappop 方法，让用户可以把可变序列当作堆队列或者优先队列来使用。





有些对象里包含对其他对象的引用；这些对象称为容器
扁平序列因为只能包含原子数据类型，比如整数、浮点数或字符，所以不能嵌套使用。


## 字典和集合

### 泛映射类型
即只有可散列的数据类型才能用作这些映射里的键

如果一个对象是可散列的，那么在这个对象的生命周期中，它的散列值是不变的，而且这个对象需要实现 __hash__() 方法。另外可散列对象还要有 __eq__() 方法，  这样才能跟其他键做比较。如果两个可散列对象是相等的，那么它们的散列值一定是一样的……

Python 里所有的不可变类型都是可散列的，元组的话，只有当一个元组包含的所有元素都是可散列类型的情况下，它才是可散列的。

用户自定义的类型的对象都是可散列的，散列值就是它们的 id() 函数的返回值，所以所有这些对象在比较的时候都是不相等的。如果一个对象实现了 __eq__ 方法，并且在方法中用到了这个对象的内部状态的话，那么只有当所有这些内部状态都是不可变的情况下，这个对象才是可散列的。

```
>>> a = dict(one=1, two=2, three=3)
>>> b = {'one': 1, 'two': 2, 'three': 3}
>>> c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
>>> d = dict([('two', 2), ('one', 1), ('three', 3)])
>>> e = dict({'three': 3, 'one': 1, 'two': 2})
```

### 字典推导
country_code = {country: code for code, country in DIAL_CODES}

defaultdict，OrderedDict

d.get(k, default) 来代替 d[k]，给找不到的键一个默认的返回值


my_dict.setdefault(key, []).append(new_value)

if key not in my_dict:
     my_dict[key] = []
my_dict[key].append(new_value)



dd = defaultdict(list)
(1) 调用 list() 来建立一个新列表。
(2) 把这个新列表作为值，'new-key' 作为它的键，放到 dd 中。
(3) 返回这个列表的引用。

如果有一个类继承了 dict，然后这个继承类提供了 __missing__ 方法，那么在 __getitem__ 碰到找不到的键的时候，Python 就会自动调用它，而不是抛出一个KeyError 异常。
如 果 要 自 定 义 一 个 映 射 类 型， 更 合 适 的 策 略 其 实 是 继 承 collections.UserDict 类


collections.OrderedDict
这个类型在添加键的时候会保持顺序，因此键的迭代次序总是一致的。OrderedDict 的 popitem 方法默认删除并返回的是字典里的最后一个元素，但是如果像 my_odict.popitem(last=False) 这样调用它，那么它删除并返回第一个被添加进去的元素。

collections.ChainMap
该类型可以容纳数个不同的映射对象，然后在进行键查找操作的时候，这些对象会被当作一个整体被逐个查找，直到键被找到为止。

collections.Counter
这个映射类型会给键准备一个整数计数器。每次更新一个键的时候都会增加这个计数器。

colllections.UserDict
这个类其实就是把标准 dict 用纯 Python 又实现了一遍。

从 Python 3.3 开始，types 模块中引入了一个封装类名叫 MappingProxyType，返回一个只读的映射视图，如果对原映射做出了改动，我们通过这个视图可以观察到，但是无法通过这个视图对原映射做出修改。


### 集合
集合中的元素必须是可散列的，set 类型本身是不可散列的，但是 frozenset 可以。因此可以创建一个包含不同 frozenset 的 set。
a | b 返回的是它们的合集，a & b 得到的是交集，而 a - b 得到的是差集。
found = len(set(needles).intersection(haystack))

dis.dis（反汇编函数）

像 {1, 2, 3} 这种字面量句法相比于构造方法（set([1, 2, 3])）要更快且更易读。

#### 集合推导
{chr(i) for i in range(32, 256) if 'SIGN' in name(chr(i),'')}


一个可散列的对象必须满足以下要求。
(1) 支持 hash() 函数，并且通过 __hash__() 方法所得到的散列值是不变的。
(2) 支持通过 __eq__() 方法来检测相等性。
(3) 若 a == b 为真，则 hash(a) == hash(b) 也为真。

如果你实现了一个类的 __eq__ 方法，并且希望它是可散列的，那么它一定要有个恰当的 __hash__ 方法，保证在 a == b 为真的情况下 hash(a) == hash(b) 也必定为真。否则就会破坏恒定的散列表算法，导致由这些对象所组成的字典和集合完全失去可靠性，这个后果是非常可怕的。另一方面，如果一个含有自定义的 __eq__ 依赖的类处于可变的状态，那就不要在这个类中实现 __hash__ 方法，因为它的实例是不可散列的。

由 dict([key1, value1), (key2, value2)] 和 dict([key2, value2], [key1, value1]) 得到的两个字典，在进行比较的时候，它们是相等的；但是如果在 key1 和 key2 被添加到字典里的过程中有冲突发生的话，这两个键出现在字典里的顺序是不一样的。

往字典里添加新键可能会改变已有键的顺序
不要对字典同时进行迭代和修改,导致新散列表中键的次序变化。


• 集合里的元素必须是可散列的。
• 集合很消耗内存。
• 可以很高效地判断元素是否存在于某个集合。
• 元素的次序取决于被添加到集合里的次序。
• 往集合里添加元素，可能会改变集合里已有元素的次序。



## 函数装饰器和闭包
装饰器的一个关键特性是，它们在被装饰的函数定义之后立即运行，函数装饰器在导入模块时立即执行


不过，多数装饰器会修改被装饰的函数。通常，它们会定义一个内部函数，然后将其返回，替换被装饰的函数。使用内部函数的代码几乎都要靠闭包才能正确运作。

dis 模块为反汇编 Python 函数字节码提供了简单的方式。

在 averager 函数中，series 是自由变量（free variable） 。这是一个技术术语，指未在本地作用域中绑定的变量，
```
>>> avg.__code__.co_varnames ('new_value', 'total')
>>> avg.__code__.co_freevars ('series',)
```
闭包是一种函数，它会保留定义函数时存在的自由变量的绑定，这样调用函数时，虽然定义作用域不可用了，但是仍能使用那些绑定。

但是对数字、字符串、元组等不可变类型来说，只能读取，不能更新。如果尝试重新绑定，例如 count = count + 1，其实会隐式创建局部变量 count。这样，count 就不是自由变量了，因此不会保存在闭包中。


Python 2 没有 nonlocal，因此需要变通方法， “PEP 3104—Access to Names in Outer Scopes” （nonlocal 在这个 PEP 中引入，http://www.python.org/dev/peps/ pep-3104/）中的第三个代码片段给出了一种方法。基本上，这种处理方式是把内部函数需要修改的变量（如 count 和 total）存储为可变对象（如字典或简单的实例）的元素或属性，并且把那个对象绑定给一个自由变量。

 使用 functools.wraps 装饰器把相关的属性从 func 复制到 clocked 中。此外，这个新版还能正确处理关键字参数。

 内置了三个用于装饰方法的函数：property、classmethod 和 staticmethod。

 functools.lru_cache(maxsize=128, typed=False) maxsize 参数指定存储多少个调用的结果。
 maxsize 应该设为 2 的幂。typed 参数如果设为 True，把不同参数类型得到的结果分开保存，即把通常认为相等的浮点数和整数参数（如 1 和 1.0）区分开。
 被 lru_cache 装饰的函数，它的所有参数都必须是可散列的。

 @singledispatch 标记处理 object 类型的基函数。
各个专门函数使用 @«base_function».register(«type») 装饰。

Python 把被装饰的函数作为第一个参数传给装饰器函数。那怎么让装饰器接受其他参数呢？答案是：创建一个装饰器工厂函数，把参数传给它，返回一个装饰器，然后再把它应用到要装饰的函数上。

## 迭代
解释器需要迭代对象 x 时，会自动调用 iter(x)。
内置的 iter 函数有以下作用。
(1) 检查对象是否实现了 __iter__ 方法，如果实现了就调用它，获取一个迭代器。
(2) 如果没有实现 __iter__ 方法，但是实现了 __getitem__ 方法，Python 会创建一个迭代器，尝试按顺序（从索引 0 开始）获取元素。
(3) 如果尝试失败，Python 抛出 TypeError 异常，通常会提示“C object is not iterable” （C 对象不可迭代） ，其中 C 是目标对象所属的类。

可迭代的对象使用 iter 内置函数可以获取迭代器的对象。如果对象实现了能返回迭代器的 __iter__ 方法，那么对象就是可迭代的。序列都可以迭代；实现了 __getitem__ 方法，而且其参数是从零开始的索引，这种对象也可以迭代。

__next__ 返回下一个可用的元素，如果没有元素了，抛出 StopIteration 异常。
__iter__ 返回 self，以便在应该使用可迭代对象的地方使用迭代器，例如在 for 循环中。

it = iter(s)
while True:
...     try:
...         print(next(it))  
...     except StopIteration:  
...         del it  
...         break  


检查对象 x 是否为迭代器最好的方式是调用 isinstance(x, abc.Iterator)。

迭代器迭代器是这样的对象：
实现了无参数的 __next__ 方法，返回序列中的下一个元素；如果没有元素了，那么抛出 StopIteration 异常。Python 中的迭代器还实现了 __iter__ 方法，因此迭代器也可以迭代。

这一模式正确的实现方式是，每次调用 iter(my_ iterable) 都新建一个独立的迭代器。
可迭代的对象一定不能是自身的迭代器。也就是说，可迭代的对象必须实现__iter__ 方法，但不能实现 __next__ 方法。
另一方面，迭代器应该一直可以迭代。迭代器的 __iter__ 方法应该返回自身。

定义体中有 yield 关键字，该函数就是生成器函数。

for循环也是




















a
