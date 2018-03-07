### 文件操作_v2

嗯，那如何要把游標的位置給打印來？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('test', 'r')
print(f.tell())

---------------執行結果---------------

0

Process finished with exit code 0
```

那在試試把文件讀完後，再打印一次游標位置

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('test', 'r')
print(f.tell())
print(f.readline())
print(f.tell())

---------------執行結果---------------
0
Python（英國發音：/ˈpaɪθən/ 美國發音：/ˈpaɪθɑːn/），是一種物件導向、直譯式的電腦程式語言

129   # 129個字符，記得換行符號要也算進去哦

Process finished with exit code 0
```

唔…不相信？是 129 個字符，好吧，那就用另一個方式 `read()` 來呈現好了，預設 `read()` 不指定的話，是讀取所有的，因此我們這次使用 `read(6)` 來試試，看看是不是真的為 `6` 個字符


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('test', 'r')
print(f.tell())
print(f.read(6))
print(f.tell())

---------------執行結果---------------

0
Python
6

Process finished with exit code 0
```

嗯嗯，真的是 `6` 個字符，那通常我們還是會用 `readline()` ，因為這樣不用去數這一行有幾個字符，直接幫你讀取整行，那假設我們現在讀取了三行後，要怎麼讓游標回到一開始？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('test', 'r')
print(f.tell())
print(f.readline())
print(f.readline())
print(f.readline())
print(f.tell())
f.seek(0)
print(f.readline())

---------------執行結果---------------

0
Python（英國發音：/ˈpaɪθən/ 美國發音：/ˈpaɪθɑːn/），是一種物件導向、直譯式的電腦程式語言

它包含了一組功能完備的標準庫，能夠輕鬆完成很多常見的任務。它的語法簡單，與其它大多數程式設計語言使用大括弧不一樣，它使用縮進來定義語句塊

與Scheme、Ruby、Perl、Tcl等動態語言一樣，Python具備垃圾回收功能，能夠自動管理記憶體使用

454
Python（英國發音：/ˈpaɪθən/ 美國發音：/ˈpaɪθɑːn/），是一種物件導向、直譯式的電腦程式語言


Process finished with exit code 0
```

唔…觀察一下，游標真的回到一開始了，原來使用 `seek(0)` 就可以了，通常 `tell()` 會跟 `seek()` 一起使用， `tell()` 會打印出目前游標的位置， `seek()` 可以指定游標的位置停在哪裡

那如果要檢查文件是用什麼字符編碼的，要怎麼查呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('test', 'r')
print(f.readline())
print(f.encoding)

---------------執行結果---------------

Python（英國發音：/ˈpaɪθən/ 美國發音：/ˈpaɪθɑːn/），是一種物件導向、直譯式的電腦程式語言

UTF-8

Process finished with exit code 0
```

唔，看到打印出 `UTF-8` 了，這樣就可以確定了

觀察一下，下面代碼的 `fileno()` 跟 `name()` 

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open("test", "wb")
print ("Name of the file: ", f.name)

fid = f.fileno()
print ("File Descriptor: ", fid)

---------------執行結果---------------

Name of the file:  foo.txt
File Descriptor:  3
```

`fileno()` 這個是作業系統專門的接口，專門去調度系统的 I/O 操作，這個接口是提供給所有程式語言使用，並不是Python才會有的

`name()` 這個是打印出文件名字，很簡單吧

`isatty()` 就是判斷是不是一個終端設備，為真返回`True`，這個較少用

`seekable()` 不是所有的文件都可以移動游標，這個主要是拿來判斷普通文件裡的游標能不能移動，為真返回`True`

`readable()` 判斷文件是不是可讀的權限，為真返回`True`

`writable()` 判斷文件是不是可寫的權限，為真返回`True`

`flush()` 即時地把記憶體中的緩衝區裡的資料，立刻寫回至文件中，同時清空緩衝區，一般情況下，文件關閉後會自動刷新緩衝區，但有時你需要在關閉前刷新它，這時就可以使用 `flush()`方法

下面我們來做一下實驗，先在terminal開二個分頁，一個用python3執行，進行互動模式，另一個用`tail -f foo.txt` 即時地去監看這個文件的變化，當我在分頁一執行`flush()`時，看看分頁二裡的文件是不是即時地被寫入

![Python_flush](./IMG/21.gif)

唔…看起來 `flush()` 的使用情境就是上圖的用法

接下來做一個比較有趣的應用，來實做一個進度條

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


import sys, time

for i in range(20):
    sys.stdout.write("#")
    sys.stdout.flush()
    time.sleep(0.5)
```

這個代碼，各位可以試試看，挺有趣的應用…

`closed` 判斷文件是不是關閉，為真返回`True`

再來講一下 `truncate()`，這個是截斷字符串用的

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('foo.txt', 'a', encoding='utf-8')
f.truncate()

---------------執行結果---------------

Hello Python
Hello Python1
Hello Python2
Hello Python3
Hello Python4
```

打開文件的模式改成用`a`，發現並不會去截斷字符串，那如果改成 `truncate(4)` ，看看會有什麼不同？


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('foo.txt', 'a', encoding='utf-8')
f.truncate(4)

---------------執行結果---------------

Hell
```

觀察發現，怎麼字符串從第四個之後被截斷了，這是因為 `truncate(4)` 的關係，那我們在加入先前學到的 `seek()` ，能不能從特定指定的位置之後開始截斷字符串，那要怎麼做呢？

```

#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('foo.txt', 'a', encoding='utf-8')
f.seek(10)
f.truncate(7)

---------------執行結果---------------

Hello P
```

有發現了嗎？怎麼還是一樣是`從頭開始截 7 個字符串`，完全沒有依照原本的想像，從指定的第 10 個字符串開始往後截斷，所以由此可知， `truncate()` 截斷的標準，完全不受 `seek()` 影響的

先前示範的代碼都`只能讀`或`只能寫`，接下來我們讓`讀寫`同時在一起好了，也順便觀察一下，有什麼不一樣的地方

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('foo.txt', 'r+', encoding='utf-8')
print(f.readline())
print(f.readline())
f.write("----------我是分隔線----------")
print(f.readline())

---------------執行結果---------------

Hello Python
Hello Python1
Hello Python2
Hello Python3
Hello Python4----------我是分隔線----------
```

咦，奇怪了，怎麼不是在第二行寫入`----------我是分隔線----------`，怎麼跑到最後一行了？等於其實因為是 `r+` 這個關係，是用`讀取跟寫入`的方式打開文件，不相信？好吧，那我們就來打印一下游標位置，確認一下是怎麼運作

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('foo.txt', 'r+', encoding='utf-8')
print(f.readline())
print(f.readline())
print(f.tell())
f.write("----------我是分隔線----------")
print(f.readline())

---------------執行結果---------------

Hello Python

Hello Python1

27
Hello Python2


Process finished with exit code 0

觀察文件內容：
Hello Python
Hello Python1
Hello Python2
Hello Python3
Hello Python4----------我是分隔線--------------------我是分隔線----------
```

觀察一下，上面的代碼，的確真的是以`讀寫`的方法打開文件了，那剛剛使用 `r+` ，那換使用 `w+` ，觀察一下，跟 `r+` 有什麼不同？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('foo.txt', 'w+', encoding='utf-8')
print(f.readline())
print(f.readline())
print(f.tell())
f.write("----------我是分隔線----------")
print(f.readline())

---------------執行結果---------------

0


Process finished with exit code 0

觀察文件內容：
----------我是分隔線----------
```

唔!!!怎麼只剩一行`----------我是分隔線----------`？這是因為 `f = open('foo.txt', 'w+', encoding='utf-8')` 這一句的關係，先打開一個文件叫 `foo.txt` ，接著因為 `w` 會有 `truncate` 的作用，所以不管原本文件裡面有什麼，都會被清空，然後在用`寫入`至文件裡面，所以上面的代碼執行完畢後，才會只剩下一行，又不相信？好吧，那就再來實驗一下，確認一下到底是怎麼運作的…

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('foo.txt', 'w+', encoding='utf-8')
print("目前游標起始位置：", f.tell())
f.write("----------我是第一行----------\n")
f.write("----------我是第二行----------\n")
f.write("----------我是第三行----------\n")
f.write("----------我是第四行----------\n")
f.write("----------我是第五行----------\n")
print("目前游標位置：", f.tell())
f.seek(10)
print("游標返回的位置：", f.tell())
print(f.readline())
f.write("My name is Tony Stark, I'm ironman")
print("最後游標的位置：", f.tell())
f.close()

---------------執行結果---------------

目前游標起始位置： 0
目前游標位置： 180
游標返回的位置： 10
我是第一行----------

最後游標的位置： 214

Process finished with exit code 0

觀察文件內容：
----------我是第一行----------
----------我是第二行----------
----------我是第三行----------
----------我是第四行----------
----------我是第五行----------
My name is Tony Stark, I'm ironman
```

咦，正常應該要從`我是第一行----------`這邊開始寫入的，但就是真的沒辦法寫，因為`w+`是做`寫入更新`，所以即使游標返回特定位置，一樣不受影響，只會寫在文件裡的最後一行



```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('foo.txt', 'rb', encoding='utf-8')     # 讀取二進制文件
print(f.readline())
print(f.readline())
print(f.readline())
f.close()

---------------執行結果---------------

Traceback (most recent call last):
  File "/Python/project/path/file_operation.py", line 4, in <module>
    f = open('foo.txt', 'rb', encoding='utf-8')     # 讀取二進制文件
ValueError: binary mode doesn't take an encoding argument

Process finished with exit code 1
```

若是讀取`二進制`文件，就不需要傳 `encoding` 編碼的參數給它，否則就會報錯，試試把 `encoding` 拿掉看看會有什麼結果？

接下來實做一下怎麼對`二進制文件`做操作

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('foo.txt', 'rb')     # 讀取二進制文件
print(f.readline())
print(f.readline())
print(f.readline())
f.close()

---------------執行結果---------------

b'----------\xe6\x88\x91\xe6\x98\xaf\xe7\xac\xac\xe4\xb8\x80\xe8\xa1\x8c----------\n'
b'----------\xe6\x88\x91\xe6\x98\xaf\xe7\xac\xac\xe4\xba\x8c\xe8\xa1\x8c----------\n'
b'----------\xe6\x88\x91\xe6\x98\xaf\xe7\xac\xac\xe4\xb8\x89\xe8\xa1\x8c----------\n'

Process finished with exit code 0
```

有看到前面多了一個 `b` 代表這個是一個 `bytes-like` 這個就是指二進制的文件

1. 網路傳送，只能用二進制進行傳送
2. 讀取二進制的文件

那既然有 `rb` ，也就有 `wb` ，那就來看一下怎麼用？

```
f = open('foo.txt', 'wb')     # 寫入二進制文件
f.write("Hello Binary\n")
f.close()

---------------執行結果---------------

Traceback (most recent call last):
  File "/Python/project/path/file_operation.py", line 4, in <module>
    f = open('fooo.txt', 'rb')     # 讀取二進制文件
TypeError: a bytes-like object is required, not 'str'

Process finished with exit code 1
```
出現 `TypeError: a bytes-like object is required, not 'str'` 這是說明不能是一個字符串，所以我們要把`字符串`轉成 [Python 基礎 - 10 - bytes 數據類型.md](./Python%20基礎%20-%2010%20-%20bytes%20數據類型.md)(可以回頭參考一下)

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('foo.txt', 'wb')     # 寫入二進制文件
f.write("Hello Binary\n".encode())
f.close()

---------------執行結果---------------
觀察文件內容：

Hello Binary

```

咦！奇怪，為什麼看到的還是一個字符串？其實存的時候`不是以 0101 儲存的`，而是`文件以二進制編碼儲存`


知識點：

`U` 表示在讀取時，會將 `\r`、`\n`、`\r\n` 自動轉換成 `\n` `(與 r 或 r+ 模式使用)`

* rU
* r+U

`b` 表示處理二進制文件 (如： FTP 上傳 ISO 檔的文件， Linux 可以怱略，但 windows 處理二進制文件時，需要標注)

修改文件有二種方法，一種是把文件裡的所有資料，都暫時存到記憶體裡，找到要修改的文字，然後再回存至文件裡，就像 `vim` 一樣，另一種是 `rename` 的方式，也就是建立另一個新的文件，然後修改完原本的文件後，寫入至新文件，那我們就來實做第二種方式

準備好二個文件，一個是 `foo` 裡面要有內容，另一個新文件是空的 - `foo.bak`

**foo文件內容如下**

```
柯文哲扮演1位到西門町刺青的Rocker，但因實在太怕痛而刺不下去
而柯文哲還以為是真的要在身上刺青
還說「啊？是真的要刺下去嗎？我還沒跟我老婆報備呢。」
```

**foo.bak文件內容如下**

```

```

要把`『是真的要刺下去嗎』` 修改成 `『不是真的要刺下去吧』`，並寫入至`foo.bak`那要怎麼做咧？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open("foo", "r", encoding="utf-8")
f_new = open("foo.bak", "w", encoding="utf-8")

for line in f:
    if "是真的要刺下去嗎" in line:
        line = line.replace("是真的要刺下去嗎", "不是真的要刺下去吧")
        f_new.write(line)
    else:
        f_new.write(line)
f.close()
f_new.close()
```

* `f = open("foo", "r", encoding="utf-8")` 只用讀取模式找到要修改的字符串
* `f_new = open("foo.bak", "w", encoding="utf-8")` 用寫入模式，是把上面找到修改後的字符串寫入至新文件裡
* `if "是真的要刺下去嗎" in line:` 先找出要修改的字符串
* 用 `replace()` 來做修改字符串

可以優化成下面代碼

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open("foo.txt", "r", encoding="utf-8")
f_new = open("foo.bak", "w", encoding="utf-8")

for line in f:
    if "是真的要刺下去嗎" in line:
        line = line.replace("是真的要刺下去嗎", "不是真的要刺下去吧")
    f_new.write(line)
f.close()
f_new.close()
```

觀察一下**foo.bak文件內容**

```
柯文哲扮演1位到西門町刺青的Rocker，但因實在太怕痛而刺不下去
而柯文哲還以為是真的要在身上刺青
還說「啊？不是真的要刺下去吧？我還沒跟我老婆報備呢。」
```

嗯！確實已經修改了，但上面的代碼，還是寫的不夠好，因為是寫死的，那如果我想讓用戶自已輸入想要替換的文字，那要怎麼做呢？

`請在Terminal中執行 $python3 sed.py 是真的要刺下去嗎 不是真的要刺下去吧`

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import sys

find_sys = sys.argv[1]
replace_sys = sys.argv[2]

f = open("foo.txt", "r", encoding="utf-8")
f_new = open("foo.bak", "w", encoding="utf-8")

for line in f:
    if find_sys in line:
        line = line.replace(find_sys, replace_sys)
    f_new.write(line)
f.close()
f_new.close()
```

在執行這代碼前，先砍掉`foo.bak`，確認不存在後，再執行上面的代碼，然後觀察一下是不是真的有替換了

那之前在操作文件時，有沒有發現常常有的代碼會寫`f.close()`，有的沒有寫，其實這是在操作文件時很容易被忽略，所以現在要介紹一個好物，`with語句`，它會主動幫我們關閉文件，並釋放文件資源，所以可以寫成下面的代碼，就不需要在額外寫`f.close()`了，這樣就輕鬆一點了

```
with open("file.txt", "r", encoding="utf-8") as f:
	...
```

所以上面的代碼還可以再優化

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import sys

find_sys = sys.argv[1]
replace_sys = sys.argv[2]

with open("foo.txt", "r", encoding="utf-8") as f, \
        open("foo.bak", "w", encoding="utf-8") as f_new:
    for line in f:
        if find_sys in line:
            line = line.replace(find_sys, replace_sys)
        f_new.write(line)
```

同樣在 terminal 執行上面的代碼


參考資料：

* [fileno](https://www.tutorialspoint.com/python3/file_fileno.htm)
