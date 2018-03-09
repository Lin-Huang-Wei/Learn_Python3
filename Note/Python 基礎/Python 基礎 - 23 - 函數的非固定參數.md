### 函數的非固定參數

**默認參數**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test(x, y=2):
    print(x)
    print(y)

test(1)

---------------執行結果---------------

1
2

Process finished with exit code 0
```

觀察上面的代碼，調用函數時，少傳遞了一個值，但卻沒有 error message ，這就是`默認參數`
那調用默認參數時，也可以用下面二種方式來調用

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test(x, y=2):
    print(x)
    print(y)

test(1,3)    # 用的是位置參數調用

---------------執行結果---------------

1
3

Process finished with exit code 0
```

or

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test(x, y=2):
    print(x)
    print(y)

test(1,y=4)    # 用的是關鍵字參數調用

---------------執行結果---------------

1
4

Process finished with exit code 0
```

**默認參數的特點：**

`調用函數的時候，默認參數可有可無，不一定要傳值進去`

以上介紹的 `位置參數`、`關鍵字參數`、`默認參數`都有一個特點，如果在調用函數時，不能超過調用函數定義的個數，否則都會出錯，不管是多了一個參數或是少一個參數，那為了因應不確定 `實際參數`會有幾個時，那應該要怎麼定義 `形式參數`呢？這時候就可以使用第四種的函數類型 - `參數組`。

**參數組介紹**

參數組有分二種：

1. `fun1(*args): 接收 N 個位置參數，放進一個 tuple(元組) 的形式`
2. `fun2(**kwargs): 接收 N 個關鍵字參數，放進一個字典的形式`

定義一個參數組的函數

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test(*args):
    print(args)

test(1,2,3)       # 傳遞N個位置參數或叫實際參數轉成為 tuple(元組)
test(*[4,5,6])    # *args=tuple[4,5,6]，傳遞一個列表

---------------執行結果---------------

(1, 2, 3)
(4, 5, 6)

Process finished with exit code 0
```

觀察上面代碼，因為定義一個`形式參數為 *args`時，調用時的所有`實際參數`會通通放進一個 `tuple(元組)` 裡，`這樣的好處是，在傳遞實際參數時，就沒有了數量上的限制`。

那可不可以跟 `位置參數`一起使用呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test(x,*args):
    print(x)
    print(args)

test(1,2,3,4,5,6,7)

---------------執行結果---------------

1
(2, 3, 4, 5, 6, 7)

Process finished with exit code 0
```

觀察上面代碼，很明顯地可以發現是可以與 `位置參數`一起使用。

那如果想要傳遞的是一個字典，要怎麼做？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test(**kwargs):
    print(kwargs)

test(name='tony', age=20, sex='male')    # 傳遞 N 個關鍵字參數轉成字典的方式
test(**{'name':'ironman', 'age':25, 'sex':'male'})    #傳遞一個字典

---------------執行結果---------------

{'name': 'tony', 'age': 20, 'sex': 'male'}
{'name': 'ironman', 'age': 25, 'sex': 'male'}

Process finished with exit code 0
```

那如果想要調用出字典中的值，要怎麼做呢？

```
def test(**kwargs):
    print(kwargs)
    print(kwargs['name'])
    print(kwargs['age'])
    print(kwargs['sex'])

test(name='tony', age=20, sex='male')
test(**{'name':'ironman', 'age':25, 'sex':'male'})

---------------執行結果---------------

{'name': 'tony', 'age': 20, 'sex': 'male'}
tony
20
male
{'name': 'ironman', 'age': 25, 'sex': 'male'}
ironman
25
male

Process finished with exit code 0
```

那可不可以跟`位置參數`、`關鍵字參數`、`默認參數`一起使用？

**使用位置參數和\*\*kwargs一起使用**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test(name, **kwargs):
    print(name)
    print(kwargs)


test('tony')

---------------執行結果---------------

tony
{}

Process finished with exit code 0
```

觀察上面代碼，打印出 tony 和一個空字典。

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test(name, **kwargs):
    print(name)
    print(kwargs)


test('tony', 'stark')

---------------執行結果---------------

Traceback (most recent call last):
  File "/Python/project/path/function_4.py", line 8, in <module>
    test('tony', 'stark')
TypeError: test() takes 1 positional argument but 2 were given

Process finished with exit code 1
```

觀察上面代碼，發現 `test()` 函數只需要一個位置參數，但傳了二個位置參數進去，所以噴 error message 了，所以來修正一下這支代碼

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test(name, **kwargs):
    print(name)
    print(kwargs)
    print(kwargs['age'])


test('tony', age=20, sex='male')

---------------執行結果---------------

tony
{'age': 20, 'sex': 'male'}
20

Process finished with exit code 0
```

觀察上面代碼，就可以正確的把關鍵字參數放進一個字典裡了。

**使用默認參數和 \*\*kwargs 一起使用**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test(name, age=18,**kwargs):
    print(name)
    print(age)
    print(kwargs)

test('tony', sex='male')

---------------執行結果---------------

tony
18
{'sex': 'male', 'hobby': 'Potts'}

Process finished with exit code 0
```

觀察上面代碼

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test(name, age=18,**kwargs):
    print(name)
    print(age)
    print(kwargs)

print("====================第一種情況====================")
test('tony', sex='male', hobby='Potts')
print("====================第二種情況====================")
test('ironman',25, sex='none', hobby='none')
print("====================第三種情況====================")
test('Potts', age=22, sex='female', hobby='tony')

---------------執行結果---------------

====================第一種情況====================
tony
18
{'sex': 'male', 'hobby': 'Potts'}
====================第二種情況====================
ironman
25
{'sex': 'none', 'hobby': 'none'}
====================第三種情況====================
Potts
22
{'sex': 'female', 'hobby': 'tony'}

Process finished with exit code 0
```

觀察上面代碼，看到三種不同的調用方式，在 `默認參數`時，會有什麼不一樣？

1. age 採用默認參數，打印出 18
2. age 採用位置參數，打印出 25
3. age 採用關鍵字參數，打印出 22

那可不可以全部都一起用？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def test(name, age=18, *args, **kwargs):
    print(name)
    print(age)
    print(args)
    print(kwargs)

test('tony', age=32, sex='male', hobboy='Potts')
print("====================我是分隔線====================")
test('tony', 30, 4, 56, 7, 49, sex='male', hobby='Potts')

---------------執行結果---------------

====================第一種情況====================
tony
32
()
{'sex': 'male', 'hobboy': 'Potts'}
====================第二種情況====================
tony
30
(4, 56, 7, 49)
{'sex': 'male', 'hobby': 'Potts'}

Process finished with exit code 0
```

觀察上面代碼，看到二種不同的調用方式

1. age 採用關鍵字參數調用，蓋過原本的默認參數，因 *agrs 無法使用關鍵字參數調用，所以打印出空的 tuple(元組) ，後面 \*\*kwargs 採用關鍵字調用，所以就打印出字典的形式。
2. age 採用位置參數調用，蓋過原本的默認參數，因*args使用位置參數調用，所以打印出(4, 56, 7, 49)這個元組，後面 \*\*kwargs 採用關鍵字調用，所以就打印出字典的形式。


Reference:

* [python函数的四种参数传递方式](http://lazybios.com/2013/04/four-kinds-of-function-argment-pass-in-python/)