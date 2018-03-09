### 裝飾器 - 小高潮

請使用下面的代碼，再不修改源代碼下，替 foo 函數跟 bar 函數新增一個功能

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import time

def foo():
    time.sleep(3)
    print('in the foo')

def bar():
    time.sleep(3)
    print('int the bar')

foo()
bar()
```
還記得之前說的 [Python 函數與常用模組 - 1 - 裝飾器前奏(高階函數)](./Python%20函數與常用模組%20-%201%20-%20裝飾器前奏(高階函數).md) 的規則嗎？

* 規則1: `在不修改被裝飾函數源代碼的情況下，為它加上新的功能`
* 規則2: `不能修改被裝飾的函數的調用方式`

首先，定義一個函數，來做為新功能的實現。

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import time

def deco(func):
    start_time = time.time()
    func()
    stop_time = time.time()
    print("in the func run time is {}".format(stop_time - start_time))

def foo():
    time.sleep(3)
    print('in the foo')

def bar():
    time.sleep(3)
    print('int the bar')

# deco(foo()) 
# deco(bar())
# 這樣的寫法的錯的，因為如果是加了小括號()代表是把這個函數的執行結果的返回值給傳進去了，所以這樣的寫法是不行的。

deco(foo)
deco(bar)

---------------執行結果---------------

in the foo
in the func run time is 3.0011279582977295
int the bar
in the func run time is 3.0044898986816406

Process finished with exit code 0
```

在上面的代碼中，聲明一個新的函數叫 `deco(func)`，主要是實現的功能是 `顯示程序執行多久時間`，調用方式使用 `deco(foo)`，這樣會把 `foo函數` 給帶進去 `deco(func)`，但使用這樣的方式，雖然有達成 `規則1: 在不修改被裝飾函數源代碼的情況下，為它加上新的功能`，卻違反了 `規則2: 不能修改被裝飾的函數的調用方式`，所以我們再修改一下代碼

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


import time

def deco(func):
    start_time = time.time()
    return func
    stop_time = time.time()
    print("in the func run time is {}".format(stop_time - start_time))

def foo():
    time.sleep(3)
    print('in the foo')

def bar():
    time.sleep(3)
    print('in the bar')

foo = deco(foo)
foo()
bar = deco(bar)
bar()

---------------執行結果---------------

in the foo
in the bar

Process finished with exit code 0
```

結果這上面代碼執行完後，有發現什麼問題嗎？沒錯，雖然有符合 `規則2: 不能修改被裝飾的函數的調用方式`了，但原本新增的功能卻不見了，這是為什麼呢？請看圖解，先看綠色的線

![裝飾器說明_1](./IMG/5.png)

那要怎麼把新功能給加上去？在加上新功能之前，還記得之前說是由[Python 函數與常用模組 - 1 - 裝飾器前奏(高階函數)](./Python%20函數與常用模組%20-%201%20-%20裝飾器前奏(高階函數).md) + [Python 函數與常用模組 - 2 -裝飾器前戲(嵌套函數)](./Python%20函數與常用模組%20-%202%20-裝飾器前戲(嵌套函數).md) => `裝飾器`，而上面的代碼都只有使用到高階函數，並沒有使用到嵌套函數，接下來就要使用嵌套函數去實現新功能。


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


import time

def timer(func):
    def deco():
        start_time = time.time()
        func()
        stop_time = time.time()
        print("in the func run time is {}".format(stop_time - start_time))
    return deco

def foo():
    time.sleep(3)
    print('in the foo')

def bar():
    time.sleep(3)
    print('in the bar')

print("打印return deco = timer(foo)的記憶體位址: {}".format(timer(foo)))
print("打印return deco = timer(bar)的記憶體位址: {}".format(timer(bar)))

foo = timer(foo)
foo()
bar = timer(bar)
bar()

---------------執行結果---------------

打印return deco = timer(foo)的記憶體位址: <function timer.<locals>.deco at 0x109e8e6a8>
打印return deco = timer(bar)的記憶體位址: <function timer.<locals>.deco at 0x109e8e6a8>
in the foo
in the func run time is 3.00457501411438
in the bar
in the func run time is 3.0016891956329346

Process finished with exit code 0
```

請看圖解

![裝飾器說明_2](./IMG/6.png)

首先，我們來檢視一下 `foo函數` 的代碼都沒有被修改，而且調用方式也沒有被修改，所以我們可以很確定地說，這已經完全符合 `裝飾器` 所有的規則。

但這樣每次都還要去運行 `foo = timer(foo)`，所以 Python 有提供了一個語法糖，只要在要 `foo函數` 前加一個 `@timer` 就行了

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


import time

def timer(func):
    def deco():
        start_time = time.time()
        func()
        stop_time = time.time()
        print("in the func run time is {}".format(stop_time - start_time))
    return deco

@timer  # foo = timer(foo)
def foo():
    time.sleep(3)
    print('in the foo')
@timer  # bar = timer(bar)
def bar():
    time.sleep(3)
    print('in the bar')

print("打印return deco = timer(foo)的記憶體位址: {}".format(timer(foo)))
print("打印return deco = timer(bar)的記憶體位址: {}".format(timer(bar)))

foo()
bar()

---------------執行結果---------------

打印return deco = timer(foo)的記憶體位址: <function timer.<locals>.deco at 0x10d743730>
打印return deco = timer(bar)的記憶體位址: <function timer.<locals>.deco at 0x10d743730>
in the foo
in the func run time is 3.0012028217315674
in the bar
in the func run time is 3.0002009868621826

Process finished with exit code 0
```

以上就是 `裝飾器` 的功能實現。

