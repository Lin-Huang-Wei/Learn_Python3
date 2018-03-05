### 列表的使用

如果想要存所有 Marvel's The Avengers 角色的人名，該如何存呢？請用目前已學到的知識來實做…

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
```

目前已存了五個人，透過空白來區分(也可以用逗號)，但該如何去取某一個人的值！？因為咱們已經把這五個人的人名存在一個變量裡了，所以沒辦法取，除非…可以像 Shell Script 透過 sed or awk 去做區分(喂~不能這樣做)，想改也沒辦法改，實務上，我們想存很多的訊息，並且透過字符串來存，就面臨了二個問題，一是不好存，二是不好改，所以後來發展出了新的數據類型，就叫 `列表`，在 Python 裡，使用列表時，就請用 **`[](中括號)`**。

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = []

print(names)

---------------執行結果---------------
[]

Process finished with exit code 0
```
這樣就打印出來一個空的列表，接下來把剛剛的人名給存在這列表裡吧…

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names)

---------------執行結果---------------

['Rogers', 'Stark', 'Thor', 'Loki', 'Natasha']

Process finished with exit code 0
```

這樣就把人名存進去了，接下來我們來取其中一個人名好了，抓 `Rogers`好了，該怎麼取呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names["Rogers"])

---------------執行結果---------------

Traceback (most recent call last):
  File "/Users/ironman/python/names.py", line 7, in <module>
    print(names["Rogers"])
TypeError: list indices must be integers or slices, not str

Process finished with exit code 1
```

結果發現報錯 `TypeError: list indices must be integers or slices, not str`，是因為不能這樣取值的，正確取值應該是…

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names[0])

---------------執行結果---------------

Rogers

Process finished with exit code 0
```

這樣就取出來了，但為什麼會是 0 呢？ `0`就代表 `Rogers`在這個列表中，從左到右開始的第一個位置，但在電腦存數據時，不是從1開始算的，是從 0 開始算起的，因為二進制使用的第一個狀態就是 0，所以從 0 開始算的，因此，我們可以透過位置來取得相對應的值。

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names[0], names[1])

---------------執行結果---------------

Rogers Stark

Process finished with exit code 0
```

接下來有沒有辦法一次就把 `Stark` 跟 `Thor` 給取出來？


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names[0], names[1])
print(names[1:2])

---------------執行結果---------------

Rogers Stark
['Stark']

Process finished with exit code 0
```

咦~怎麼只出現 `Stark` ？那在往後取一位試試…

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names[0], names[1])
print(names[1:3])

---------------執行結果---------------

Rogers Stark
['Stark', 'Thor']

Process finished with exit code 0
```

哦…這樣就取出來正確的了，那有沒有發現一個問題，就是只有顧到(包括) `起始位置`，而沒有顧到(不包括) `結束位置` ，所以我們叫這個是 **`顧頭不顧尾`** ，如果你要取的位置是連續的就可以用這個方式，而這個方式就叫做 `slices(切片)`。

假設今天我有一個很長的列表，不知道有幾個，但想取最後一個，該怎麼取呢？我們依上面的代碼，先取出最後一個值

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names[0], names[1])
print(names[1:3])
print(names[4])        # 在已知可數的情況下，就是我們一般取最後一個值的用法

---------------執行結果---------------

Rogers Stark
['Stark', 'Thor']
Natasha

Process finished with exit code 0
```

在不可數的情況下，也是一般較常用的寫法是

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names[0], names[1])
print(names[1:3])
print(names[4])
print(names[-1])

---------------執行結果---------------

Rogers Stark
['Stark', 'Thor']
Natasha
Natasha

Process finished with exit code 0
```

**正的就是從左到右取值，負的就是從右到左取值**，那如果接下想要取一次取最後二個值，該怎麼取呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names)
print(names[-1:-3])

---------------執行結果---------------

['Rogers', 'Stark', 'Thor', 'Loki', 'Natasha']
[]

Process finished with exit code 0
```

為什麼取出空列表哩，主要是因為 `切片` 還是一樣是 **`從左往右數`** 來取得數值的，所以我們應該要寫成 -3 開始取值

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names)
print(names[-3:-1])

---------------執行結果---------------

['Rogers', 'Stark', 'Thor', 'Loki', 'Natasha']
['Thor', 'Loki']

Process finished with exit code 0
```

又不對了，別忘記了，**`切片取值是顧頭不顧尾`**， 所以才會取出 `Thor` 跟 `Loki`，那在修改一下，參考下圖說明

![切片負值的說明](./IMG/16.png)

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names)
print(names[-2:-1])

---------------執行結果---------------

['Rogers', 'Stark', 'Thor', 'Loki', 'Natasha']
['Loki']

Process finished with exit code 0
```

又不對了，因為 `切片是顧頭不顧尾`，但又想取最後二個值，又該怎麼辦？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names)
print(names[-2:])

---------------執行結果---------------

['Rogers', 'Stark', 'Thor', 'Loki', 'Natasha']
['Loki', 'Natasha']

Process finished with exit code 0
```

這樣就可以正常取到了，有發現嗎？如果要取最後一個數值，其實 `省略不打`就可以取到了，那如果要正取 0 到 3 ，同樣的也是可以省略 0 ，代碼寫法如下

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]

print(names)
print(names[:3])

---------------執行結果---------------

['Rogers', 'Stark', 'Thor', 'Loki', 'Natasha']
['Rogers', 'Stark', 'Thor']

Process finished with exit code 0
```

接下來那我們又想要多加入一個人，該怎麼寫呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]
names.append("Barton")

print(names)

---------------執行結果---------------

['Rogers', 'Stark', 'Thor', 'Loki', 'Natasha', 'Barton']

Process finished with exit code 0
```
**`append()`** 意思就是附加，而且是加在最後面的，那如果想要隨意放在列表的任一位置呢？


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]
names.append("Barton")
names.insert(1, "Coulson")

print(names)

---------------執行結果---------------

['Rogers', 'Coulson', 'Stark', 'Thor', 'Loki', 'Natasha', 'Barton']

Process finished with exit code 0
```

**`insert()`** 就是插入任意你想要插入的位置，依上例代碼來說，就是要把 `Coulson` 插入到 `Stark` 前面，因此，填入的數字就 `1`，這樣就會插入在 `Stark` 前了，那如果要在新增一個人，到 `Stark`後面，又要怎麼寫呢？

``` 
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]
names.append("Barton")
names.insert(1, "Coulson")
names.insert(3, "Fury")

print(names)

---------------執行結果---------------

['Rogers', 'Coulson', 'Stark', 'Fury', 'Thor', 'Loki', 'Natasha', 'Barton']

Process finished with exit code 0
```

那要怎麼列表中的人名做取代的呢？ 

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]
names.append("Barton")
names.insert(1, "Coulson")
names.insert(3, "Fury")
names[2] = "Bruce"

print(names)

---------------執行結果---------------

['Rogers', 'Coulson', 'Bruce', 'Fury', 'Thor', 'Loki', 'Natasha', 'Barton']

Process finished with exit code 0
```

有發現了嘛， `Stark` 被取代成 `Bruce`了，這就是取代，那再來要怎麼刪除人名嗎？總共三種方法，這次刪除都以位置為 `1` 的 `Coulson` 做示範


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]
names.append("Barton")
names.insert(1, "Coulson")
names.insert(3, "Fury")
names[2] = "Bruce"

print(names)

# delete methods

# 1. names.remove("Coulson")

# 2. del names[1] = names.pop(1)

# 3. names.pop(1)

names.pop(1)

print(names)

---------------執行結果---------------

['Rogers', 'Coulson', 'Bruce', 'Fury', 'Thor', 'Loki', 'Natasha', 'Barton']
['Rogers', 'Bruce', 'Fury', 'Thor', 'Loki', 'Natasha', 'Barton']

Process finished with exit code 0
```
 
這三個方法都可以用來刪除，其中要特別說明的就是 `pop()` 這個刪除是最後一個，請看下圖跟代碼

![pop說明](./IMG/17.png)

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]
names.append("Barton")
names.insert(1, "Coulson")
names.insert(3, "Fury")
names[2] = "Bruce"

print(names)

names.pop()

print(names)

---------------執行結果---------------

['Rogers', 'Coulson', 'Bruce', 'Fury', 'Thor', 'Loki', 'Natasha', 'Barton']
['Rogers', 'Coulson', 'Bruce', 'Fury', 'Thor', 'Loki', 'Natasha']

Process finished with exit code 0
```

那如果要在列表中，尋找特定的一個人名是在哪一個位置，那該怎麼找呢？這次我們來找 `Loki` 好了

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]
names.append("Barton")
names.insert(1, "Coulson")
names.insert(3, "Fury")
names[2] = "Bruce"

print(names)
print(names.index("Loki"))

---------------執行結果---------------

['Rogers', 'Coulson', 'Bruce', 'Fury', 'Thor', 'Loki', 'Natasha', 'Barton']
5

Process finished with exit code 0
```

嗯，找到了，但如果我想找出位置後，並打印出相對應的值，該怎麼寫？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha"]
names.append("Barton")
names.insert(1, "Coulson")
names.insert(3, "Fury")
names[2] = "Bruce"

print(names)
print(names.index("Loki"))
print( names[names.index("Loki")] )

---------------執行結果---------------

['Rogers', 'Coulson', 'Bruce', 'Fury', 'Thor', 'Loki', 'Natasha', 'Barton']
5
Loki

Process finished with exit code 0
```

那如何在列表中，查詢有相同人名的有幾個？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = "Rogers Stark Thor Loki Natasha"
names = ["Rogers", "Stark", "Thor", "Loki", "Natasha", "Coulson"]
names.append("Barton")
names.insert(1, "Coulson")
names.insert(3, "Fury")
names[2] = "Bruce"

print(names)
print(names.count("Coulson"))

---------------執行結果---------------

['Rogers', 'Coulson', 'Bruce', 'Fury', 'Thor', 'Loki', 'Natasha', 'Coulson', 'Barton']
2

Process finished with exit code 0
```
