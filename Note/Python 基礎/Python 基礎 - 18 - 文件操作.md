### 文件操作

在來我們來玩一下文件操作，這個在未來工作上，也是會很常用到的功能

Python2.7中，可以用 `file()` 來打開文件，而在 Python3 中，一律都是用 `open()` ，接下來在當前目錄下，先建立一個空文件叫 `test`

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open("test")
f.write('i am a ironman')
f.close()

---------------執行結果---------------

io.UnsupportedOperation: not writable

Process finished with exit code 1
```

報錯了，雖然打開了，但不可以寫，因為預設只有`讀取`的權限，沒有 `w(寫入)` 的權限，詳細可以參考下圖

![文件預設mode](./IMG/20.png)

那到底有幾種模式呢？請看下表

**文件操作模式:**

Character | Meaning
--------- | -------- 
`r`       | open for reading (default)
`w`       | open for writing, truncating the file first
`x`       | create a new file and open it for writing
`a`       | open for writing, appending to the end of the file if it exists
`b`       | binary mode
`t`       | text mode (default)
`+`       | open a disk file for updating (reading and writing)
`U`       | universal newline mode (deprecated)


所以依據上表的權限，我們給予 `w` 的權限試試

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open("test", 'w')
f.write('i am a ironman')
f.close()
```

咦！可以寫入了，那就在多寫個幾行試試？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open("test", 'w')
f.write('i am a ironman')
f.write('my name is tony stark')
f.close()
```

觀察一下， `test` 這個文字檔，有發現什麼了嗎？是的，你剛剛打的字都已經連起來了， `i am a ironmanmy name is tony stark` 就像是這樣子，明明程式是分別寫二行的，但
怎麼還會連起來呢？這其實是因為沒有使用`換行符號 (\n)`，只要在文字最後面加入換行符號就可以換行嚕

再來我們來試試用 `a` 權限，先寫入檔案，再讀取檔案看看，試試有什麼作用？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open("test", 'a')
#f.write('i am a ironman')
f.write('my name is tony stark')
date = f.read()
print('----->', date)
f.close()

---------------執行結果---------------

io.UnsupportedOperation: not readable

Process finished with exit code 1
```
咦，怎麼會出錯呢？其實是因為 `a` 並不能做讀取的動作，它只會在原有的文件裡的最後面一行，附加新增的文字進去


以上就是簡單的文件操作，接下來在進階一點點，如果我想要打印文件的前三行，怎麼做？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


f = open('test', 'r')
print(f.readline())
print(f.readline())
print(f.readline())

---------------執行結果---------------

Python（英國發音：/ˈpaɪθən/ 美國發音：/ˈpaɪθɑːn/），是一種物件導向、直譯式的電腦程式語言。

它包含了一組功能完備的標準庫，能夠輕鬆完成很多常見的任務。它的語法簡單，與其它大多數程式設計語言使用大括弧不一樣，它使用縮進來定義語句塊。

與Scheme、Ruby、Perl、Tcl等動態語言一樣，Python具備垃圾回收功能，能夠自動管理記憶體使用。

它經常被當作腳本語言用於處理系統管理任務和網路程式編寫，然而它也非常適合完成各種高階任務。


Process finished with exit code 0
```

可是上面代碼僅限用於讀取很少的行數，如果今天要讀取二、三十行，那總不能一直使用`print(f.readline())`吧，所以我們可以應用之前所學的，使用 `for循環` 來簡化代碼，並達成我們想要的

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('test', 'r')

for i in range(3):
    print(f.readline())
    
---------------執行結果---------------

Python（英國發音：/ˈpaɪθən/ 美國發音：/ˈpaɪθɑːn/），是一種物件導向、直譯式的電腦程式語言。

它包含了一組功能完備的標準庫，能夠輕鬆完成很多常見的任務。它的語法簡單，與其它大多數程式設計語言使用大括弧不一樣，它使用縮進來定義語句塊。

與Scheme、Ruby、Perl、Tcl等動態語言一樣，Python具備垃圾回收功能，能夠自動管理記憶體使用。


Process finished with exit code 0
```

再來，把這個文件用循環跑過一次

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('test', 'r')

for line in f.readlines():
    print(line.strip())

---------------執行結果---------------

Python（英國發音：/ˈpaɪθən/ 美國發音：/ˈpaɪθɑːn/），是一種物件導向、直譯式的電腦程式語言。
它包含了一組功能完備的標準庫，能夠輕鬆完成很多常見的任務。它的語法簡單，與其它大多數程式設計語言使用大括弧不一樣，它使用縮進來定義語句塊。
與Scheme、Ruby、Perl、Tcl等動態語言一樣，Python具備垃圾回收功能，能夠自動管理記憶體使用。
它經常被當作腳本語言用於處理系統管理任務和網路程式編寫，然而它也非常適合完成各種高階任務。
Python虛擬機本身幾乎可以在所有的作業系統中運行。
使用一些諸如py2exe、PyPy、PyInstaller之類的工具可以將Python原始碼轉換成可以脫離Python解釋器執行的程式。
Python的官方直譯器是CPython，該直譯器用C語言編寫，是一個由社群驅動的自由軟體，目前由Python軟體基金會管理。
Python支援命令式程式設計、物件導向程式設計、函數式編程、面向側面的程式設計、泛型編程多種編程範式。

Process finished with exit code 0    
```

上面這代碼是先把文件讀成一個列表，並且把換行符號給刪除了，再來如果我們希望在第四行不要打印出來，該怎麼做？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('test', 'r')

for index, line in enumerate(f.readlines()):
    if index == 3:
        print('---------我是分隔線----------')
        continue
    print(line.strip())
    
---------------執行結果---------------

Python（英國發音：/ˈpaɪθən/ 美國發音：/ˈpaɪθɑːn/），是一種物件導向、直譯式的電腦程式語言。
它包含了一組功能完備的標準庫，能夠輕鬆完成很多常見的任務。它的語法簡單，與其它大多數程式設計語言使用大括弧不一樣，它使用縮進來定義語句塊。
與Scheme、Ruby、Perl、Tcl等動態語言一樣，Python具備垃圾回收功能，能夠自動管理記憶體使用。
---------我是分隔線----------
Python虛擬機本身幾乎可以在所有的作業系統中運行。
使用一些諸如py2exe、PyPy、PyInstaller之類的工具可以將Python原始碼轉換成可以脫離Python解釋器執行的程式。
Python的官方直譯器是CPython，該直譯器用C語言編寫，是一個由社群驅動的自由軟體，目前由Python軟體基金會管理。
Python支援命令式程式設計、物件導向程式設計、函數式編程、面向側面的程式設計、泛型編程多種編程範式。

Process finished with exit code 0
```

我們一樣沿用之前上一個的代碼，並且使用`下標`的方式，來標示出第四行，再使用 `continue` ，這樣就可以不打印出來第四行了

假設，我們現在有一個文件，大約有 2GB ，那使用 `readlines()` 這種方式的話，是一次性的先把資料讀成列表後，暫時放在記憶體當中，會相當花時間，所以 `readlines()` 只適合讀取小文件，所以那有沒有一種方法，每讀一行，就只保留一行在記憶體裡，這要怎麼弄呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('test', 'r')

for line in f:
    print(line)
```

上面這代碼就是一行一行的讀，且記憶體裡只會存一行，這寫法是效能較好的，那為什麼會變的比較好，是因為已經把 `f` 這個文件變成一個`迭代器`，就`不是列表`的形式了，所以也沒辦法透過 `enumerate()` 來做`下標`了，因此我們需要自已做一個`計數器`來計算到幾行，好了，那要怎麼把上面的代碼跟上一個代碼做結合呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

f = open('test', 'r')
count = 0
for line in f:
    if count == 3:
        print('---------我是分隔線----------')
        count += 1
        continue
    print(line)
    count += 1
    
---------------執行結果---------------

Python（英國發音：/ˈpaɪθən/ 美國發音：/ˈpaɪθɑːn/），是一種物件導向、直譯式的電腦程式語言。
它包含了一組功能完備的標準庫，能夠輕鬆完成很多常見的任務。它的語法簡單，與其它大多數程式設計語言使用大括弧不一樣，它使用縮進來定義語句塊。
與Scheme、Ruby、Perl、Tcl等動態語言一樣，Python具備垃圾回收功能，能夠自動管理記憶體使用。
---------我是分隔線----------
Python虛擬機本身幾乎可以在所有的作業系統中運行。
使用一些諸如py2exe、PyPy、PyInstaller之類的工具可以將Python原始碼轉換成可以脫離Python解釋器執行的程式。
Python的官方直譯器是CPython，該直譯器用C語言編寫，是一個由社群驅動的自由軟體，目前由Python軟體基金會管理。
Python支援命令式程式設計、物件導向程式設計、函數式編程、面向側面的程式設計、泛型編程多種編程範式。

Process finished with exit code 0
```




知識點：

* 文件句柄： 就是文件的記憶體位置，句柄包含了：文件名、字符集、大小、硬碟的起始位置等。


參考資料：

[句柄wiki](https://zh.wikipedia.org/wiki/%E5%8F%A5%E6%9F%84)

