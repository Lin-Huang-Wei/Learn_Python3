### 裝飾器 - 高潮版


現在我們來模擬一個網站的使用者登入，透過 `裝飾器` 的方式來實現登入驗証功能。

**條件**

* 首頁不需要登入就可以使用
* 會員頁需要登入才能使用
* 討論區需要登入才能使用

先構思一個簡單版的。

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

def index():
    print("Welcome to Index")

@auth
def home():
    print("Welcome to Home")

@auth
def bbs():
    print("Welcome to BBS")
```

上面代碼已經完成，建立三個頁面，而且透過 `@auth` 裝飾器，把會員頁及討論區都有加入登入驗証的功能，接下來就來寫 `@auth` 這個裝飾器。

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

user, passwd = 'tony', 'stark'

def auth(func):
    def wrapper(*args, **kwargs):
        username = input("Username:").strip()
        password = input("Password:").strip()

        if username == user and password == passwd:
            print("\033[32;1mUser has passed authentication\033[0m")
            func(*args, **kwargs)
        else:
            exit("\033[31;1mInvalid Username or Password\033[0m")
    return wrapper

def index():
    print("Welcome to Index")

@auth
def home():
    print("Welcome to Home")

@auth
def bbs():
    print("Welcome to BBS")

index()
home()
bbs()

---------------執行第一次結果---------------
Welcome to Index
Username:tony
Password:stark
User has passed authentication
Welcome to Home
Username:tony
Password:stark
User has passed authentication
Welcome to BBS

Process finished with exit code 0

---------------執行第二次結果---------------
Welcome to Index
Username:tony
Password:stark
User has passed authentication
Welcome to Home
Username:tony
Password:hello
Invalid Username or Password

Process finished with exit code 1
```

第一次測試，全部都先輸入正確的，第二次測試，故意輸入錯誤的。

看起來首頁都確實沒有做驗証就可以使用，而會員頁及討論區都需要輸入帳號密碼來驗証。

再來請觀察一下，下面的代碼

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

user, passwd = 'tony', 'stark'

def auth(func):
    def wrapper(*args, **kwargs):
        username = input("Username:").strip()
        password = input("Password:").strip()

        if username == user and password == passwd:
            print("\033[32;1mUser has passed authentication\033[0m")
            func(*args, **kwargs)
        else:
            exit("\033[31;1mInvalid Username or Password\033[0m")
    return wrapper

def index():
    print("Welcome to Index")

@auth
def home():
    print("Welcome to Home")
    return "From Home"

@auth
def bbs():
    print("Welcome to BBS")

index()
print(home())
bbs()

---------------執行結果---------------
Welcome to Index
Username:tony
Password:stark
User has passed authentication
Welcome to Home
None
Username:tony
Password:stark
User has passed authentication
Welcome to BBS

Process finished with exit code 0
```

有發現執行結果中，有一個 `None` 嗎？為什麼會有 `None`呢？

先前的裝飾器雖然都沒有 `修改被裝飾的函數的源代碼`，也沒有 `修改被裝飾的函數的調用方式`，但卻改變了 `home函數` 的結果。

這是因為 `func(*args, **kwargs)` 的執行結果等於是 `home函數`的結果，即為 `From Home`，所以當去調用 `home()`時，其實就等於去調用 `wrapper函數`， `func(*args, **kwargs)` 沒有返回，所以 `home函數` 的執行結果才為 `None`。

那這樣要怎麼修改呢？

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

user, passwd = 'tony', 'stark'

def auth(func):
    def wrapper(*args, **kwargs):
        username = input("Username:").strip()
        password = input("Password:").strip()

        if username == user and password == passwd:
            print("\033[32;1mUser has passed authentication\033[0m")
            res = func(*args, **kwargs)   #From home
            str = "after authentication"
            print(str.center(50, '='))
            return  res
        else:
            exit("\033[31;1mInvalid Username or Password\033[0m")
    return wrapper

def index():
    print("Welcome to Index")

@auth
def home():
    print("Welcome to Home")
    return "From Home"

@auth
def bbs():
    print("Welcome to BBS")

index()
print(home())   #wrapper()
bbs()

---------------執行結果---------------

Welcome to Index
Username:tony
Password:stark
User has passed authentication
Welcome to Home
===============after authentication===============
From Home
Username:tony
Password:stark
User has passed authentication
Welcome to BBS
===============after authentication===============

Process finished with exit code 0
```

嗯！看起來 `home函數` 的執行結果也已經被正確給返回了。

接下來要正式進入正題了，由於目前使用的認証方式是採用 `local`的，但一般認証的方式除了local之外，還可以透過遠端的服務來做，例如： `LDAP`，所以我們要來模擬一下，如果同時要做 `local` 及 `ldap` 驗証登入的話，該怎麼做呢？！


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

user, passwd = 'tony', 'stark'

def auth(func):
    print("auth func:", func)
    def outer_wrapper():
        def wrapper(*args, **kwargs):
            print("wrapper func agrs:", *args, **kwargs)
            username = input("Username:").strip()
            password = input("Password:").strip()

            if username == user and password == passwd:
                print("\033[32;1mUser has passed authentication\033[0m")
                res = func(*args, **kwargs)  # From home
                str = "after authentication"
                print(str.center(50, '='))
                return res
            else:
                exit("\033[31;1mInvalid Username or Password\033[0m")

        return wrapper
    return outer_wrapper

def index():
    print("Welcome to Index")

@auth(auth_type="local")
def home():
    print("Welcome to Home")
    return "From Home"

@auth(auth_type="ldap")
def bbs():
    print("Welcome to BBS")

index()
print(home())   #wrapper()
bbs()

---------------執行結果---------------

Traceback (most recent call last):
  File "/Python/path/pratice_decorator4.py", line 29, in <module>
    @auth(auth_type="local")
TypeError: auth() got an unexpected keyword argument 'auth_type'

Process finished with exit code 1
```

跟著錯誤修改，因為 `auth函數` 需要傳入參數

* `auth(func)` 修改成 `auth(auth_type)`
*  `print("auth func:", func)` 修改成 `print("auth auth_type:", auty_type )` 

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

user, passwd = 'tony', 'stark'

def auth(auth_type):
    print("auth auth_type:", auth_type)
    def outer_wrapper():
        def wrapper(*args, **kwargs):
            print("wrapper func agrs:", *args, **kwargs)
            username = input("Username:").strip()
            password = input("Password:").strip()

            if username == user and password == passwd:
                print("\033[32;1mUser has passed authentication\033[0m")
                res = func(*args, **kwargs)  # From home
                str = "after authentication"
                print(str.center(50, '='))
                return res
            else:
                exit("\033[31;1mInvalid Username or Password\033[0m")

        return wrapper
    return outer_wrapper

def index():
    print("Welcome to Index")

@auth(auth_type="local")
def home():
    print("Welcome to Home")
    return "From Home"

@auth(auth_type="ldap")
def bbs():
    print("Welcome to BBS")

index()
print(home())   #wrapper()
bbs()

---------------執行結果---------------

auth auth_type: local
Traceback (most recent call last):
  File "/Python/path/pratice_decorator4.py", line 29, in <module>
    @auth(auth_type="local")
TypeError: outer_wrapper() takes 0 positional arguments but 1 was given

Process finished with exit code 1
```

跟著錯誤再次修改，`outer_wrapper()` 需要 0 個位置參數，但卻給了一個參數，這是因為原本的 `func` 被刪除了， 其實 `func` 就要往下一層放

* `def outer_wrapper():` 修改成 `def outer_wrapper(func):`


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

user, passwd = 'tony', 'stark'

def auth(auth_type):
    print("auth auth_type:", auth_type)
    def outer_wrapper(func):
        def wrapper(*args, **kwargs):
            print("wrapper func agrs:", *args, **kwargs)
            username = input("Username:").strip()
            password = input("Password:").strip()

            if username == user and password == passwd:
                print("\033[32;1mUser has passed authentication\033[0m")
                res = func(*args, **kwargs)  # From home
                str = "after authentication"
                print(str.center(50, '='))
                return res
            else:
                exit("\033[31;1mInvalid Username or Password\033[0m")

        return wrapper
    return outer_wrapper

def index():
    print("Welcome to Index")

@auth(auth_type="local")
def home():
    print("Welcome to Home")
    return "From Home"

@auth(auth_type="ldap")
def bbs():
    print("Welcome to BBS")

index()
print(home())   #wrapper()
bbs()

---------------執行結果---------------

auth auth_type: local
auth auth_type: ldap
Welcome to Index
wrapper func agrs:
Username:tony
Password:stark
User has passed authentication
Welcome to Home
===============after authentication===============
From Home
wrapper func agrs:
Username:tony
Password:stark
User has passed authentication
Welcome to BBS
===============after authentication===============

Process finished with exit code 0
```

其實上面代碼已經可以正常執行了，但卻還沒有加入判斷 `local` 或是 `ldap` 驗証的方法，所以再修改一下代碼

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

import time

user, passwd = 'tony', 'abc123'

def auth(auth_type):
    print("auth func:", auth_type)
    def outer_wrapper(func):
        def wrapper(*args, **kwargs):
            print("wrapper func args:", *args, **kwargs)
            if auth_type == "local":
                username = input("Username:")
                password = input("Password:")

                if user == username and passwd == password:
                    print("\033[33;1mUser has passed authentication\033[0m")
                    res = func(*args, **kwargs)
                    print("---after authentication")
                    return res
                else:
                    exit("\033[31;1mInvaild Username or Password\033[0m")
            elif auth_type == "ldap":
                print("我不會LDAP")

        return wrapper
    return outer_wrapper


def index():
    print("Welcome to Index Page")

@auth(auth_type="local")
def home():
    print("Welcome to Home Page")
    return "From Home"

@auth(auth_type="ldap")
def bbs():
    print("Welcome to BBS Page")

index()
print(home())
bbs()
```


如果這上面的過程還是有不了解的，建議可以用 Pycharm 去下中斷點，慢慢去了解，以上這就是完整的裝飾器介紹。
