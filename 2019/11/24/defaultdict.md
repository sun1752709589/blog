# Python中collections.defaultdict()使用

### 1、以一个例子开始：统计一个列表里各单词重复次数
```python
words = ['hello', 'world', 'nice', 'world']
counter = dict()
for kw in words:
    counter[kw] += 1
```
这样写肯定会报错的，因为各词的个数都没有初始值，引发KeyError

### 2、改进一下：加入if判断
```python
words = ['hello', 'world', 'nice', 'world']
counter = dict()
for kw in words:
    if kw in words:
        counter[kw] += 1
    else:
        counter[kw] = 0
```

### 3、再改进：使用setdefault()方法设置默认值
```python
words = ['hello', 'world', 'nice', 'world']
counter = dict()
for kw in words:
    counter.setdefault(kw, 0)
    counter[kw] += 1
```
setdefault()，需提供两个参数，第一个参数是键值，第二个参数是默认值，每次调用都有一个返回值，如果字典中不存在该键则返回默认值，如果存在该键则返回该值，利用返回值可再次修改代码。
```python
words = ['hello', 'world', 'nice', 'world']
counter = dict()
for kw in words:
    counter[kw] = counter.setdefault(kw, 0) + 1
```

### 4、接着改进
一种特殊类型的字典本身就保存了默认值defaultdict()，defaultdict类的初始化函数接受一个类型作为参数，当所访问的键不存在的时候，可以实例化一个值作为默认值。
```python
from collections import defaultdict
dd = defaultdict(list)
defaultdict(<type 'list'>, {})
#给它赋值，同时也是读取
dd['h'h]
defaultdict(<type 'list'>, {'hh': []})
dd['hh'].append('haha')
defaultdict(<type 'list'>, {'hh': ['haha']})
```
该类除了接受类型名称作为初始化函数的参数之外，还可以使用任何不带参数的可调用函数，到时该函数的返回结果作为默认值，这样使得默认值的取值更加灵活。
```python
>>> from collections import defaultdict
>>> def zero():
...     return 0
...
>>> dd = defaultdict(zero)
>>> dd
defaultdict(<function zero at 0xb7ed2684>, {})
>>> dd['foo']
0
>>> dd
defaultdict(<function zero at 0xb7ed2684>, {'foo': 0})
```
最终代码：
```python
from collections import defaultdict
words = ['hello', 'world', 'nice', 'world']
#使用lambda来定义简单的函数
counter = defaultdict(lambda: 0) 
for kw in words:
    counter[kw] += 1
```