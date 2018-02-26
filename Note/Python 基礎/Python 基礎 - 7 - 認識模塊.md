### 認識模塊

什麼是模塊？簡單說就是別人寫好了一堆功能，封裝在一起。

模塊有分二種，一個是之前有提到的 `標準庫`，就是不需要透過額外的安裝就有的模塊 ，另一個叫 `第三方庫`，需要另外安裝才能使用的模塊


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import sys

print(sys.path)

---------------執行結果---------------

['/user/ironman/python', '/usr/local/Cellar/python3/3.5.2_3/Frameworks/Python.framework/Versions/3.5/lib/python35.zip', '/usr/local/Cellar/python3/3.5.2_3/Frameworks/Python.framework/Versions/3.5/lib/python3.5', '/usr/local/Cellar/python3/3.5.2_3/Frameworks/Python.framework/Versions/3.5/lib/python3.5/plat-darwin', '/usr/local/Cellar/python3/3.5.2_3/Frameworks/Python.framework/Versions/3.5/lib/python3.5/lib-dynload', '/usr/local/lib/python3.5/site-packages']
```

上面代碼打印出很多個路徑，這到底是什麼東西呢？其實這就是 Python 的環境變量，換句說話，當我們使用 `標準庫` 或是 `第三方庫` 時，
會去這些路徑做調用引入的動作 `/usr/local/lib/python3.5/site-packages`  這個是當我們自已有編寫模塊時，可以放在這路徑底下，
若找不到 site-packages，就改找 `dist-packages`，也可以把自已寫的模塊放在此路徑下，讓全局可調用
接下來試試 `sys.argv` 這是做什麼用的？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import sys

print(sys.argv)

---------------執行結果---------------

# 在 Pycharm裡執行，會打印絕對路徑
['/Users/ironman/PycharmProjects/pratice1/sys.py']

Process finished with exit code 0

# 在Terminal裡執行，會打印相對路徑
['sys.py']

# 在Terminal裡再執行一次，並加入其他參數
['sys.py', '1', '2', '3']
```

其實這模塊就是跟我們在 Shell Script 執行時，需要要取讀 `$1` , `$2` , `$3` 參數的值，就可以用這個方式來做，
接下來我們要來介紹列表，列表是什麼呢？剛剛上面代碼執行第三次的結果，它就是一種列表的形式 `['sys.py', '1', '2', '3']` ，
假設如果我們要讀取列表中，第三個值的話，應該要怎麼做呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import sys

print(sys.argv)
print(sys.argv[2])

---------------執行結果---------------

# 在Terminal執行，python3 sys.py 1 2 3
['sys.py', '1', '2', '3']
2
```

再來介紹 `OS` 模塊 ，這個是調用操作系統命令，來達成建立文件，刪除文件，查詢文件等，就是使用這個模塊，現在就來做個小小實驗…

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import os

os.system("df -h")

---------------執行結果---------------

Filesystem      Size   Used  Avail Capacity  iused    ifree %iused  Mounted on
/dev/disk2     222Gi   45Gi  178Gi    21% 11736792 46570438   20%   /
devfs          182Ki  182Ki    0Bi   100%      631        0  100%   /dev
map -hosts       0Bi    0Bi    0Bi   100%        0        0  100%   /net
map auto_home    0Bi    0Bi    0Bi   100%        0        0  100%   /home

Process finished with exit code 0
```

上面打印出來就是顯示目前硬碟容量空間，那現在如果想要把這結果存下來，要怎麼做？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import os

cmd_res = os.system("df -h")
print("---->", cmd_res)

---------------執行結果---------------

Filesystem      Size   Used  Avail Capacity  iused    ifree %iused  Mounted on
/dev/disk2     222Gi   45Gi  178Gi    21% 11737038 46570192   20%   /
devfs          182Ki  182Ki    0Bi   100%      631        0  100%   /dev
map -hosts       0Bi    0Bi    0Bi   100%        0        0  100%   /net
map auto_home    0Bi    0Bi    0Bi   100%        0        0  100%   /home
----> 0

Process finished with exit code 0
```

奇怪，為什麼打印出來是 `0` ，這個 `0` 在Linux中代表命令執行正確，回傳0，而 `cmd_res = os.system("df -h")` 這個是指stdout到螢幕上，並不會把結果給保存下來，那如果我想要保存這個結果呢？換個命令試試…

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import os

cmd_res = os.popen("ls")
print("---->", cmd_res)

---------------執行結果---------------
----> <os._wrap_close object at 0x101fd82e8>

Process finished with exit code 0
```

咦~這打印出來的是一個記憶體對象的位置，但跟我想要的結果不太一樣，那要如何才能把想要的結果給正確呈現，需要在動一點點小手腳

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import os

cmd_res = os.popen("ls").read()
print("---->", cmd_res)

---------------執行結果---------------
----> Filesystem      Size   Used  Avail Capacity  iused    ifree %iused  Mounted on
/dev/disk2     222Gi   45Gi  178Gi    21% 11737886 46569344   20%   /
devfs          182Ki  182Ki    0Bi   100%      631        0  100%   /dev
map -hosts       0Bi    0Bi    0Bi   100%        0        0  100%   /net
map auto_home    0Bi    0Bi    0Bi   100%        0        0  100%   /home

Process finished with exit code 0
```

這樣就把結果給正確讀取出來了，但…為什麼要用 `read()`，其實就是 `os.popen("ls")` 是暫時存在記憶體裡的某一個位置，所以需要透過 `read()`這個方法把它給讀取出來，接下來我們再來試試要怎麼在當前目錄中，建立一個資料夾

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import os

os.mkdir("new_dir")


---------------執行結果---------------

coding.py
guess.py
guess_for.py
guess_for_anyplay.py
guess_while.py
interaciton.py
login.py
new_dir    # ------>  這就是我們剛剛所建立的資料夾
passwd.py
sys.py
test
test.py
test.sh
var.py
---> 0      #  執行正確，回傳值為0
```

接下來我們要來自已寫個一個簡單的模塊叫 `sys_login.py`，因為有使用 `getpass` 務必使用 terminal 去執行

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


import getpass

_username = 'ironman'
_password = 'tonystark'

username = input("請輸入用戶名稱:")
password = getpass.getpass("請輸入密碼:")

if _username == username and _password == password:
    print("Weclome %s Login " % username)
else:
    print("invalid username or password")

print(username, password)
```

接下來再寫一個 python 腳本叫 `sys_mod.py` 來調用剛剛寫好的模塊

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


import sys_login

---------------執行結果---------------

# 在當前目錄下，執行 python3 sys_mod.py
請輸入用戶名稱:ironman
請輸入密碼:
Weclome ironman Login
ironman tonystark
```

嗯，觀察結果，二個檔案同時存在於當前目錄中時，發現可以正確執行，那如果把 `sys_login.py` 移至 `new_dir` 裡(之前練習中，所建立的資料夾) ，那再觀察會不會正確執行呢？首先在當前目錄的列出目前檔案，可以用 `tree` 這個指令去做列表

```
$ tree

---------------執行結果---------------

├── __pycache__
│   └── sys_login.cpython-35.pyc
├── new_dir
│   └── sys_login.py
├── sys.py
└── sys_mod.py

2 directories, 4 files
```

確認已經把 `sys_login.py` 移至 `new_dir` 裡後，就可以直接在當前目錄下再次執行，觀察看看有什麼不同？

```
$ python3 sys_mod.py

---------------執行結果---------------

Traceback (most recent call last):
  File "sys_mod.py", line 4, in <module>
    import sys_login
ImportError: No module named 'sys_login'
```

很明顯可以發現錯誤訊息是說 `ImportError: No module named 'sys_login'` 匯入錯誤，找不到這個模塊，那這該如何修復呢？

還記得一開始的代碼所打印的 Python 全局境變量嗎？這次我們把 `sys_login.py` 這個模塊，放到 `/usr/local/lib/python3.5/site-packages/` ，然後再次執行一次，觀察又有什麼不同？

```
$ cp new_dir/sys_login.py  /usr/local/lib/python3.5/site-packages/
```

在當前目錄下，再執行一次 `tree` 確認已經被搬走了


```
$ tree

---------------執行結果---------------

├── __pycache__
│   └── sys_login.cpython-35.pyc
├── new_dir
├── sys.py
└── sys_mod.py

2 directories, 4 files
```


```
$ ls -la /usr/local/lib/python3.5/site-packages/sys_login.py

---------------執行結果---------------

-rw-r--r--  1 daniel  admin  371 12  7 01:34 /usr/local/lib/python3.5/site-packages/sys_login.py
```


```
$ python3 sys_mod.py

---------------執行結果---------------

請輸入用戶名稱:ironman
請輸入密碼:
Weclome ironman Login
ironman tonystark
```

咦~居然又可以輸入用戶名稱跟密碼了，這果然印證了之前所說的，可以把自已所寫好的模塊，放到 `/usr/local/lib/python3.5/site-packages/` 路徑下，Python 自動會去這些路徑底上去尋找，並做調用。