
|                    |                    | Built-in Functions |                    |                     |
|:------------------ |:------------------:|:------------------:|:------------------:| -------------------:|
|      abs()         |       dict()       |       help()       |       min()        |       setattr()     |
|      all()         |       dir()        |       hex()        |       next()       |       slice()       |
|      any()         |       divmod()     |       id()         |       object()     |       sorted()      |
|      ascii()       |       enumerate()  |       input()      |       oct()        |       staticmethod()|
|      bin()         |       eval()       |       int()        |       open()       |       str()         |
|      bool()        |       exec()       |       isinstance() |       ord()        |       sum()         |
|      bytearray()   |       filter()     |       issubclass() |       pow()        |       super()       |
|      bytes()       |       float()      |       iter()       |       print()      |       tuple()       |
|      callable()    |       format()     |       len()        |       property()   |       type()        |
|      chr()         |       frozenset()  |       list()       |       range()      |       vars()        |
|      classmethod() |       getattr()    |       locals()     |       repr()       |       zip()         |
|      compile()     |       globals()    |       map()        |       reversed()   |       \_\_import\_\_()  |
|      complex()     |       hasattr()    |       max()        |       round()      |                     |
|      delattr()     |       hash()       |       memoryview() |       set()        |                     |


===

* **all(iterable)**

如果所有可迭代對象的元素為真，就返回 True，否則就返回 False 。 簡單說，可迭代對象的元素為 非0 就為 True

```
>>> all([0])
False
>>> all([1])
True
>>> all([-1])
True
```

* **any(iterable)**

如果可迭代對象的任意元素不為空，就返回 True， 否則就返回 False 。 

```
>>> any([])
False
>>> any([0])
False
>>> any([1])
True
>>> any([1, 0])
True
>>> any([-1, 0])
True
```

* **ascii(object)**

返回一個對象包含可打印表示的字符串。

```
>>> a = ascii([1, 2, 3])
>>> print(type(a), [a])
<class 'str'> ['[1, 2, 3]']
```

* **bin(x)**

把十進制轉成二進制

```
>>> bin(1)
'0b1'
>>> bin(2)
'0b10'
>>> bin(4)
'0b100'
>>> bin(8)
'0b1000'
>>> bin(255)
'0b11111111'
```

* **bool(x)**

用來判斷 True and False 用的， 空列表和空字典都為 False

```
>>> bool(0)
False
>>> bool(1)
True
>>> bool(-1)
True
>>> bool([])
False
>>> bool([1, 2, 3])
True
>>> bool({})
False
>>> bool({'name':'iron'})
True
```

* **bytes() 和 bytearray()**

講 bytearray 這個內置方法之前，我們先來複習一下 bytes ，請觀察資訊

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

a = bytes("abce", encoding="utf-8")
print(a, a.capitalize())

---------------執行結果---------------
b'abce' b'Abce'

Process finished with exit code 0
```

還記得先前曾提過，字符串是不能修改的，所以字節更是不能修改的，但如果想要修改的話，就得生成一個新的字符串或是字節。

**`bytearray`** 是個可修改的二進制字節，請觀察資訊

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

b = bytearray("abc", encoding="utf-8")
print(b[0])
print(b[1])
b[0] = 98
print(b)

---------------執行結果---------------
97
98
bytearray(b'bbc')

Process finished with exit code 0
```

由上觀察得知， bytearray 把二進制變成一個數組，這樣就可以去做修改，但這內置方法較不常用，知道就行了。

* **callable(object)**

判斷列表可以加括號嗎？ 加括號就是代表可調用的，請觀察資訊

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

print(callable([]))

---------------執行結果---------------
False

Process finished with exit code 0
```

那函數可以加括號嗎？ 加括號就是代表可調用的，請觀察資訊

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def sayhi():
    pass
print(callable(sayhi))

---------------執行結果---------------
True

Process finished with exit code 0
```

* **chr(i) 和 ord()**

chr() 會返回 ASCII 碼對應表， ord() 則是反查 ASCII 碼對應表中的數字

```
>>> chr(97)
'a'
>>> chr(98)
'b'
>>> ord('a')
97
>>> ord('b')
98
```

* **compile()** 

基本上是用不到，主要是在底層時，把代碼進行編譯。

```
>>> code = "for i in range(5): print(i)"
>>> compile(code, '', 'exec')
<code object <module> at 0x1021330c0, file "", line 1>
>>> code
'for i in range(10): print(i)'
>>> exec(code)
0
1
2
3
4
>>>
>>> code_num = "1+3/2*6"
>>> n = compile(code_num, '', 'eval')
>>> eval(n)
10.0
```

也可以寫成下面這樣的代碼

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

code = '''
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        # print(b)
        yield b
        a, b = b, a + b
        n = n + 1
    return '出現異常，return想打印什麼就打印什麼'


f = fib(10)
while True:
    try:
        x = next(f)
        print('f:', x)
    except StopIteration as e:
        print("Generator return value:", e.value)
        break
'''

py_obj = compile(code, "err.log", "exec")
exec(py_obj)

---------------執行結果---------------
f: 1
f: 1
f: 2
f: 3
f: 5
f: 8
f: 13
f: 21
f: 34
f: 55
Generator return value: 出現異常，return想打印什麼就打印什麼

Process finished with exit code 0
```

也可以直接用 `exec()`

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

code = '''
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        # print(b)
        yield b
        a, b = b, a + b
        n = n + 1
    return '出現異常，return想打印什麼就打印什麼'


f = fib(10)
while True:
    try:
        x = next(f)
        print('f:', x)
    except StopIteration as e:
        print("Generator return value:", e.value)
        break
'''

exec(code)

---------------執行結果---------------
f: 1
f: 1
f: 2
f: 3
f: 5
f: 8
f: 13
f: 21
f: 34
f: 55
Generator return value: 出現異常，return想打印什麼就打印什麼

Process finished with exit code 0
```

* **delattr(object, name)**

這個等到面向對象的時候，再做詳解，這個很有用。

* **dir()**

這個是拿來查函數有什麼方法可以用的。

```
>>> a = {1,2,3,4,5}
>>> dir(a)
['__and__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__iand__', '__init__', '__init_subclass__', '__ior__', '__isub__', '__iter__', '__ixor__', '__le__', '__len__', '__lt__', '__ne__', '__new__', '__or__', '__rand__', '__reduce__', '__reduce_ex__', '__repr__', '__ror__', '__rsub__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__xor__', 'add', 'clear', 'copy', 'difference', 'difference_update', 'discard', 'intersection', 'intersection_update', 'isdisjoint', 'issubset', 'issuperset', 'pop', 'remove', 'symmetric_difference', 'symmetric_difference_update', 'union', 'update']
>>>
```

* **divmod(a, b)**

用來取餘數的。

```
>>> divmod(10, 2)
(5, 0) → 第一個數字是『商數』，第二個數字是『餘數』
>>> divmod(10, 3)
(3, 1)
>>>
>>> (10 // 2) → 用來取商數
5
>>> (10 % 2) → 用來取餘數
0
```

* **eval(expression, globals=None, locals=None)**

把字符串變成一個字典。

```
>>> x = 1
>>> eval('x+1')
2
```

* **filter(function, iterable)**

講 `filter()` 之前，先提一下匿名函數。

一般我們在寫函數會像下面的代碼

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def sayhi(n):
    print(n)

sayhi(3)

---------------執行結果---------------
3

Process finished with exit code 0
```

而匿名函數就會像是下面的代碼

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

calc = lambda n:print(n)
calc(5)

---------------執行結果---------------
5

Process finished with exit code 0
```

但其實寫成匿名函數還是有一些限制，只能寫簡單的三元運算，太複雜的判斷都沒辦法做，簡單的三元運算如下

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

calc = lambda n:3 if n>4 else n
calc(5)

---------------執行結果---------------
3

Process finished with exit code 0
```

而 **filter()** 就是在一堆數據中，去過濾出自已想要的值

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

res = filter(lambda n:n>5,range(10))

for i in res:
    print(i)
    
---------------執行結果---------------
6
7
8
9

Process finished with exit code 0
```

上面這段代碼，就是在 range(10) 中，大於 5 的把它給打印出來。

* **map(function, iterable, ...)**

另一個 **map()** 就是對傳入的數值做處理

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

res = map(lambda n:n*2, range(10))  # 等同於列表生成式： [ i*2 for i in range(10) ] ，效果是一樣的。

for i in res:
    print(i)
    
---------------執行結果---------------
0
2
4
6
8
10
12
14
16
18

Process finished with exit code 0    
```

* **functools.reduce(function, iterable[, initializer])**

有 map 函數，那就介紹一下 **reduce 函數**，在 Python 2.7 版本中， recuce 函數是內置函數，但在 **`Python3 版本中，已經移到 functools 這個標準庫裡了`**。

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import functools

res = functools.reduce(lambda x, y:x*y, range(10))
print(res)

---------------執行結果---------------
45

Process finished with exit code 0
```

用 for 迴圈來實現相加

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

sum = 0
for i in range(10):
    sum += i
    print(sum)

---------------執行結果---------------
0
1
3
6
10
15
21
28
36
45

Process finished with exit code 0  
```

再用 **reduce 函數**來實現階乘的效果。

整數的階乘（英語：factorial）是所有小於及等於該數的正整數的積，0的階乘為1。 即：n!=1×2×3×...×n。

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import functools

res = functools.reduce(lambda x, y:x*y, range(1, 10))
print(res)

---------------執行結果---------------
362880

Process finished with exit code 0  
```

使用 for 迴圈來實現階乘。

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

num = int(input("請輸入一個數字: "))
factorial = 1

# 檢查數字是否為 0, 負數, 整數
if num < 0:
    print("負數沒有階乘")
elif num == 0:
    print("0 的階乘為 1")
else:
    for i in range(1, num + 1):
        factorial = factorial * i
    print("{} 的階乘為 {}".format(num, factorial))

---------------執行結果---------------
請輸入一個數字: 9
9 的階乘為 362880

Process finished with exit code 0  
```

其實還可以更簡洁的寫法，就是 **import math**。

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import  math

num = int(input("請輸入一個數字: "))

if num < 0:
    print("負數沒有階乘")
else:
    print("{} 的階乘為 {}".format(num, math.factorial(num)))

---------------執行結果---------------   
請輸入一個數字: 9
9 的階乘為 362880

Process finished with exit code 0  
```

**frozenset()**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

a  = frozenset([1, 2, 3, 4, 5, 6, 7, 6, 4, 2])
print(a)

---------------執行結果---------------   
frozenset({1, 2, 3, 4, 5, 6, 7})

Process finished with exit code 0  
```

其實看到 `frozen` 這個字眼，就知道是凍結，意思也就是說 **frozenset 函數**是不可以變動的。

* **getattr(object, name[, default])**

這個等到面向對象的時候，再做詳解。

* **globals()**

返回一個字典，返回的是當前這整個程序的所有變量的 key-value 的格式。

* **hash(object)**

python 字典內部就是這樣找出映射的對應關係。

* **hex(x)**

把一個整數轉成 16 進制

```
>>> hex(255)
'0xff'
>>> hex(-255)
'-0xff'
>>> hex(-42)
'-0x2a'
```

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

for i in range(1, 50):
    print(hex(i))
```

請執行上面試代碼，並觀察

* **locals()**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test():
    local_var = 123

print(globals())
print(globals().get('local_var'))
```

執行上面代碼後，肯定找不到 `local_var` ，因為 `globals` 只打印全局變量，不打印局部變量，所以要打印出局部變量，則需要使用 `print(locals())`，並且去調用 `test()`

```
def test():
    local_var = 123
    print(locals())
test()
print(globals())
print(globals().get('local_var'))
```

* **oct(x)**

把一個整數轉成 8 進制

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

for i in range(1, 17):
    print(i, oct(i))
```

請執行上代碼，並觀察

* **pow(x, y[, z])**

計算 x 的 y 次方，如果 z 存在，就會再對結果做一次計算，效果等同於 pow(x, y) % z)

**注意: 如果是用內置方法的 pow() 得到結果為 int ，若是使用 math.pow() 得到結果為 float**

```
>>> pow(2, 8)
256
>>> math.pow(2, 8)
256.0
>>> print(type(pow(2, 8)))
<class 'int'>
>>> import math
>>> print(type(math.pow(2, 8)))
<class 'float'>
```

* **repr(object)**

返回一個包含對象可打印表示的字符串

```python3
# 先定義 a = "Hello, World\n" 
>>> a = "Hello, World\n"
# 輸出 a 原有的樣子
>>> a
'Hello, World\n'
# 用 str() 函數輸出 a ，得到一個字串
>>> str(a)
'Hello, World\n'
# 用 repr() 函數輸出 a ，獲得機器閱讀的形式，也就是這變量實際上長的樣子
>>> repr(a)
"'Hello, World\\n'"
# 用 print() 函數加工輸出，將轉義字符進行轉義
>>> print(a)
Hello, World

# 對 str() 函數的返回值進行 print() 函數的加工輸出，得到與 print(a) 相同的結果
>>> print(str(a))
Hello, World

# 對 rper() 函數的返回值進行 print() 函數的加工輸出，得到與 a 和 str(a) 相同的結果
>>> print(repr(a))
'Hello, World\n'
# 對 rper() 函數的返回值傳給 eval() 函數，得到與 a 和 str(a) 相同的結果
>>> eval(repr(a))
'Hello, World\n'
```

用 `eval()` 函數可返回相對應的對象，請執行上面代碼後，並觀察結果

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

a = 1
result = a==eval(repr(a))
print("a={} 型態: {} 等於 evel(repr(a))={} 型態 {}，是否成立: {}".format(a, type(a), eval(repr(a)), type(eval(repr(a))), result))

a = 'Hello World'
result = a==eval(repr(a))
print("a={} 型態: {} 等於 evel(repr(a))={} 型態 {}，是否成立: {}".format(a, type(a), eval(repr(a)), type(eval(repr(a))), result))

a = 1.234
result = a==eval(repr(a))
print("a={} 型態: {} 等於 evel(repr(a))={} 型態 {}，是否成立: {}".format(a, type(a), eval(repr(a)), type(eval(repr(a))), result))

a = [1, 2, 3, 4, 5]
result = a==eval(repr(a))
print("a={} 型態: {} 等於 evel(repr(a))={} 型態 {}，是否成立: {}".format(a, type(a), eval(repr(a)), type(eval(repr(a))), result))

a = (6, 7, 8, 9, 10)
result = a==eval(repr(a))
print("a={} 型態: {} 等於 evel(repr(a))={} 型態 {}，是否成立: {}".format(a, type(a), eval(repr(a)), type(eval(repr(a))), result))

a = set({11, 12, 13, 14, 15})
result = a==eval(repr(a))
print("a={} 型態: {} 等於 evel(repr(a))={} 型態 {}，是否成立: {}".format(a, type(a), eval(repr(a)), type(eval(repr(a))), result))

a = {'x':1, 'y':1}
result = a==eval(repr(a))
print("a={} 型態: {} 等於 evel(repr(a))={} 型態 {}，是否成立: {}".format(a, type(a), eval(repr(a)), type(eval(repr(a))), result))

---------------執行結果---------------
a=1 型態: <class 'int'> 等於 evel(repr(a))=1 型態 <class 'int'> 是否成立: True
a=Hello World 型態: <class 'str'> 等於 evel(repr(a))=Hello World 型態 <class 'str'> 是否成立: True
a=1.234 型態: <class 'float'> 等於 evel(repr(a))=1.234 型態 <class 'float'> 是否成立: True
a=[1, 2, 3, 4, 5] 型態: <class 'list'> 等於 evel(repr(a))=[1, 2, 3, 4, 5] 型態 <class 'list'> 是否成立: True
a=(6, 7, 8, 9, 10) 型態: <class 'tuple'> 等於 evel(repr(a))=(6, 7, 8, 9, 10) 型態 <class 'tuple'> 是否成立: True
a={11, 12, 13, 14, 15} 型態: <class 'set'> 等於 evel(repr(a))={11, 12, 13, 14, 15} 型態 <class 'set'> 是否成立: True
a={'x': 1, 'y': 1} 型態: <class 'dict'> 等於 evel(repr(a))={'x': 1, 'y': 1} 型態 <class 'dict'> 是否成立: True

Process finished with exit code 0  
```

用 class 去定義 repr() 函數方法來控制此函數實例返回的內容

**`__repr__`** is more for developers while **`__str__`** is for end users.

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

class Point:
    def __init__(self, x, y):
        self.x, self.y = x, y
    def __repr__(self):
        return 'Point(x=%s, y=%s)' % (self.x, self.y)

p = Point(1, 2)
print(str(p))

---------------執行結果---------------
Point(x=1, y=2)

Process finished with exit code 0  
```

* **round(number[, ndigits])**

`round()` 函數是用來取小數點下第幾位，預設是 **None**，若是採用預設值，返回值則為整數，反之則為浮點數。

```
>>> round(1.2345)
1
>>> round(1.2345, 1)
1.2
>>> round(1.2345, 2)
1.23
>>> round(1.2345, 3)
1.234
>>> round(1.2345, 4)
1.2345
>>> round(1.2345, 5)
1.2345
>>> type(round(1.2345))
<class 'int'>
>>> type(round(1.2345, 2))
<class 'float'>
```

* **class slice(start, stop[, step])**

就是切片，可以觀察一下面的代碼，會發現 **`d[slice(2, 5)] = d[2:5]`**，這樣的切片方式。

```
>>> slice(2, 5)
slice(2, 5, None)
>>> d = range(50)
>>> d
range(0, 50)
>>> d[slice(2, 5)]
range(2, 5)
>>> d[2:5]
range(2, 5)
```

* **sorted(iterable, \*, key=None, reverse=False)**

按照 KEY 來排序

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

a = { -1:0, 6:3, 2:1, 4:7 }
print("字典是無序：", a)
print("按照預設值 key 來排序：", sorted(a))
print("按照 key 來排序：", sorted(a.items()))
print("按照 value 來排序：", sorted(a.items(), key=lambda  x:x[1]))

---------------執行結果---------------
按照預設值 key 來排序： [-1, 2, 4, 6]
按照 key 來排序： [(-1, 0), (2, 1), (4, 7), (6, 3)]
按照 value 來排序： [(-1, 0), (2, 1), (6, 3), (4, 7)]
字典是無序： {-1: 0, 6: 3, 2: 1, 4: 7}

Process finished with exit code 0  
```

* **zip(\*iterables)**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

a = [1, 2, 3, 4, 5, 6, 7]
b = [9, 2, 8, 10, 3, 9]

for i in zip(a, b):
    print(i, type(i))
    
---------------執行結果---------------
(1, 9) <class 'tuple'>
(2, 2) <class 'tuple'>
(3, 8) <class 'tuple'>
(4, 10) <class 'tuple'>
(5, 3) <class 'tuple'>
(6, 9) <class 'tuple'>

Process finished with exit code 0  
```

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

a = [1, 2, 3, 4, 5, 6, 7]
b = [9, 2, 8, 10, 3, 9]

for i in map(lambda pair: max(pair), zip(a, b)):
    print(i, type(i))

---------------執行結果---------------
9 <class 'int'>
2 <class 'int'>
8 <class 'int'>
10 <class 'int'>
5 <class 'int'>
9 <class 'int'>

Process finished with exit code 0
```

* **__import__(name, globals=None, locals=None, fromlist=(), level=0)**

通常是用關鍵字去做 import

建立以下這樣的目錄結構

```
Pratice
├── decorator.py
├── 內置函數.py
```

寫一個 `decorator.py`

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


import time

# 寫一個裝飾器
def timmer(func):
    def warpper(*args, **kwargs):
        start_time = time.time()
        func()
        stop_time = time.time()
        print('the func run time is {}'.format(stop_time - start_time))
    return warpper

@timmer # 調用裝飾器
def hello():
    time.sleep(3)
    print('in the hello')

hello()
```

再寫一個 `內置函數.py`

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

# import decorator           # 通常都是用這個方法 import
__import__('decorator')      # 只知道某個關鍵字去做 import

---------------執行結果---------------
in the hello
the func run time is 3.0039420127868652

Process finished with exit code 0
```




Referenc:

* [Python3 - Built-in Functions](https://docs.python.org/3/library/functions.html)
* [Python3 階乘實例](http://www.runoob.com/python3/python3-factorial.html)
* [Python中关于str（）函数和repr（）函数的那些事](https://blog.csdn.net/WIinter_FDd/article/details/78063985)
* [repr與str雜談———暴風雨前的輕鬆小品技術文](https://ithelp.ithome.com.tw/articles/10194593)
* [改变对象的字符串显示](http://python3-cookbook.readthedocs.io/zh_CN/latest/c08/p01_change_string_representation_of_instances.html)
* [Purpose of Python's __repr__](https://stackoverflow.com/questions/1984162/purpose-of-pythons-repr)
* [Python's zip, map, and lambda](https://bradmontgomery.net/blog/pythons-zip-map-and-lambda/)

