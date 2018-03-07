### 淺copy 補充說明

在 `import copy` 這個模塊裡

基於第一個列表來做淺copy，實際上第二個列表裡的元素，是第一個列表的 `引用`。

接下來介紹 `淺copy`有三種方式可以使用

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import copy

person = ["name", ["saving", 100]]

p1 = copy.copy(person)    # method1
p2 = person[:]            # method2
p3 = list(person)         # method3

print(p1)
print(p2)
print(p3)

---------------執行結果---------------

['name', ['saving', 100]]
['name', ['saving', 100]]
['name', ['saving', 100]]

Process finished with exit code 0
```

接下來我們做一個有關夫妻關係的 `淺copy` 列表

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import copy

person = ["name", ["saving", 100]]

p1 = person[:]
p2 = person[:]

print(p1)
print(p2)

---------------執行結果---------------

['name', ['saving', 100]]
['name', ['saving', 100]]

Process finished with exit code 0
```

`p1` 跟 `p2` 都是基於 `person` 淺copy得來的，看上去都是一樣的，所以我們再來修改一下

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import copy

person = ["name", ["saving", 100]]

p1 = person[:]
p2 = person[:]

p1[0]="Stark"
p2[0]="Potts"

print(p1)
print(p2)

---------------執行結果---------------

['Stark', ['saving', 100]]
['Potts', ['saving', 100]]

Process finished with exit code 0
```

嗯，觀察出，原本 `person` 列表中的 `names` 已經被修改過了，接下來我們假設這夫妻合開一個銀行帳戶，當 `Stark` 去領錢時， `Potts` 的存款應該也是會短少的

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import copy

person = ["name", ["saving", 100]]


p1 = person[:]
p2 = person[:]

p1[0]="Stark"
p2[0]="Potts"

p1[1][1]=50

print(p1)
print(p2)

---------------執行結果---------------
['Stark', ['saving', 50]]
['Potts', ['saving', 50]]

Process finished with exit code 0
```

由上面結果得知 `Potts`的帳戶也短少了50塊，這就是 `淺copy`使用 `聯合帳戶` 時的一個範例。


