### 元組 (tuple) 的使用與簡易購物車實作

其實跟列表差不多，也是存一組數，只不過是它一旦建立了，就不能修改了，只能做 `切片` 跟 `查詢`，所以只叫 `只讀列表`

語法： name = ("Rogers", "Stark", "Thor", "Loki")

它只有二個方法可以使用，一個是 `count()`、一個是 `index()`。接下來就簡單試試這二個功能怎麼用

```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

names = ("Rogers", "Stark", "Thor", "Loki", "Thor", "Thor")

print(names.index("Stark"))
print(names.count("Thor"))

---------------執行結果---------------

1
3

Process finished with exit code 0
```

什麼時候才會使用到 `tuple(元組)`，一般大都是會用在有連接資料庫的文件時，當你要設定不能變動的設定，如：IP 位置，Port 號等，就可以使用元組，當服務啟動時，就只吃你設定好的元組，假設有別人再去改動你的設定檔時，會報錯，再來就是使用元組也可以提醒別人，這個配置請不要隨便修改跟異動。


接下來要應用之前所學到的，來建構一個簡單的購物車

目前的需求是：

* 啟動程序後，讓使用者輸入預算，並打印出商品列表
* 允許使用者根據商品編號來購買商品
* 使用者選擇商品後，檢查預算是否足夠？如果夠，就直接扣款，如果不夠，就提醒使用者預算不足
* 可以讓使用者隨時退出購物的程序，退出時，請打印出使用者已購買的商品及餘額


```
#!/usr/bin/env python3
# -*- coding:utf-8 -*-

product_list = [
    ('iPhone', 100),
    ('Mac Pro', 1000),
    ('iPad Mini', 150),
    ('Bike', 50)
]


budget = input("請輸入你的購買預算：")
shopping_car = []


if budget.isdigit():
    budget = int(budget)
    while True:
        for index, item in enumerate(product_list):
            #print(product_list.index(item), item)
            print(index, item)

        user_choice = input("請選擇要購買的商品:")
        if user_choice.isdigit():
            user_choice = int(user_choice)
            if user_choice < len(product_list) and user_choice >= 0:
                product_itme = product_list[user_choice]
                if product_itme[1] <= budget:
                    shopping_car.append(product_itme)
                    budget -= product_itme[1]
                    print("Added %s into shopping car, your current balance is \033[31;1m%s\033[0m" %(product_itme, budget))
                else:
                    print("\033[41;1m你的餘額[%s]...不夠了!!!\033[0m" %(budget))
                    break
            else:
                print("你輸入錯誤商品代號\033[34;1m[%s]\033[0m了，請重新輸入" %(user_choice))
        elif user_choice == 'q':
            print("---------------購買清單---------------")
            for p in shopping_car:
                print(p)
            print("\033[41;1m你的餘額[%s]...\033[0m" % (budget))
            print('Exit')
            exit()
        else:
            print("invalid option")
```