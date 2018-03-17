### 生成器並行運算

目前我們已經大致上都了解生成器了，但要怎麼實際應用呢？！接下來就要舉個例子

**`yield`** 保存了這個函數的中斷狀態，返回當前這個狀態的值，並且把函數停在這，想什麼時候回來執行就什麼時候回來執行。

**通過 `yield` 實現單綫程的情況下，實現並發運算的效果**

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def consumer(name):
    print("%s 準備吃包子啦!" %name)
    while True:
       baozi = yield  # baozi 沒有返回值就會為none

       print("包子[%s]来了,被[%s]吃了!" %(baozi,name))
       
 c = consumer("Tony")
 c.__next__()
      
       
---------------執行結果---------------
Tony 準備吃包子啦!

Process finished with exit code 0
```

生成一個消費者叫 Tony ，其實上面代碼就是一個生成器，所以要用 `__next__()` 這個方法去調用，但只有一個 `__next__()` 方法，程序會停在 `baozi = yield` 就結束了，那下面的就不會去執行了，那如果要繼續執行下面的話，要怎麼做呢？！

其實很簡單，在調用一次 `__next__()` 方法，就可以了

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def consumer(name):
    print("%s 準備吃包子啦!" %name)
    while True:
       baozi = yield  # baozi 沒有返回值就會為 none

       print("包子[%s]来了,被[%s]吃了!" %(baozi,name))
       
 c = consumer("Tony")
 c.__next__()
 c.__next__()     
       
---------------執行結果---------------
Tony 準備吃包子啦!
包子[None]来了,被[Tony]吃了!

Process finished with exit code 0
```

耶！包子被打印出來了，注意，包子變成 none ，而這個 none 是代表包子為空，當然就沒有包子可以吃，那要怎麼做一個包子給 Tony ？

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

# import time
def consumer(name):
    print("%s 準備吃包子啦!" %(name))
    while True:
       baozi = yield

       print("包子[%s]来了,被[%s]吃了!" %(baozi,name))

c = consumer("Tony")
c.__next__()

b1 = "韭菜餡"
c.send(b1)

---------------執行結果---------------
Tony 準備吃包子啦!
包子[韭菜餡]来了,被[Tony]吃了!

Process finished with exit code 0
```

我們製作一個包子叫 `b1 = "韭菜餡"` ，並且透過 `send()` 這個方法來給 Tony 包子吃。

* `send()` 其實就是去調用 yield ，同時也給它傳一個值。
*  `__next__()` 就只是去調用 yield ，並不會傳值給 yield 。

看下面代碼就可以更清楚了解

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import time
def consumer(name):
    print("%s 準備吃包子啦!" %(name))
    while True:
       baozi = yield

       print("包子[%s]来了,被[%s]吃了!" %(baozi,name))

c = consumer("Tony")
c.__next__()

b1 = "韭菜餡"
c.send(b1)
c.__next__()

---------------執行結果---------------
Tony 準備吃包子啦!
包子[韭菜餡]来了,被[Tony]吃了!
包子[None]来了,被[Tony]吃了!

Process finished with exit code 0
```

觀察一下，有發現`第二個__next__()` 的確沒有傳值，就只有去調用而已。

有沒有發現上面代碼裡面看起來很像二個任務在交互，所以接下來我們就把做包子的過程規範一點。

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import time
def consumer(name):
    print("%s 準備吃包子啦!" %(name))
    while True:
       baozi = yield

       print("包子[%s]来了,被[%s]吃了!" %(baozi,name))

def producer(name):
    c = consumer('A')
    c2 = consumer('B')
    c.__next__()
    c2.__next__()
    print("老子开始準備做包子啦!")
    for i in range(10):
        time.sleep(1)
        print("做了1個包子分兩半!")
        c.send(i)
        c2.send(i)

producer("tony")

---------------執行結果---------------
A 準備吃包子啦!
B 準備吃包子啦!
老子开始準備做包子啦!
做了1個包子分兩半!
包子[0]来了,被[A]吃了!
包子[0]来了,被[B]吃了!
做了1個包子分兩半!
包子[1]来了,被[A]吃了!
包子[1]来了,被[B]吃了!
做了1個包子分兩半!
包子[2]来了,被[A]吃了!
包子[2]来了,被[B]吃了!
做了1個包子分兩半!
包子[3]来了,被[A]吃了!
包子[3]来了,被[B]吃了!
做了1個包子分兩半!
包子[4]来了,被[A]吃了!
包子[4]来了,被[B]吃了!
做了1個包子分兩半!
包子[5]来了,被[A]吃了!
包子[5]来了,被[B]吃了!
做了1個包子分兩半!
包子[6]来了,被[A]吃了!
包子[6]来了,被[B]吃了!
做了1個包子分兩半!
包子[7]来了,被[A]吃了!
包子[7]来了,被[B]吃了!
做了1個包子分兩半!
包子[8]来了,被[A]吃了!
包子[8]来了,被[B]吃了!
做了1個包子分兩半!
包子[9]来了,被[A]吃了!
包子[9]来了,被[B]吃了!

Process finished with exit code 0
```

單綫程下的並行效果就出來，實際上還是串行的，因為可以在不同的角色中切換，並且因為運行速度特別快，所以感覺上像是並行的，這個就是『異步 IO 』的雛形。

epoll 的底層原理跟上面這個代碼差不多，本質上是這樣做的。

我們管上面這種代碼，叫『協程』，可以理解成最簡單的協程，也是一個典型的基本的生產者消費模型，簡單說，就理解成一個人在生產，一個在消費。





