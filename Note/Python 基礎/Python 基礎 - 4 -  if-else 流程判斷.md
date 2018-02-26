### if else 流程判斷

hmm~前面講了那麼多，終於可以稍稍的正式進入另一個階段，沒錯，要開始寫判斷式了
這次先從最簡單的判斷式開始，`if else` 開始… Go

首先，之前有寫有一個簡單的互動式 `用戶輸入` 的代碼，忘記了嗎？沒關係!!!
請回去看 [Python 基礎 - 3 - 用戶交互程序](./Python 基礎 - 3 - 用戶交互程序.md) 的第一個代碼，這次會針對這個代碼做一個小小的優化，是什麼呢？

各位有沒有覺得在輸入密碼時，是用 `明碼` 表示的，看起來是不是覺得很怪怪的，那要怎麼樣讓它變成不是明碼呢？

在 Python 裡有 `標準庫`，這個是不用透過安裝就有的，現在要先認識第一個模塊，叫 `getpass`，可以透過 `import` 就可以直接調用，而在Shell Script中要調用另一個腳本就用 `source`，現在就來動手作個小實驗吧…

`(這裡請使用Terminal去執行，因為 getpass 在 Pycharm 中，是無法使用的)`

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import getpass

username = input("username:")
password = getpass.getpass("password:")

print(username, password)

---------------執行結果---------------

username:ironman
password:
ironman tonystark
```

現在確認看到已經不是明碼輸出了，很好，接著我們在來多加個功能，要如何判斷使用者輸入的是否正確？

```
import getpass

_username = 'ironman'
_password = 'tonystark'

username = input("username:")
password = getpass.getpass("password:")

if _username == username and _password == password:
    print("Weclome {name} Login ".format(name=username))
else:
    print("invalid username or password")

print(username, password)

---------------執行結果---------------

username:ironman
password:
Weclome ironman Login
ironman tonystark 
```

以上就是簡單的『用戶認証』，接下來大家有發現有什麼地方不一樣嗎？

hmm…各位有覺得為什麼 Python 的判斷式看起來有點怪怪的？

是因為 Python 中沒有所謂的結束符號來表示，而是使用 `縮排`，不像 C, C++, Java 等語言用 `{ }` 大括號定義程式區塊，而這樣做的好處是可以讓 Python 整個結構看起來較為清晰

來小小實驗一下，如果沒有按照 Pythone 的規範來做縮排，看看會發生什麼事…？

```
import getpass

_username = 'ironman'
_password = 'tonystark'

username = input("username:")
password = getpass.getpass("password:")

if _username == username and _password == password:
    print("Weclome {name} Login ".format(name=username))
else:
    print("invalid username or password")

 print(username, password)

---------------執行結果---------------

 File "passwd.py", line 18
    print(username, password)
                            ^
IndentationError: unindent does not match any outer indentation level
```

`IndentationError` 很明顯的出現縮排錯誤，所以還是要注意縮排唷


簡單的會做了，那我們就來稍稍提升一下難度，來試做一個猜年紀的小遊戲

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

age_of_ironman = 35

guess_age = input("guess age:")

if guess_age == age_of_ironman:
    print("Bingo, You got it!!!")
elif guess_age > age_of_ironman:
    print("You may think smaller...")
else:
    print("You may think bigger...")

---------------執行結果---------------

guess age:2
Traceback (most recent call last):
  File "/paht/to/python/project/guess.py", line 11, in <module>
    elif guess_age > age_of_ironman:
TypeError: unorderable types: str() > int()
```

噢哦，出錯了，知道為什麼嗎？
不知道的可以參考 [Python 基礎 - 3 - 用戶交互程序](./Python 基礎 - 3 - 用戶交互程序.md) 的第四、五個代碼，再複習一下

`TypeError: unorderable types: str() > int()`   是因為 `input()` 默認都是當字符串，即使你輸入的是數字，也是會被轉成字符串

所以就再修改一下代碼

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

age_of_ironman = 35

guess_age = int(input("guess age:"))

if guess_age == age_of_ironman:
    print("Bingo, You got it!!!")
elif guess_age > age_of_ironman:
    print("You may think smaller...")
else:
    print("You may think bigger...") 

---------------執行結果---------------

guess age:21
You may think bigger...
```

OK，目前這代碼看起來大致已經完成了


知識點：

* Python 中，為什麼要縮排？是因為 Python 程式語言，並沒有使用 `{ } 來表示區塊 block` 的觀念，主要都是透過 `縮排` 來表示
       不管你是使用 if、elif、else、for、while 等會使用到區塊概念的關鍵字，當在 `:` 之後的下一行的就必須縮排
* 縮排時，標準的 tab stop 是 8，但個人的習慣大都會把 tab stop 換成 4個空白字符 (業界好像也是這樣用吧？)


參考來源：

* [關於 Python 的縮排](http://www.codemud.net/~thinker/GinGin_CGI.py/show_id_doc/263)
* [瓶水相逢 - 艾小克](https://dotblogs.com.tw/chhuang/2012/08/17/74115)