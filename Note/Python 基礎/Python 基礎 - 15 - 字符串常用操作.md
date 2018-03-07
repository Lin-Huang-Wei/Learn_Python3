### 字符串常用操作

今天就介紹一下常用的字符串操作，都是以 `Python3` 撰寫的


**首字母變大寫**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = "ironman
print(name.capitalize())
---------------執行結果---------------
Ironman
```

**計算字符串重複的字共出現幾次**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = "i am a ironman"
print(name.count('a'))
---------------執行結果---------------
3
```

**把字符串置中後，長度要`50`，不足50的就用`-`補滿**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = "i am a ironman"
print(name.center(50, "-"))
---------------執行結果---------------
------------------i am a ironman------------------
```

**以`xx`為結尾的，就返回True，不是就False**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = "i am a ironman"
print(name.endswith("an"))
print(name.endswith("i"))
---------------執行結果---------------
True
False
```

**會把字符串中的`\t`變成20個空白格，`default tabsize=8`**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = "i \tam a ironman"
print(name.expandtabs(tabsize=20))
---------------執行結果---------------
i                   am a ironman
```

**算字符串長度**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = "i am a ironman"
print(len(name))
---------------執行結果---------------
14
```

**可以透過`find()`找出字符串要切片的起始位置，沒找到返回`-1`**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-



name = "i am a ironman"

>>> print(name.find("am"))
2											# 找到切片起始位置是2
>>> print(name[name.find("am"):3])
a
>>> print(name[name.find("am"):4])
am
>>> print(name[name.find("am"):5])
am
>>> print(name[name.find("am"):6])
am a
>>> print(name[name.find("am"):7])
am a
>>> print(name[name.find("am"):8])
am a i
>>> print(name[name.find("am"):9])
am a ir
>>> print(name[name.find("am"):10])
am a iro
>>> print(name[name.find("am"):])
am a ironman
```

**透過`formate()`對字符串做格式化**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = "My name is {name} and I'm {years} old"

>>> print(name.format(name='ironman', years='18'))
My name is ironman and I'm 18 old
```

**使用`format_map()`對字符串做格式化, 替換字段使用`{} 大括號`**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = "My name is {name} and I'm {years} old"

>>> print(name.format_map( {'name':'ironman', 'years':'18' } ))	# {} 是一種字典的形式
My name is ironman and I'm 18 old
```


**查找`name`這個字符串的下標位置**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

name = "i am a ironman"
>>> print(name.index(name))
0
```

**判斷字符串裡，是不是由數字或是大小寫英文字母組成的，為真就返回`True`，若是有包含特殊符號的為就會返回`False`**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('123'.isalnum())
True
>>> print('abc'.isalnum())
True
>>> print('ABC'.isalnum())
True
>>> print('abc123'.isalnum())
True
>>> print('ABC123'.isalnum())
True
>>> print('abc123@'.isalnum())
False
>>> print('ABC123.'.isalnum())
False
```

**判斷字符串，是不是所有字符都是為大小寫的英文字母，為真返回`True`，反則返回`False`**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('ABC'.isalpha())
True
>>> print('abc'.isalpha())
True
>>> print('abc123'.isalpha())
False
>>> print('abc@'.isalpha())
False
>>> print('abc1@'.isalpha())
False
```

**判斷字符串中，是不是為十進制的數字**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('11'.isdecimal())
True
>>> print("1234".isdecimal())
True
>>> print("12.34".isdecimal())
False
>>> print("1A".isdecimal())
False
```

**判斷字符串中，是不是數字(整數)**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('1234'.isdigit())
True
>>> print('12.34'.isdigit())
False
```

**判斷字符串中，是不是合法的標識符，也就是`變量名`命名是否合法**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


>>> print('_Python3'.isidentifier())
True
>>> print('1_Python3'.isidentifier())
False
>>> print('__Python3'.isidentifier())
True
>>> print('{Python3}'.isidentifier())
False
>>> print('我好弱'.isidentifier())		# 一般不會用中文當變量名，請勿使用
True
```

**判斷字符串中，是不是小寫字母，不管有沒有特殊符號，為真返回`True`，為否返回`False`**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('abc'.islower())
True
>>> print('abC'.islower())
False
>>> print('ab1'.islower())
True
>>> print('ab!@'.islower())
True
```

**判斷字符串中，是不是數字，為真返回`True`，為否返回`False`**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('1234'.isnumeric())
True
>>> print('12.34'.isnumeric())
False
>>> print('12A'.isnumeric())
False
```

**判斷字符串裡，是不是空格**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print(' '.isspace())
True
>>> print('\r\t\n '.isspace())
True
>>> print('\n '.isspace())		# 換行，游標在下一行
True
>>> print('\t '.isspace())		# tab鍵，8個空格
True
>>> print('\r '.isspace())		# 換行，游標在上一行
True
```

**判斷字符串裡，每個字母第一個字為大寫，為真返回`True`，為否返回`False`**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('My Name Is'.istitle())
True
>>> print('My Name is'.istitle())
False
```

**判斷字符串裡，是不是可以打印的字符串或是字符串為空，為真返回`True`，為否返回`False`，`(不常用)`**

> S.isprintable() -> bool
> 
> Return True if all characters in S are considered
> printable in `repr() or S is empty`, False otherwise

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('My Name Is'.isprintable())
True
>>> print('123.34'.isprintable())
True
>>> print(' '.isprintable())
True
>>> print('\n'.isprintable())
False
>>> print('\t'.isprintable())
False
```

**判斷字符串裡，每個字母是不是大寫，為真返回`True`，為否返回`False`**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('MY NAME IS'.isupper())
True
>>> print('MY NAME iS'.isupper())
False
```

**使用字符串作為分隔符，串連多個數據成一個字符串**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> s = ",".join(('Python', '3'))
>>> print(s)
Python,3
>>> print('+'.join(['1', '2', '3', '4']))
1+2+3+4
```

**字符串內容往左對齊，並使用指定字符填充剩餘長度**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print(name.ljust(50, '*'))
i am a ironman************************************
```

**字符串內容往右對齊，並使用指定字符填充剩餘長度**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print(name.rjust(50, '*'))
************************************i am a ironman
```

**字符串轉換成小寫**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('IRONMAN'.lower())
ironman
```

**字符串轉換成大寫**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('ironman'.upper())
IRONMAN
```

**刪除字符串左邊空格和enter，或是指定中的字符**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> s = "    i am ironman"
>>> print(s.lstrip())
i am ironman
>>> s = "i am ironman"
>>> print(s.lstrip('i am'))
ronman
```

**刪除字符串右邊空格和enter，或是指定中的字符**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> s = "i am ironman     "
>>> print(s.rstrip())
i am ironman
>>> s = "i am ironman"
>>> print(s.rstrip('man'))
i am iro
```

**刪除字符串左右二邊空格和enter，或是指定中的字符**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('\nironman\n'.strip())
ironman
>>> s = " i am a ironman "
>>> print(':', s.strip(), ':')
: i am a ironman :
>>> s = "i am a ironman"
>>> print(s.strip('man'))
i am a iro
```

**轉換表，`請注意觀察，才會了解有什麼差異`**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> p = str.maketrans("abcdef", '123456')
>>> print("i am a ironman".translate(p))
i 1m 1 ironm1n
```

**在字符串中，把old字符串取代成new的字符串**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('ironman'.replace('i', 'I', 1))
Ironman
>>> print('ironman'.replace('r', 'R', 2))
iRonman
>>> print('i am a ironman'.replace('a', '.'))
i .m . ironm.n
```


**在字符串中，從右邊開始找，找到最後一個的字符給打印出來下標位置**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> s = "i am a ironman"
>>> print(s.rfind("a"))
12
```

**把字符串拆成列表**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('1+2+3+4'.split('+'))
['1', '2', '3', '4']
>>> print('1+2\n+3+4'.split('\n'))
['1+2', '+3+4']
```

**字符串以換行符為分隔符拆分，去掉換行符；`keepends為True，保留換行符`
**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('1+2\n+3+4'.splitlines())
['1+2', '+3+4']
>>> print('1+2\n+3+4'.splitlines(keepends=True))
['1+2\n', '+3+4']
```

**字符串裡把小寫字母變大寫，大寫字母變小寫**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('Tony Stark'.swapcase())
tONY sTARK
```

**字符串裡每個單字的第一個字母變成大寫，其餘字母都會被轉成小寫**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('tony stark'.title())
Tony Stark
>>> print('hELLO wORLD pYTHON3'.title())
Hello World Python3
```

**在字符串的左邊填0**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

>>> print('tony stark'.zfill(10))
tony stark
>>> print('tony stark'.zfill(20))
0000000000tony stark
```


參考來源：

* [Python内置模块](http://www.howsoftworks.net/python.api/builtins/)