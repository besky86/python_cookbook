# 保留最新的N个元素

__使用collections.deque可以实现__

当创建了固定容量的deque之后，在容量已满的情况下，再插入数据，会自动移除对应个数的最旧的数据元素：
```
>>> q = deque(maxlen=3)
>>> q.append(1)
>>> q.append(2)
>>> q.append(3)
>>> q
deque([1, 2, 3], maxlen=3)
>>> q.append(4)
>>> q
deque([2, 3, 4], maxlen=3)
>>> q.append(5)
>>> q
deque([3, 4, 5], maxlen=3)
```


# 获取最大或最小的N个元素

__使用heapq解决__
```
import heapq
nums = [1, 8, 2, 23, 7, -4, 18, 23, 42, 37, 2]
print(heapq.nlargest(3, nums)) # Prints [42, 37, 23]
print(heapq.nsmallest(3, nums)) # Prints [-4, 1, 2]
```

也可以使用key:
```
portfolio = [
{'name': 'IBM', 'shares': 100, 'price': 91.1},
{'name': 'AAPL', 'shares': 50, 'price': 543.22},
{'name': 'FB', 'shares': 200, 'price': 21.09},
{'name': 'HPQ', 'shares': 35, 'price': 31.75},
{'name': 'YHOO', 'shares': 45, 'price': 16.35},
{'name': 'ACME', 'shares': 75, 'price': 115.65}
]
cheap = heapq.nsmallest(3, portfolio, key=lambda s: s['price'])
expensive = heapq.nlargest(3, portfolio, key=lambda s: s['price'])
```

使用heapq生成的堆是__最小堆__, 第一个元素永远最小：
```
>>> nums = [1, 8, 2, 23, 7, -4, 18, 23, 42, 37, 2]
>>> import heapq
>>> heap = list(nums)
>>> heapq.heapify(heap)
>>> heap
[-4, 2, 1, 23, 7, 2, 18, 23, 42, 37, 8]
>>> heapq.heappop(heap)
-4
>>> heapq.heappop(heap)
1
>>> heapq.heappop(heap)
2
```


# 实现优先级队列
每个元素都有个优先级，每次弹出返回的都是优先级最大的元素


简单实现：
```
import heapq
class PriorityQueue:
    def __init__(self):
        self._queue = []
        self._index = 0

    def push(self, item, priority):
        heapq.heappush(self._queue, (-priority, self._index, item))
        self._index += 1

    def pop(self):
        return heapq.heappop(self._queue)[-1]
```

```
>>> class Item:
    ... def __init__(self, name):
    ... self.name = name
    ... def __repr__(self):
    ... return 'Item({!r})'.format(self.name)
    ...
>>> q = PriorityQueue()
>>> q.push(Item('foo'), 1)
>>> q.push(Item('bar'), 5)
>>> q.push(Item('spam'), 4)
>>> q.push(Item('grok'), 1)
>>> q.pop()
Item('bar')
>>> q.pop()
Item('spam')
>>> q.pop()
Item('foo')
>>> q.pop()
Item('grok')
>>>
```

1. -priority是因为heapq是最小堆，所以需要转换为最大堆。
2. 数组是可以比较的，但是，如果同位置的元素无法比较，则会报错，所以，在实例中用index避免了这一点
```
>>> a = (1, Item('foo'))
>>> b = (5, Item('bar'))
>>> a < b
True
>>> c = (1, Item('grok'))
>>> a < c
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: unorderable types: Item() < Item()
>>>
```
```
>>> a = (1, 0, Item('foo'))
>>> b = (5, 1, Item('bar'))
>>> c = (1, 2, Item('grok'))
>>> a < b
True
>>> a < c
```

# 如何存储默认字典值
__collections.defaultdict__
```
d = defaultdict(list)
d['a'].append(1)
d['a'].append(2)
d['b'].append(4)
```


# 排序的字典
__OrderedDict__
```
from collections import OrderedDict
d = OrderedDict()
d['foo'] = 1
d['bar'] = 2
d['spam'] = 3
d['grok'] = 4
# Outputs "foo 1", "bar 2", "spam 3", "grok 4"
for key in d:
print(key, d[key])
```

在Ordereddict中，为了保持插入顺序，会有一个list来记录,因此，Ordereddict是普通的dict大小的两倍。如果在实现大数据量的需求时，使用Ordereddict就要考虑是否会超出内存容量。



# 合理利用tuple的可比较属性


# 找出两个dict的公共元素
dict的三个方法：
1. keys(): 可以执行集合类似的操作，交集，并集，差集合.
2. items(): 返回键值对视图，也有集合类似的操作，可以找出公共元素.
3. values(): 返回值得视图，由于值可以重复，因此，不能进行集合操作，如果实在需要集合操作，需要把返回的视图转化成set.

# 删除重复元素, 并保持顺序

1. 简单实现
```
def dedupe(items):
    seen = set()
    for item in items:
    if item not in seen:
        yield item
        seen.add(item)
```

这个版本无法在unhashable的元素上使用,可以使用兼容版本
2. 兼容形式：
```
def dedupe(items, key=None):
    seen = set()
    for item in items:
        val = item if key is None else key(item)
        if val not in seen:
            yield item
            seen.add(val)
```


# 使用slice来切分
slice()是内置函数，会创建一个slice对象
```
>>> items = [0, 1, 2, 3, 4, 5, 6]
>>> a = slice(2, 4)
>>> items[2:4]
[2, 3]
>>> items[a]
[2, 3]
>>> items[a] = [10,11]
>>> items
[0, 1, 10, 11, 4, 5, 6]
>>> del items[a]
>>> items
[0, 1, 4, 5, 6]
```

# 找出出现次数最多的数据：
使用counter:
```
words = [
'look', 'into', 'my', 'eyes', 'look', 'into', 'my', 'eyes',
'the', 'eyes', 'the', 'eyes', 'the', 'eyes', 'not', 'around', 'the',
'eyes', "don't", 'look', 'around', 'the', 'eyes', 'look', 'into',
'my', 'eyes', "you're", 'under'
]
from collections import Counter
word_counts = Counter(words)
top_three = word_counts.most_common(3)
print(top_three)
# Outputs [('eyes', 8), ('the', 5), ('look', 4)]
```
Counter有update函数，如dict.update一样


# 给元素为dict的list排序
使用operator.itemgetter()函数

> It can be a dictionary key name, a
  numeric list element, or any value that can be fed to an object’s __getitem__() method.

如果itemgetter的参数有多个，那么，返回的是对应值组成的数组


# 以某个字段聚合记录
方案:`itertools.groupby() function`
```
rows = [
    {'address': '5412 N CLARK', 'date': '07/01/2012'},
    {'address': '5148 N CLARK', 'date': '07/04/2012'},
    {'address': '5800 E 58TH', 'date': '07/02/2012'},
    {'address': '2122 N CLARK', 'date': '07/03/2012'},
    {'address': '5645 N RAVENSWOOD', 'date': '07/02/2012'},
    {'address': '1060 W ADDISON', 'date': '07/02/2012'},
    {'address': '4801 N BROADWAY', 'date': '07/01/2012'},
    {'address': '1039 W GRANVILLE', 'date': '07/04/2012'},
]
from operator import itemgetter
from itertools import groupby
# Sort by the desired field first
rows.sort(key=itemgetter('date'))
# Iterate in groups
for date, items in groupby(rows, key=itemgetter('date')):
    print(date)
    for i in items:
    print(' ', i)
```
__使用这个函数必须先排好序__

# 筛选
filter(), compress()
> The compress() function then picks out the items corresponding to True values.
```
addresses = [
    '5412 N CLARK',
    '5148 N CLARK',
    '5800 E 58TH',
    '2122 N CLARK'
    '5645 N RAVENSWOOD',
    '1060 W ADDISON',
    '4801 N BROADWAY',
    '1039 W GRANVILLE',
]
counts = [ 0, 3, 10, 4, 1, 7, 6, 1]
>>> from itertools import compress
>>> more5 = [n > 5 for n in counts]
>>> more5
[False, False, True, False, False, True, True, False]
>>> list(compress(addresses, more5))
['5800 E 58TH', '4801 N BROADWAY', '1039 W GRANVILLE']
>>>
```

# 对元素列表进行获取时使用name：
__使用namedtuple__
```
>>> from collections import namedtuple
>>> Subscriber = namedtuple('Subscriber', ['addr', 'joined'])
>>> sub = Subscriber('jonesy@example.com', '2012-10-19')
>>> sub
Subscriber(addr='jonesy@example.com', joined='2012-10-19')
>>> sub.addr
'jonesy@example.com'
>>> sub.joined
'2012-10-19'
>>>
```
对于依赖数据结构的数据，及时使用namedtuple。相对于dict来说，namedtuple不可修改，immutable, 如果实在需要修改数据，使用namedtuple的_replace方法：
```
>>> s = Stock('ACME', 100, 123.45)
>>> s
Stock(name='ACME', shares=100, price=123.45)
>>> s.shares = 75
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
AttributeError: can't set attribute
>>>
>>> s = s._replace(shares=75)
>>> s
Stock(name='ACME', shares=75, price=123.45)
>>>
```
