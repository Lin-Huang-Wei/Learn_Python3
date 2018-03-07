### 字典的操作使用

接下來介紹`字典`，這在未來工作上，會是很常使用的，就來好好了解一下唄…

字典是一個 `key(鍵)-value(值)` 的數據類型，可以儲存很多訊息

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

print(info)

---------------執行結果---------------

{'stu1001': 'Tony Stark', 'stu1003': 'Bruce Banner', 'stu1002': 'Steve Rogers'}

Process finished with exit code 0
```

觀察一下，有發現什麼了嗎？沒錯，`字典怎麼沒有按照順序排列呢？`

字典的其中一個特性是`無順序性的`，因為`沒有下標`，所以不會照順序排列，但仍然可以透過`key`來查找，字典不像`列表是有下標的`，可以透過下標來查找位置，先試試用`key`來查找`Tony Stark`，查找有二種方法

**Method 1：**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

print(info)
print(info["stu1001"])			# 這種查找方式是要百分百確定key有存在於字典的，否則會報錯
print(info)

---------------執行結果---------------

{'stu1001': 'Tony Stark', 'stu1003': 'Bruce Banner', 'stu1002': 'Steve Rogers'}
Tony Stark

Process finished with exit code 0
```

**Method 2：dict.get()**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

print(info)

print(info.get('stu1001'))			# 比較安全查找的方式
print(info.get('stu1004'))			# 故意查找不存在的，只會返回none

---------------執行結果---------------

{'stu1001': 'Tony Stark', 'stu1003': 'Bruce Banner', 'stu1002': 'Steve Rogers'}
Tony Stark
None

Process finished with exit code 0
```

那有沒有一個方法，可以判斷我的`字典`裡，這個數據存不存在？存在就取值，不存在就建立

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

print(info)

print('stu1003' in info)    # Pyhton2.7版，會寫 info.has_key('stu1003')，效果是一樣的
print('stu1004' in info) 

---------------執行結果---------------

{'stu1003': 'Bruce Banner', 'stu1002': 'Steve Rogers', 'stu1001': 'Tony Stark'}
True
False

Process finished with exit code 0
```

再把 `Tony Stark` 改成 `東尼史塔克`，看看要怎麼修改…

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

print(info)
info["stu1001"] = "東尼史塔克"
print(info)

---------------執行結果---------------

{'stu1001': 'Tony Stark', 'stu1003': 'Bruce Banner', 'stu1002': 'Steve Rogers'}
{'stu1001': '東尼史塔克', 'stu1003': 'Bruce Banner', 'stu1002': 'Steve Rogers'}

Process finished with exit code 0
```

那再新增一個人員進來呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

print(info)
info["stu1001"] = "東尼史塔克"
info["stu1004"] = "復仇者聯盟"
print(info)

---------------執行結果---------------

{'stu1002': 'Steve Rogers', 'stu1003': 'Bruce Banner', 'stu1001': 'Tony Stark'}
{'stu1004': '復仇者聯盟', 'stu1002': 'Steve Rogers', 'stu1003': 'Bruce Banner', 'stu1001': '東尼史塔克'}

Process finished with exit code 0
```

那要在刪除字典中，不想要的資料，要怎麼刪除呢？ 總共有三種方法

**Method 1：del**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

print(info)
#print(info["stu1001"])
info["stu1001"] = "東尼史塔克"
info["stu1004"] = "復仇者聯盟"

del info["stu1004"]     # Python 內建的通用刪除方法，可以用來刪字典，也可以刪列表，任何物件
print(info)

---------------執行結果---------------

{'stu1002': 'Steve Rogers', 'stu1001': 'Tony Stark', 'stu1003': 'Bruce Banner'}
{'stu1002': 'Steve Rogers', 'stu1001': '東尼史塔克', 'stu1003': 'Bruce Banner'}

Process finished with exit code 0
```

**Method 2：dic.pop()**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

print(info)
#print(info["stu1001"])
info["stu1001"] = "東尼史塔克"
info["stu1004"] = "復仇者聯盟"

info.pop("stu1004")     # 一定要指定一個key，不指定就會報錯
print(info)

---------------執行結果---------------

{'stu1001': 'Tony Stark', 'stu1003': 'Bruce Banner', 'stu1002': 'Steve Rogers'}
{'stu1001': '東尼史塔克', 'stu1003': 'Bruce Banner', 'stu1002': 'Steve Rogers'}

Process finished with exit code 0
```

**Method 3：dic.popitem()**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

print(info)
#print(info["stu1001"])
info["stu1001"] = "東尼史塔克"
info["stu1004"] = "復仇者聯盟"

info.popitem()     # 如果沒指定，就會隨機刪
print(info)

---------------執行結果---------------

{'stu1002': 'Steve Rogers', 'stu1003': 'Bruce Banner', 'stu1001': 'Tony Stark'}
{'stu1003': 'Bruce Banner', 'stu1001': '東尼史塔克', 'stu1004': '復仇者聯盟'}

Process finished with exit code 0
```

接下來，我們只想打印出，字典中所有的值

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

print(info.values())

---------------執行結果---------------

dict_values(['Tony Stark', 'Steve Rogers', 'Bruce Banner'])

Process finished with exit code 0
```

看起來成功了，那既然可以只打印值，應該也可以只打印字典中所有的key

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

print(info.keys())

---------------執行結果---------------

dict_keys(['stu1001', 'stu1002', 'stu1003'])

Process finished with exit code 0
```

唔，成功打印出所有的key了，再來我們來新增一個成員好了

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

info.setdefault('stu1007', "Phil Coulson")
print(info)

---------------執行結果---------------

{'stu1003': 'Bruce Banner', 'stu1001': 'Tony Stark', 'stu1007': 'Phil Coulson', 'stu1002': 'Steve Rogers'}

Process finished with exit code 0
```

唔，看起來也成功新增一個人員了，那假設如果用`dict.setdefault('key', 'value')` 這種語法，能不能拿來修改已存在的值，來試試唄

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

info.setdefault('stu1007', "Phil Coulson")
print(info)
info.setdefault('stu1003', "Nick Fury")
print(info)

---------------執行結果---------------

{'stu1002': 'Steve Rogers', 'stu1007': 'Phil Coulson', 'stu1003': 'Bruce Banner', 'stu1001': 'Tony Stark'}
{'stu1002': 'Steve Rogers', 'stu1007': 'Phil Coulson', 'stu1003': 'Bruce Banner', 'stu1001': 'Tony Stark'}

Process finished with exit code 0
```

嗯，看起來 `stu1003`的值，並沒有被改變，所以`dict.setdefault('key', 'value)`的主要功能就是把`不存在於字典中的key，給建立起來`

試試更新字典呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

info2 = {
    'stu1001': "Jarvis",
    'stu2002': "Maria Hill",
}

info.update(info2)
print(info)

---------------執行結果---------------

{'stu1002': 'Steve Rogers', 'stu2002': 'Maria Hill', 'stu1001': 'Jarvis', 'stu1003': 'Bruce Banner'}

Process finished with exit code 0
```

觀察一下，發現當二個字典中，有二個一樣的`key`時，如果使用了`dict.update()` 就會被更新了。

以上面代碼為例，這二個字典中，都有一個`key`叫 `stu1001`，但二個分別代表不同的值，
`info{'stu1001': "Tony Stark"}`，而 `info2{'stu1001': "Jarvis"}`，當執行`info.update(info2)`時，是`把二個字典給合併`了，如果有對到一樣的`key`就更新，沒有的話，就建立一組新的key-value，這就是`dict.update()`的作用。

接下來試試`dict.items()`，觀察一下，有什麼作用

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

print(info.items())

---------------執行結果---------------

dict_items([('stu1003', 'Bruce Banner'), ('stu1001', 'Tony Stark'), ('stu1002', 'Steve Rogers')])

Process finished with exit code 0
```
唔…看起來是把字典給轉成列表了。

接下來試一下怎麼做`初始化字典`

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

info2 = info.fromkeys([9, 8, 7])
print(info2)

---------------執行結果---------------

{8: None, 9: None, 7: None}

Process finished with exit code 0
```

咦，奇怪，為什麼不是用`info.fromkeys([9, 8, 7])`嗎？怎麼打印出來的全都不一樣了，那是因為其實跟`info`這個字典無關了，所以直接用`dict.fromkeys(seq, value=none)`來驗証一下吧

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

info2 = dict.fromkeys([9, 8, 7], "test")
print(info2)

---------------執行結果---------------

{8: 'test', 9: 'test', 7: 'test'}

Process finished with exit code 0
```
這次就不透過`info.fromkeys()`來調用了，直接使用`dict.fromkeys()`，這樣就完成一個初始化字典啦，並且還直接賦值了。

再來觀察一下，下面這個代碼

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

info2 = dict.fromkeys([9, 8, 7], [1, {"name":"ironman"}, 9527])
print(info2)
info2[8][1]["name"] = "Bruce Lee"
print(info2)

---------------執行結果---------------

{8: [1, {'name': 'ironman'}, 9527], 9: [1, {'name': 'ironman'}, 9527], 7: [1, {'name': 'ironman'}, 9527]}
{8: [1, {'name': 'Bruce Lee'}, 9527], 9: [1, {'name': 'Bruce Lee'}, 9527], 7: [1, {'name': 'Bruce Lee'}, 9527]}

Process finished with exit code 0
```

有發現什麼嗎？我們原以為是初始化了三個獨立的數據，其實不是的，這個就跟之前講到的`淺copy`有點類似，看下圖說明好了

![字典fromkeys的踩坑說明](./IMG/19.png)

`dict.fromkeys() 較適用於一層式的字典，多層式的字典就不要用了`

再來試一下，字典的循環是怎麼使用

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

for i in info:
    print(i)

---------------執行結果---------------

stu1001
stu1003
stu1002

Process finished with exit code 0
```

咦，只有打印出key，如果也想把值給打印出來呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

for i in info:
    print(i, info[i])

---------------執行結果---------------

stu1001 Tony Stark
stu1003 Bruce Banner
stu1002 Steve Rogers

Process finished with exit code 0
```

還有另一種方法也可以同時打印出 `key-value`的循環

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


info = {
    'stu1001': "Tony Stark",
    'stu1002': "Steve Rogers",
    'stu1003': "Bruce Banner",
}

for k, v in info.items():
    print(k, v)

---------------執行結果---------------

stu1003 Bruce Banner
stu1002 Steve Rogers
stu1001 Tony Stark

Process finished with exit code 0 
```

那這二種有什麼差別呢？其實第一種的效率比較好，因為`第一種是透過 key 的方式去取值`，但`第二種多了一個把字典轉成列表的過程`，如果當數據量較大時，就不適合用第二種方式。

接下來，再寫稍稍複雜一點的，`巢狀式(嵌套)(nested)字典`，並修改掉其中一個值

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

shopping_online_catalog = {
    "歐美": {
        "www.amazon.com": ["美國最大購物網站", "亞馬遜"],
        "www.ebay.com": ["美國知名購物網站", "eBay"],
        "www.kickstarter.com": ["美國募資平台", "有很多有趣的專案在募資"]
    },

    "日本": {
        "media.buyee.jp": ["日本代購網站", "不會日文也可以買，日本雅虎官方合作夥伴"],
        "global.rakuten.com": ["日本樂天網站", "好像是日本第一大購物網站？"]
    },

    "大陸": {
        "www.taobao.com": ["大陸淘寶網", "應該是大陸最大的購物網站吧？！"],
        "www.jd.com": ["大陸京東商城", "其實我也不知道它在賣啥的"]
    }
}

---------------執行結果---------------

{'大陸': {'www.taobao.com': ['大陸淘寶網', '應該是大陸最大的購物網站吧？！'], 'www.jd.com': ['大陸京東商城', '我只知道他們技術很牛逼']}, '日本': {'media.buyee.jp': ['日本代購網站', '不會日文也可以買，日本雅虎官方合作夥伴'], 'global.rakuten.com': ['日本樂天網站', '好像是日本第一大購物網站？']}, '歐美': {'www.amazon.com': ['美國最大購物網站', '亞馬遜'], 'www.kickstarter.com': ['美國募資平台', '有很多有趣的專案在募資'], 'www.ebay.com': ['美國知名購物網站', 'eBay']}}

Process finished with exit code 0
```

`key` 盡量不要使用中文字，很容易因為字符編碼不一致，而導致取不到值或是程序失敗




參考資料：

* [howsoftworks](http://www.howsoftworks.net/python.api/builtins/dict_fromkeys.html)
* [字典(Dictionary) fromkeys()方法](http://www.runoob.com/python/att-dictionary-fromkeys.html)

