### Python reversed list
列表倒序
old:
```python
a = [1,2,3,4,5]
for i in range(len(a), 0, -1):
    print i
```
new:
```python
a = [1,2,3,4,5]
for i in reversed(a):
    print i
```

### Looping over a collection and indices
迭代两个数列，并且得到key ,value

old:
```python
a = [1,2,3,4,5]
for i in range(len(a)):
    print i '->' a[i]
```
new:
```python
a = [1,2,3,4,5]
for key, value in enumerate(a):
    print key, value
```

### Looping over two collection;
同时循环两个数列

old:
```python
a = ['a', 'b', 'c']
colors = ['red', 'blue', 'green', 'yellow', 'white']

length = min(a, colors)
for i in range(length):
    print a[i], '->', colors[i]
```

new:
```python
a = ['a', 'b', 'c']
colors = ['red', 'blue', 'green', 'yellow', 'white']

for a, color in zip(a, colors):
    print a, '->', color
```

### Custom sort order
自定义列表排序，比如按照内容长短

old:
```python
colors = ['red', 'blue', 'green', 'yellow', 'white']
def compare_length(c1, c2):
    if len(c1) > len(c2): return 1
    if len(c1) < len(c2): return -1
    return 0
print sorted(b, cmp=compare_length)
```
new:
```python
print sorted(b, key=len)
```

### call function until a sentinel value
调用函数直到某一个值

old:
```python
while True:
    with open('package-lock.json', 'r') as f:
        block = f.read(32)
        if block == '':
            break
        blocks.append(block)
```
new:
```python
blocks = []
for block in iter(partial(f.read, 32), ''):
    blocks.append(block)
```

### distinguishing multiple exit point in loop
在循环中有多个退出点(for else)

old:
```python
def find(seq, target):
    found = False
    for i, value in enumerate(seq):
        if value == target:
            found = True
            break
    if not found:
        return -1
    return 1
```
new:
```python
def find(seq, target):
    for i, value in enumerate(seq):
        if value == target:
            found = True
            break
    else:
        return -1
    return 1
```

### looping over dictionary key
循环字段键值(python3)

old:
```python
d = {'a':'b', 'c':'d'}
for k in d.keys():
    if k.startswith('r'):
        del d[k]
```
new:
```python
d = [k : d[k] for k in d if not k.startswith('c')]
```

### looping over dictionary key and value
循环迭代字典

old:
```python
d = {'a':'b', 'c':'d'}
for k in d:
    print k, '->', d[k]
```
new:
```python
d = {'a':'b', 'c':'d'}
for k, value in d.items():
    print k, '->', value
```

### construct a dictionary from pairs
构建字典

```python
# 两个列表构建字典
names = ['raymond', 'rachel', 'matthew']
colors = ['red', 'green', 'blue']
d = dict(zip(names, colors))

# 单个列表构建字典
d = dict(enumerate(numes))

```

### counting with dictionaries
字典计数

old:
```python
colors = ['red', 'green', 'red', 'blue', 'green', 'red']

# ex1
d = {}
for color in colors:
    if color not in d:
        d[color] = 0
    d[color] += 1

# ex2
d = {}
for color in colors:
    d[color] = d.get(color, 0) + 1

# ex3
from collections import defaultdict
d = defaultdict(int)
for color in colors:
    d[color] += 1

```
new:
```python
from collections import Counter
colors = ['red', 'green', 'red', 'blue', 'green', 'red']

d = Counter(colors)
```

### grouping with dictionaries
字典分组统计

old:
```python
names = ['raymond', 'rachel', 'matthew', 'roger', 'melissa', 'judith']
d = {}
for name in names:
    key = len(name)
    d.setdefault(key, []).append(name)


#In [1]: d
#Out[2]: {5: ['roger'], 6: ['rachel', 'judith'], 7: ['raymond', 'matthew', 'melissa']}

```
new:
```python
from collections import defaultdict


names = ['raymond', 'rachel', 'matthew', 'roger', 'melissa', 'judith']
d = defaultdict(list)
for name in names:
    key = len(name)
    d[key].append(name)
```

### is a dictionary popitem() atomic
字典pop原子操作
##### 什么是原子操作:
> 原子操作就是不会因为进程并发或者线程并发而导致被中断的操作。原子操作的特点就是要么一次全部执行，要么全不执行。不存在执行了一半而被中断的情况。

```python
d = {'judith': 'red', 'matthew': 'red', 'melissa': 'green', 'roger': 'blue'}
d.popitem()
```

### Updating multiple state variables
更新多个变量状态
old:
```python
def fibonacci(n):
    x = 0
    y = 1
    for i in range(n):
        print x
        t = y
        y = x + y
        x = t
```
new:
```python
def fibonacci(n):
    x, y = 0, 1
    for i in range(n):
        yiled x
        x, y = y, x + y
```

### Concatenating strings
字符串拼接
old:
```python
names = ['raymond', 'rachel', 'matthew', 'roger', 'melissa', 'judith']

s = names[0]
for name in names[1:]:
    s += ', ' + name
print s
```
new:
```python
names = ['raymond', 'rachel', 'matthew', 'roger', 'melissa', 'judith']

print ', '.join(names)
```

### Updating sequences
列表更新
old:
```python
names = ['raymond', 'rachel', 'matthew', 'roger', 'melissa', 'judith']

# delete
del names[0]
# pop
names.pop(0)
# insert
names.insert(0, 'eds')
```
new:
Deques对appendleft()和popleft()具有O(1)速度，list对插入和pop O(n) 
> Deques have O(1) speed for appendleft() and popleft() while lists have O(n) performance for insert(0, value) and pop(0).
```python
from collections import deque


names = ['raymond', 'rachel', 'matthew', 'roger', 'melissa', 'judith']

del names[0]
names.popleft()
# append
names.appendleft('eds')
```

### Using decorators to factor-out administrative logic
使用装饰器来分解管理函数逻辑

### How to open and close files
使用with 来打开文件
new:
```python
with open('./test.yml', 'r') as file:
    pass
```

### How to use locks
多线程如何使用锁

old:
```python
import threading


lock = threading.Lock()
lock.acquire()
try:
    print 'test1'
    print 'test2'
finally:
    lock.release()

```
new:
```python
import threading


lock = threading.Lock()
with lock:
    print 'test1'
    print 'test2'
```