### 遞歸

**遞歸特性**

1. 必須要有一個明確的結束條件。
2. 每次進入更深一層的遞歸，問題規模會比上次遞歸都應該會有所減少。
3. 遞歸效率不高，遞歸層次過多會導致堆疊溢出（在計算機中，函數調用是通過堆疊 （stack） 這種數據結構實現的，每當進入一個函數調用，棧就會加一層堆疊，每當函數返回，棧就會減一層棧幀。由於棧的大小不是無限的，所以，遞歸調用的次數過多，會導致棧溢出）


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def calc(n):
    print(n)
    return  calc(n+1)

calc(0)

---------------執行結果---------------

0
1
2
3
4
....
996
Traceback (most recent call last):
  File "/Python/project/path/遞歸.py", line 9, in <module>
    calc(0)
  File "/Python/project/path/遞歸.py", line 6, in calc
    return  calc(n+1)
  File "/Python/project/path/遞歸.py", line 6, in calc
    return  calc(n+1)
  File "/Python/project/path/遞歸.py", line 6, in calc
    return  calc(n+1)
  [Previous line repeated 993 more times]
  File "/Python/project/path/遞歸.py", line 5, in calc
    print(n)
RecursionError: maximum recursion depth exceeded while calling a Python object

Process finished with exit code 1
```

最多有 999 層。

接下來練習寫一個簡單的遞歸

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def calc(n):
    print(n)
    if int(n/2) > 0:
        return  calc(int(n/2))
    print("->", n)

calc(10)

---------------執行結果---------------
10
5
2
1
-> 1

Process finished with exit code 0
```


