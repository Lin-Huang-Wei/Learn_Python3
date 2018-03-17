
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

* ** callable(object)**

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

