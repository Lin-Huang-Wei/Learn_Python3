### 局部變量、全局變量和作用域

**局部變量**

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def change_name(name):
    print("before change", name)
    name = "Tony Stark"    # 這個函數就是這個變量的作用域，出了這個函數就無效了
    print("after change", name)

name = "tony"
change_name(name)
print(name)

---------------執行結果---------------

before change tony
after change Tony Stark
tony

Process finished with exit code 0
```

局部變量只在函數裡生效，也就是說這個函數 `def change_name(name)` 就這個變量 `name` 的**作用域**

**全局變量**

就是整個程序中都生效的變量

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

school = "test edu"    # 全局變量

def change_name(name):
    school = "test1 edu"    # 局部變量
    print("before change", name, school)
    name = "Tony Stark"     # 局部變量
    print("after change", name, school)

name = "tony"
change_name(name)
print(name)
print(school)

---------------執行結果---------------

before change tony test1 edu
after change Tony Stark test1 edu
tony
test edu

Process finished with exit code 0
```

`函數默認局部變量無法修改全局變量`，那如果要修改的話，要怎麼做呢？

```
school = "Harvard"

def change_name(name):
    global school
    school = "UCLA"
    print("before change", name, school)
    name = "Tony Stark"
    print("after change", name, school)

name = "tony"
change_name(name)
print(name)
print("school:", school)

---------------執行結果---------------

before change tony UCLA
after change Tony Stark UCLA
tony
school: UCLA

Process finished with exit code 0
```

觀察上面代碼 `global school` 加了這行，就可以讓局部變量變成全局變量

---

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def change_name():
    global name
    name = "tony"

change_name()
print(name)

---------------執行結果---------------

tony

Process finished with exit code 0
```

上面的代碼中，在最外面沒有定義一個全局變量 `name`，卻在函數裡去定義一個全局變量 `global name` ，在函數裡面去強迫修改一個全局變量，這會出現一個問題，你不會知道在程序中，會不會正常的被調用，這是一個完完全全的錯誤的用法，請勿使用。


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = ["tony", "loki", "thor"]

def change_name():
    names[0] = "ironman"
    print("inside function:", names)

change_name()
print("outside function:", names)

---------------執行結果---------------

inside function: ['ironman', 'loki', 'thor']
outside function: ['ironman', 'loki', 'thor']

Process finished with exit code 0
```
觀察上面代碼，只有`字符串跟整數`，是不能在函數裡的局部變量做修改的， `tuple(元組) 本來就不能修改的`，可以修改的有`列表、字典、集合、類`。

**結論**

1. 在程序的一開始定義的就叫做`全局變量`，在子程序中定義的變量就叫做`局部變量`
2. 全局變量的作用域是整個程序，而局部變量的作用域是定義該變量的子程序中生效
3. 當全局變量與局部變量同名時，定義局部變量的子程序才會起作用，而在其他地方全局變量起作用

