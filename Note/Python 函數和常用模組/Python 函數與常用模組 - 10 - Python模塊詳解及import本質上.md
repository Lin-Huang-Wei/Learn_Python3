#### 1 模塊定義：

* 什麼叫模塊？ 模塊本質上就是去實現一個功能，用來從邏輯上組織 python 代碼(變量、函數、類、邏輯)，本質就是.py結尾的 python 文件 (文件名：hello.py，對應的模塊名：hello)。

* 什麼叫包？ 本質就是一個目錄，必須帶有一個 `__init__.py` 的文件，用來從邏輯上組織模塊的。

#### 2 導入方法

我們先在 pycharm 中，建立一個 Directory 叫 `module_tony`，在到此資料夾裡，建立二個檔案，一個叫 `module_tony.py`，另一個叫 `main.py`，接下來就要用實際的方式來說明。

先在 `module_tony.py` 定義一個函數，那要怎麼在 `main.py` 去使用 module_tony.py 裡所定義好的函數！？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = 'tony'

def say_hello(name):
    print("Hello {} !!!".format(name))
```

接下來我們要怎麼去導入呢…請看下面情況

* import module_name

在 `main.py` 裡，去 import 剛剛寫好的 module_tony ，並去調用裡面的函數。

**main.py**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import module_tony

print(module_tony.name)
module_tony.say_hello()

---------------執行結果---------------
tony
Hello tony !!!

Process finished with exit code 0
```

* import module_name_1, module_name_2, module_name_3

這個太簡單就不 demo 了。

* from module_name import *

在 `main.py` 裡面，寫 `from module_tony import *`，但這種寫法較不建議使用，請看下面代碼：

**module_tony.py**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = 'tony'

def say_hello():
    print("Hello {} !!!".format(name))

def logger():
    print("in the module_tony")

def running():
    pass
```

**main.py**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

from module_tony import *

'''
# 實際上使用 from moule_tony import * ，其實就是把 module_tony 裡面的代碼放到 main.py 最上面去執行。

#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = 'tony'

def say_hello():
    print("Hello {} !!!".format(name))

def logger():
    print('in the module_tony')

def running():
    pass
'''

def logger():
    print('in the main')

logger()

---------------執行結果---------------
in the main

Process finished with exit code 0
```

觀察上面的代碼和執行結果，有發現到嗎？

沒錯，當使用 `from module_tony import *` 時，若 `module_tony.py` 跟 `main.py` 同時要去調用命名同樣的函數時，則會以 `main.py` 的為主，原因是因為 Python 解釋器是從上到下逐行去執行的。

當Python解釋器讀取到第一個 logger 時，就會把 `print("in the module_tony")` 放到記憶體位址，等再讀到第二個 logger時，同樣會把 `print('in the main')` 指向到同樣的記憶體位址，並取代前一個函數放進去的值。

* from module_name improt logger as logger_tony

這個用法，主要是用來解決上一個範例的，可以把 `module_tony.py` 裡的 `logger函數`  做一個別名叫 `logger_tony`，這樣就不會互相影響去調用 logger 函數了。

**main.py**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

from module_tony import logger as logger_tony

'''
# 實際上使用 from moule_tony import * ，其實就是把 module_tony 裡面的代碼放到 main.py 最上面去執行。

#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = 'tony'

def say_hello():
    print("Hello {} !!!".format(name))

def logger():
    print("in the module_tony")

def running():
    pass
'''

def logger():
    print('in the main')

logger()
logger_tony()

---------------執行結果---------------
in the main
in the module_tony

Process finished with exit code 0
```

* from module_name import m1, m2, m3

這個也很簡單，就不 demo 了。

#### 3 import本質 (路徑搜索和搜索路徑) 

導入模塊的本質就是把 Python 的文件解釋一遍，什麼？！這還不清楚，好吧，請參考下面範例

**main.py 導入使用 import module_tony**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import module_tony

'''
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = 'tony'

def say_hello():
    print("Hello {} !!!".format(name))

def logger():
    print("in the module_tony")

def running():
    pass
'''

print(module_tony.name)
module_tony.say_hello()

---------------執行結果---------------
tony
Hello tony !!!

Process finished with exit code 0
```

`import module_tony` 實際上就是 Python 解釋器會把 `module_tony.py` 整個文件解釋一遍後，把整個代碼執行完的結果，賦值給 `module_tony` 這個變量，於是…當要去調用時，就要下 `module_tony.name` or `module_tony.say_hello()`。

**main.py 導入使用 from module import name, logger as logger_tony**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

from module_tony import name, logger as logger_tony

'''
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = 'tony'

def logger():
    print('in the module_tony')
'''

def logger():
    print('in the main')

print(name)
logger_tony()
logger()

---------------執行結果---------------
tony
in the module_tony
in the main

Process finished with exit code 0
```

`from module_tony import name, logger as logger_tony` 實際上就是 Python 解釋器把 `相關的變量、函數、類、邏輯` 等，拿到了當前的文件上解釋了一遍，所以就會變成 `name = 'tony'` 、 `logger_tony = logger = print('in the module_tony')`，因此在調用上，就不需要在加入 `模塊名(module_tony)` 了，直接調用即可。

導入包的本質就是去執行這個包底下的 `__init__.py` 這個文件。

我們在 Pycharm 中，建立一個包叫 `package_tony`，同時也建立一個 `p_tony.py` 的文件，跟 `package_tony` 是同一層目錄，建立之後便會看該包底下會有一個檔案叫 `__init__.py`，進入到 `package_tony` 這個包，再建立一個檔案叫 `ironman.py` ，接下來就實例說明

**目錄結構**

```
# tree pratice_import
pratice_import
├── p_tony.py
└── package_tony
    ├── __init__.py
    └── ironman.py
```

**\_\_init\_\_.py**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

print('from the package package_tony')
```

**ironman.py**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def iron():
    print('in the ironman')
```

**p_tony.py**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import package_tony

package_tony.ironman.iron()

---------------執行結果---------------
from the package package_tony
Traceback (most recent call last):
  File "/pratice_import/p_tony.py", line 6, in <module>
    package_tony.ironman.iron()
AttributeError: module 'package_tony' has no attribute 'ironman'

Process finished with exit code 1
```

請注意， `AttributeError: module 'package_tony' has no attribute 'ironman'` 這個錯誤是說 `package_tony` 模塊，沒有 `ironman` 這個屬性，為什麼會沒有呢？

是因為導入包跟導入模塊的邏輯是不太一樣的，導入包就是去執行該包底下的 `__init__.py` 文件，所以你可以看到上面代碼執行後，會打印出 `from the package package_tony` ，意謂著這個包已經被正常導入了，但跟 `ironman.py` 這個模塊是沒有什麼關係的，因此，我們在調用時，就不能使用 `package_tony.ironman.iron()` 這樣的方法去調用，那要讓 `ironman.py` 可以正常被調用的話，就要去修改一下

**\_\_init\_\_.py**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


print('from the package package_tony')

'''
因為 __init__.py 跟 ironman.py 是在同一層目錄的
所以 __init__.py 一定可以找到 ironman.py
'''

from . import ironman   # 用相對路徑去找，ironman = 把所有代碼拿當前位置去執行

'''
def iron():
    print('in the ironman')
''' 
``` 

**p_tony.py**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import package_tony

package_tony.ironman.iron()

---------------執行結果---------------
from the package package_tony
in the ironman

Process finished with exit code 0
```

或是可以改用另一個方法 `p2_tony.py` 這樣的寫法也行

**目錄結構**

```
# tree pratice_import
pratice_import
├── p2_tony.py
├── p_tony.py
└── package_tony
    ├── __init__.py
    └── ironman.py
```

**p2_tony.py**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import os, sys

sys.path.append(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
print(sys.path)

from pratice_import import package_tony

package_tony.ironman.iron()

---------------執行結果---------------

['/Users/mayday/PycharmProjects/pratice_import', '/usr/local/Cellar/python3/3.6.0/Frameworks/Python.framework/Versions/3.6/lib/python36.zip', '/usr/local/Cellar/python3/3.6.0/Frameworks/Python.framework/Versions/3.6/lib/python3.6', '/usr/local/Cellar/python3/3.6.0/Frameworks/Python.framework/Versions/3.6/lib/python3.6/lib-dynload', '/usr/local/lib/python3.6/site-packages' ]
from the package package_tony
in the ironman

Process finished with exit code 0
```

觀察上面代碼後，確定 `導入包的本質就是去執行這個包底下的 __init__.py 這個文件`。

**小結**

* `import ironman` ； ironman = 'ironman.py all code'
* `from ironman import iron` ； iron = 'code'

`import module_name →→→ module_name.py →→→ module_name.py 的路徑 →→→ sys.path` 

#### 4 導入優化

**目錄結構**

```
# tree package_ironman
package_ironman
├── module_tony.py
└── stark.py
```

**module_tony.py**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def ironman():
    print('in the ironman')
```

**stark.py**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import time
import module_tony

def timer(func):
    def deco(*args, **kwargs):
        start_time = time.time()
        func(*args, **kwargs)
        stop_time = time.time()
        print("in the func run time is {}".format(stop_time - start_time))
    return deco

@timer
def logger():
    module_tony.ironman()
    print('in the logger')

@timer
def search():
    module_tony.ironman()
    print('in the search')


logger()
search()
---------------執行結果---------------

in the ironman
in the logger
in the func run time is 3.886222839355469e-05
in the ironman
in the search
in the func run time is 8.821487426757812e-06

Process finished with exit code 0
```

`module_tony.ironman()` ，請假想一下，有在其他200個函數中有去調用時，Python解釋器會怎麼做呢？可以從上面執行結果看出，每次執行一個函數時，都會去尋找 `module_tony` 這個路徑， 找到 `module_tony` 這個模塊，再去執行裡面的 `ironman` 函數，假設當有200多個函數去調用時，這樣會降低程序的執行效率，主要花費的時間都會是花在找 `module_tony` 模塊，那該如何優化呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import time
# import module_tony
from module_tony import ironman

def timer(func):
    def deco(*args, **kwargs):
        start_time = time.time()
        func(*args, **kwargs)
        stop_time = time.time()
        print("in the func run time is {}".format(stop_time - start_time))
    return deco

@timer
def logger():
    ironman()
    print('in the logger')

@timer
def search():
    ironman()
    print('in the search')


logger()
search()
---------------執行結果---------------

in the ironman
in the logger
in the func run time is 3.170967102050781e-05
in the ironman
in the search
in the func run time is 1.4066696166992188e-05

Process finished with exit code 0
```

**小結**

* `from module_name import module`


#### 5. 模塊的分類  

**Python 模塊分成三大類**

* 標準庫

1. time 與 datetime

* 開源模塊



* 自定義模塊





