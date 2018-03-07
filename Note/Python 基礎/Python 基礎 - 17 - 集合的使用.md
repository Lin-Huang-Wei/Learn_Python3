### 集合的使用

集合是一個無序的，不重複的數據組合，主要的作用如下

* `去重`，把一個列表變成集合，就會自動去重了。
* `關係測試`，測試二組數據之前的交集、差集、聯集等關係。


接下來我們來實作看看什麼是`去重`

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

print(list_1, type(list_1))

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} <class 'set'>

Process finished with exit code 0
```

觀察一下，發現原本有重複出現的數字已經不見了，而且這個列表也已經變成一個集合了

接下來我們來試試`關係測試`

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_2 = set([0, 2, 6, 66, 22, 8, 4])

print(list_1, list_2)

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} {0, 2, 66, 4, 6, 8, 22}

Process finished with exit code 0
```

觀察上面代碼，這二個集合中，有沒有二個一樣的數字？那如何將這二個一樣的數字給取出來呢？


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_2 = set([0, 2, 6, 66, 22, 8, 4])

print(list_1, list_2)

print(list_1.intersection(list_2))   # 交集

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} {0, 2, 66, 4, 6, 8, 22}
{4, 6}

Process finished with exit code 0
```

唔…成功取出來了，`{4, 6}`就是這二個集合的`交集`，所謂`交集`就是二個集合裡面都有的東西，A和B的交集寫作`A ∩ B`

如果做二個集合的`聯集`要怎麼取呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_2 = set([0, 2, 6, 66, 22, 8, 4])

print(list_1, list_2)

print(list_1.union(list_2))          # 聯集

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} {0, 2, 66, 4, 6, 8, 22}
{0, 1, 2, 3, 4, 5, 6, 7, 66, 9, 8, 22}

Process finished with exit code 0
```

唔…在觀察一下，發現這二個集合被合併成一個集合了，並且也做了`去重`，這個就叫做`聯集`，	A 和 B 的聯集通常寫作 `A ∪ B`

如果做二個集合的`差集`要怎麼取呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_2 = set([0, 2, 6, 66, 22, 8, 4])

print(list_1, list_2)

print(list_1.difference(list_2))     # 差集 in list_1 but not in list_2

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} {0, 2, 66, 4, 6, 8, 22}
{1, 3, 5, 9, 7}

Process finished with exit code 0
```

觀察一下，可以想像成把 `list_1 這個集合減去 list_1 跟 list_2 的交集`，就會是 list_1 的`差集`了，那…`list_2`的差集會是長什麼樣子呢？


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_2 = set([0, 2, 6, 66, 22, 8, 4])

print(list_1, list_2)

print(list_1.difference(list_2))     # 差集 in list_1 but not in list_2
print(list_2.difference(list_1))

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} {0, 2, 66, 4, 6, 8, 22}
{1, 3, 5, 9, 7}
{0, 8, 2, 66, 22}

Process finished with exit code 0
```

唔…在仔細觀察一下，list_2 的差集有什麼不同！是不是也發現`{4, 6}`這二個數字也不見了，只保留了`{0, 8, 2, 66, 22}`


除了這三個之外，還有沒有別的關係？是有的

接下來我們來試試`子集`

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_2 = set([0, 2, 6, 66, 22, 8, 4])

print(list_1, list_2)

print(list_1.issubset(list_2))       # 子集

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} {0, 2, 66, 4, 6, 8, 22}
False

Process finished with exit code 0
```

咦，出現 `False`，為什麼會出現false呢？

是因為 list_1 這個集合裡的數字，沒有完全符合 list_2 這個集合裡的數字，所以才會是 `False`，那有子集就會有`父集`，那就來試試看 list_2 是不是 list_1 的父集？


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_2 = set([0, 2, 6, 66, 22, 8, 4])

print(list_1, list_2)

print(list_1.issubset(list_2))       # 子集
print(list_2.issuperset(list_1))     # 父集

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} {0, 2, 66, 4, 6, 8, 22}
False
False

Process finished with exit code 0
```

接下來我們來新增一個集合 `list_3`，再試試剛剛那個`子集`跟`父集`，觀察一下，有什麼不同？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_2 = set([0, 2, 6, 66, 22, 8, 4])
list_3 = set([1, 3, 7])

print(list_1, list_2, list_3)

print(list_3.issubset(list_1))       # 子集
print(list_1.issuperset(list_3))     # 父集

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} {0, 2, 66, 4, 6, 8, 22} {1, 3, 7}
True
True

Process finished with exit code 0
```

唔… `list_3` 是 `list_1` 的子集，反過來說，`list_1` 是 `list_3` 的`父集`

再來試試`對稱差集`，觀察一下看看有什麼不同

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_2 = set([0, 2, 6, 66, 22, 8, 4])
list_3 = set([1, 3, 7])

print(list_1, list_2, list_3)

print(list_1.symmetric_difference(list_2))

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} {0, 2, 66, 4, 6, 8, 22} {1, 3, 7}
{0, 1, 2, 66, 3, 5, 7, 8, 9, 22}

Process finished with exit code 0
```

唔，就是把二個集合裡所沒有的元素給取出來，所以就取出了`{0, 1, 2, 66, 3, 5, 7, 8, 9, 22}`，而`{4, 6}`是這二個集合都有的，所以就不取了


再來我們在新增一個集合叫 `list_4`，當二個集合沒有交集的話，要怎麼判斷？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_2 = set([0, 2, 6, 66, 22, 8, 4])
list_3 = set([1, 3, 7])
list_4 = set([5, 6, 8])

print(list_1, list_2, list_3)

print(list_3.isdisjoint(list_4))		# Return True if two sets have a null intersection.

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} {0, 2, 66, 4, 6, 8, 22} {1, 3, 7}
True

Process finished with exit code 0
```
唔，有發現結果回應 `True`，就代表 `set.isdisjoint()` 是`判斷當二個集合沒有交集`時，就返回 `True`，那我在修改一下 `list_4`，在觀察一下

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_2 = set([0, 2, 6, 66, 22, 8, 4])
list_3 = set([1, 3, 7])
list_4 = set([5, 6, 7, 8])

print(list_1, list_2, list_3)

print(list_3.isdisjoint(list_4))

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} {0, 2, 66, 4, 6, 8, 22} {1, 3, 7}
False

Process finished with exit code 0
```

嗯！結果返回一個 `False` ，就代表 `list_3` 跟 `list_4` 是有`交集`的

用符號來表示`交集`、`聯集`、`差集`、`對稱差集`

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_2 = set([0, 2, 6, 66, 22, 8, 4])
list_3 = set([1, 3, 7])
list_4 = set([5, 6, 7, 8])

print(list_1, list_2, list_3)

print(list_1 & list_2)          # 交集(intersection)
print(list_1 | list_2)          # 聯集(Union)
print(list_1 - list_2)          # 差集(difference) in list_1 not in list_2
print(list_1 ^ list_2)          # 對稱差集(symmetric_difference)

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9} {0, 2, 66, 4, 6, 8, 22} {1, 3, 7}
{4, 6}
{0, 1, 2, 3, 4, 5, 6, 7, 66, 9, 8, 22}
{1, 3, 5, 9, 7}
{0, 1, 2, 66, 3, 5, 7, 8, 9, 22}

Process finished with exit code 0
```


再來我們來操作集合的新增、修改、刪除，先試試對一個集合做新增

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_1.add(44)
print(list_1)

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 9, 44}

Process finished with exit code 0
```

再來新增多個數字到集合裡

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_1.add(44)
list_1.update([9527, 520, 1314])
print(list_1)

---------------執行結果---------------

{1, 1314, 3, 4, 5, 6, 7, 520, 9, 44, 9527}

Process finished with exit code 0
```

再來試試刪除的方法

**Method 1: set.remove() 刪除元素，但刪除一個不存在的元素，會噴 error**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_1.add(44)
list_1.update([9527, 520, 1314])
list_1.remove(1314)
print(list_1)

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 520, 9, 44, 9527}

Process finished with exit code 0
```

**Method 2: set.pop() 隨機任意刪，並且打印出刪除的元素**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_1.add(44)
list_1.update([9527, 520, 1314])
list_1.remove(1314)
print(list_1)
print(list_1.pop())
print(list_1.pop())
print(list_1.pop())
print(list_1.pop())
print(list_1.pop())
print(list_1.pop())
print(list_1.pop())
print(list_1)

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 520, 9, 44, 9527}
1
3
4
5
6
7
520
{9, 44, 9527}

Process finished with exit code 0
```

**Method 3: set.discard() 如果元素存在，就刪除，元素不存在，也不會噴 error**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_1.add(44)
list_1.update([9527, 520, 1314])
list_1.remove(1314)
print(list_1)

list_1.discard(9)
list_1.discard(999)    # 故意刪除一個不存在的，也不會報錯
print(list_1)

---------------執行結果---------------

{1, 3, 4, 5, 6, 7, 520, 9, 44, 9527}
{1, 3, 4, 5, 6, 7, 520, 44, 9527}

Process finished with exit code 0
```

計算集合的長度

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-


list_1 = [1, 4, 5, 7, 3, 6, 7, 9]
list_1 = set(list_1)

list_1.add(44)
list_1.update([9527, 520, 1314])
list_1.remove(1314)
print(len(list_1))
print(list_1)

---------------執行結果---------------

10
{1, 3, 4, 5, 6, 7, 520, 9, 44, 9527}

Process finished with exit code 0
```







參考資料：

* [交集wiki](https://zh.wikipedia.org/wiki/%E4%BA%A4%E9%9B%86)
* [聯集wiki](https://zh.wikipedia.org/wiki/%E5%B9%B6%E9%9B%86)
* [差集wiki](https://zh.wikipedia.org/wiki/%E8%A1%A5%E9%9B%86)
* [子集wiki](https://zh.wikipedia.org/wiki/%E5%AD%90%E9%9B%86)


