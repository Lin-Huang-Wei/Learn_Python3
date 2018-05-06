### 為什麼要序列化？

互聯網的產生帶來了機器之間的通訊需求，而二者之間雙方需要採用約定的協議，序列化和反序列化屬於通訊協議的一部分。

在 OSI 七層協議模型中，展現層 ( Presentation Layer ) 主要功能是把應用層的對象轉換成一段連續的二進制串，這就是序列化；反過來，把二進制串轉換成應用層的對象，就是反序列化。

一般而言，TCP/IP 協議的應用層對應到 OSI 七層協議模型的應用層、展示層、

* 序列化：將數據結構或對象轉換成二進制串的過程。
* 反序列化：將在序列化過程中所生成的二進制串轉換成數據結構或是對象的過程。


Python 在處理文件時，文件只能存**字符串**和**二進制**

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

info = {
    'name':'tony',
    'age':22

}

f = open("test.txt","w")
f.write(info)

f.close()

---------------執行結果---------------
Traceback (most recent call last):
  File "/Python/path/json序列化.py", line 13, in <module>
    f.write(info)
TypeError: write() argument must be str, not dict

Process finished with exit code 1
```

有看到上面代碼噴出了一個 ERROR 訊息，**TypeError: write() argument must be str, not dict**，這是說**必須存成字符串，而不是字典**

我們可以用 **str() 函數**來把字典轉成字符串

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

info = {
    'name':'tony',
    'age':22

}

f = open("test.txt","w")
f.write(info)

f.close()

---------------執行結果---------------

Process finished with exit code 0
```

但是問題來了，那我們要怎麼把它給讀取出來？

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

info = {
    'name':'tony',
    'age':22
}

f = open("test.txt", "r")
data = f.read()
f.close()

print(type(data))
print(data)

---------------執行結果---------------
<class 'str'>
{'name': 'tony', 'age': 22}

Process finished with exit code 0
```

嗯！正確讀取出數據了，那如果我希望讀取出來時，是一個字典呢？

可以用 **eval() 函數**去把字符串，變成一個字典的

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

info = {
    'name':'tony',
    'age':22
}

f = open("test.txt", "r")
data = eval(f.read())
f.close()

print(type(data))
print(data)
print(data['age'])

---------------執行結果---------------
<class 'dict'>
{'name': 'tony', 'age': 22}
22

Process finished with exit code 0
```

那如果不要透過 **eval() 函數** ，又要怎麼去實現呢？接下來就是重頭戲了，我們可以使用標準規範的 **json() 函數** ，我們先做序列化，再做反序列化。

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import json

info = {
    'name':'tony',
    'age':22
}

f = open("test.text", "r")
data = json.dumps(info)
f.write(data)
f.close()

print(type(data))
print(data)

---------------執行結果---------------
<class 'str'>
{"name": "tony", "age": 22}

Process finished with exit code 0
```

那接下來就是要把剛剛存進去的資料，透過 **json() 函數** 給讀取出來

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import json

info = {
    'name':'tony',
    'age':22
}

f = open("test.txt", "r")
data = json.loads(f.read())
f.close()

print(type(data["name"]))
print(data["name"])
print(type(data["age"]))
print(data["age"])

---------------執行結果---------------
<class 'str'>
tony
<class 'int'>
22

Process finished with exit code 0
```

小結一下

* 序列化請用 **json.dumps()**
* 反序列化請用 **json.loads()**

這樣就實現了一個把內存的數據對象給存到硬碟上了，並且還可以讀取回來。


```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import json

def sayhi(name):
    print("hello", name)

info = {
    'name':'tony',
    'age':22,
    'func': sayhi
}


f = open("test.txt","w")
data = json.dumps(info)
f.write(data)
# f.write(str(info))
f.close()

print(type(data))
print(data)

---------------執行結果---------------
Traceback (most recent call last):
  File "/Python/path/json序列化.py", line 17, in <module>
    data = json.dumps(info)
  File "/usr/local/Cellar/python3/3.6.0/Frameworks/Python.framework/Versions/3.6/lib/python3.6/json/__init__.py", line 231, in dumps
    return _default_encoder.encode(obj)
  File "/usr/local/Cellar/python3/3.6.0/Frameworks/Python.framework/Versions/3.6/lib/python3.6/json/encoder.py", line 199, in encode
    chunks = self.iterencode(o, _one_shot=True)
  File "/usr/local/Cellar/python3/3.6.0/Frameworks/Python.framework/Versions/3.6/lib/python3.6/json/encoder.py", line 257, in iterencode
    return _iterencode(o, 0)
  File "/usr/local/Cellar/python3/3.6.0/Frameworks/Python.framework/Versions/3.6/lib/python3.6/json/encoder.py", line 180, in default
    o.__class__.__name__)
TypeError: Object of type 'function' is not JSON serializable

Process finished with exit code 1
```

有看到上面代碼噴出了一個 ERROR 訊息，**TypeError: Object of type 'function' is not JSON serializable** ，這是說 function 不是 json 可序列化的類型，因為 json 沒辦法處理較複雜的數據。

json 只能處理簡單的數據類型，如: 字符串、列表、字典，json 是所有語言的通用的數據格式，主要目的是在不同的程式語言中，進行數據交換，也是目前的主流使用的數據格式。XML 跟 json 功能是一樣的，但慢慢地，XML 已經慢慢開始被 json 取代了。

如果想要處理比較複雜的數據的話，那我們就要使用另一個函數 --- **pickle() 函數** ，這個函數其實功能跟 **json() 函數** 的用法是完全一樣的。

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import pickle

def sayhi(name):
    print("hello", name)

info = {
    'name':'tony',
    'age':22,
    'func': sayhi
}


f = open("test.txt","w")
data = pickle.dumps(info)
f.write(data)
f.close()

print(type(data))
print(data)

---------------執行結果---------------
Traceback (most recent call last):
  File "/Python/path/json序列化.py", line 19, in <module>
    f.write(data)
TypeError: write() argument must be str, not bytes

Process finished with exit code 1
```

有看到上面代碼噴出了一個 ERROR 訊息， **TypeError: write() argument must be str, not bytes** ， 這是因為 **pickle 默認是二進制** ，所以寫入檔案時，也必須以 **binary mode** 去寫入。

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import pickle

def sayhi(name):
    print("hello", name)

info = {
    'name':'tony',
    'age':22,
    'func': sayhi
}


f = open("test.txt","w")
data = pickle.dumps(info)
f.write(data)
f.close()

print(type(data))
print(data)

---------------執行結果---------------
<class 'bytes'>
b'\x80\x03}q\x00(X\x04\x00\x00\x00nameq\x01X\x04\x00\x00\x00tonyq\x02X\x03\x00\x00\x00ageq\x03K\x16X\x04\x00\x00\x00funcq\x04c__main__\nsayhi\nq\x05u.'

Process finished with exit code 0
```

接著去打開 test.txt 觀察一下，裡面的資料


這個不是亂碼，而是 pickle 有自已一套的語法規則，很多人誤以為 pickle 優於 json ，說是因為 pickle 可以對代碼進行加密，其實這不是加密，只是存成了 **二進制**。

好，再來我們把存進去的資料給讀取出來

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import pickle

def sayhi(name):
    print("hello", name)
    print("hello2", name)


info = {
    'name':'tony',
    'age':22,
    'func':sayhi
}

f = open("test.txt", "rb")
data = pickle.loads(f.read())
# data = eval(f.read())
f.close()

print(data)
print(type(data["name"]))
print(data["name"])
print(type(data["age"]))
print(data["age"])
print(type(data["func"]))
print(data["func"]("Tony"))
print(type(data["func"]))
print(data["func"]("Daniel"))

---------------執行結果---------------
{'name': 'tony', 'age': 22, 'func': <function sayhi at 0x1008fa620>}
<class 'str'>
tony
<class 'int'>
22
<class 'function'>
hello Tony
hello2 Tony
None
hello Daniel
hello2 Daniel
None

Process finished with exit code 0
```

在二個程序中，內存地址不可能會一樣，因為在這二個內存地址是不能相互訪問的。
由上面代碼可知，pickle 可以把函數給序列化，而且只能在 python 程式語言中使用。


```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import pickle

def sayhi(name):
    print("hello", name)

info = {
    'name':'tony',
    'age':22,
    'func': sayhi
}


f = open("test1.txt","wb")
data = pickle.dump(info, f)    # 等於 f.write(pickle.dumps(info))
f.close()


print(type(data))
print(data)

---------------執行結果---------------
<class 'NoneType'>
None

Process finished with exit code 0
```

有 dump 就有 load

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import pickle

def sayhi(name):
    print("hello", name)


info = {
    'name':'tony',
    'age':22,
    'func':sayhi
}

f = open("test.txt", "rb")
data = pickle.load(f)   # 等於 pickle.loads(f.read())
f.close()

print(type(data["func"]))
print(data["func"]("Tony"))

---------------執行結果---------------
<class 'function'>
hello Tony
None

Process finished with exit code 0
```

剛剛的例子都只 dump 一次，那可以 dump 多次嗎？在 Python2.7 中是可以 dump 好多次，load 好多次，先被 dump 進去的，就會被先 load 出來，但在 Python3 中，load 的話，最多只能 load 一次，請看下面代碼，並觀察

先 dump 二筆資料到 `test2.txt`

```python
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import json

info = {
    'name':'tony',
    'age':22
}

f = open("test2.txt", "w")
f.write(json.dumps(info))
info['age'] = 21
f.write(json.dumps(info))
f.close()
```

會產生一個檔案叫 `test2.txt`，內容為底下

```
{"name": "tony", "age": 22}{"name": "tony", "age": 21}
```

再把剛剛的 `test2.txt` 給打印出來

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import json

f = open("test2.txt", "r")

for line in f:
    print(line)
f.close()

---------------執行結果---------------
{"name": "tony", "age": 22}{"name": "tony", "age": 21}

Process finished with exit code 0
```

再來用 `json.loads` 把它讀取出來

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import json

info = {
    'name':'tony',
    'age':22
}
f = open("test2.txt", "r")

for line in f:
    # print(line)
    print(json.loads(line))
f.close()

---------------執行結果---------------
Traceback (most recent call last):
  File "/Python/path/json反序列化3.py", line 14, in <module>
    print(json.loads(line))
  File "/usr/local/Cellar/python3/3.6.0/Frameworks/Python.framework/Versions/3.6/lib/python3.6/json/__init__.py", line 354, in loads
    return _default_decoder.decode(s)
  File "/usr/local/Cellar/python3/3.6.0/Frameworks/Python.framework/Versions/3.6/lib/python3.6/json/decoder.py", line 342, in decode
    raise JSONDecodeError("Extra data", s, end)
json.decoder.JSONDecodeError: Extra data: line 1 column 28 (char 27)

Process finished with exit code 1
```

會發現出現 **JSONDecodeError** ，這是因為剛說，在 Python3 中，最多就只能讀取一次，請在把 `test2.txt` 的內容修改成

```
{"name": "tony", "age": 22}
```

並在再次執行代碼

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import json

info = {
    'name':'tony',
    'age':22
}
f = open("test2.txt", "r")

for line in f:
    # print(line)
    print(json.loads(line))
f.close()

---------------執行結果---------------
{'name': 'tony', 'age': 22}

Process finished with exit code 0
```

這時候就可以正常執行了。

**Json 特性**

* json 文本序列化格，輸出時大都是被編碼為 utf-8
* json 較適合給人閱讀
* json 可在 Python 生態系統中使用之外，也廣泛地被使用。
* json 預設只能表示 Python 內建類型的子集，並且不能使用自定義的類。


**Pickle 特性**

* pickle 是一種二進制序列化格式
* pickle 不適合給人閱讀
* pickle 是 Python 特有的函數，只能在 Python 中使用。
* pickle 可以表示大量 Python 類型。



 


