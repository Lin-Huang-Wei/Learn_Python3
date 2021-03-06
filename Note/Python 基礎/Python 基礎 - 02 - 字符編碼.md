### 字符編碼 

Python 解釋器在加載 `.py 文件`中的代碼時，會對內容進行編碼 **`(默認 ascill)`**

* **ASCII**

(American Standard Code for Information Interchange, 美國信息交换標準代碼）是基於拉丁字母的一套電腦编碼系统，

主要用於顯示現代英語和部分支援其他西歐語言，其最多只能用 **8 bit來表示(一個字節)**，即： **`2**8 = 256-1，所以ASCII碼最多只能表示255個符號`**

 

* **關於中文編碼**

為了處理漢字，程序員設計了用於簡體中文的GB2312和用於繁體中文的big5。

* **GB2312**

《信息交換用漢字編碼字符集》是由中國國家標準總局**1980**年發布，1981年5月1日開始實施的一套國家標準，標準號是GB 2312—1980，又稱為GB 2312–80、GB0 。

GB2312編碼用**兩個字節(8位2進制)表示一個漢字**，所以理論上最多可以表示256×256=65536個漢字。

整個字符集分成94個區，每區有94個位。每個區位上只有一個字符，因此可用所在的區和位來對漢字進行編碼，稱為區位碼。
基本集共收入漢字6763個和非漢字圖形字符682個。它所收錄的漢字已經覆蓋中國大陸99.75%的使用頻率。但對於人名、古漢語等方面出現的罕用字和繁體字，GB 2312不能處理，因此後來GBK及GB 18030漢字字符集相繼出現以解決這些問題。
GB2312編碼適用於漢字處理、漢字通信等系統之間的信息交換，通行於中國大陸；新加坡等地也採用此編碼。

 

* **GBK**

漢字內碼擴展規範，全名為`《漢字內碼擴展規範(GBK)》1.0版`，由中華人民共和國全國信息技術標準化技術委員會**1995年12月1日制訂**。
GBK的K為漢語拼音Kuo Zhan（擴展）中“擴”字的聲母。英文全稱Chinese Internal Code Extension Specification。
GBK 只為**"技術規範指導性文件"**，不屬於國家標準。國家質量技術監督局於2000年3月17日推出了GB 18030-2000標準，以取代GBK

 

* **GB 18030**

全稱：「國家標準GB 18030-2005《資訊科技 中文編碼字元集》」，是中華人民共和國現時最新的變長度多位元組字元集。

對GB 2312-1980完全回溯相容，與GBK基本回溯相容；支援GB 13000（93版等同於Unicode 1.1；共收錄漢字70,244個。

現在的PC平台必須支持GB18030，對嵌入式產品暫時沒有要求，所以手機、MP3都只有支持GB2312。

GB 18030版本如下： 

GB 18030-2000，相容 Unicode 3.0 中日韓統一表意文字（即擴充功能A區），共收27,533個漢字；2000年3月17日發布、2000年7月1日實施。


GB 18030-2005，更新至 Unicode 3.1 中日韓統一表意文字（即擴充功能B區），並刊載少數民族包括朝鮮文、蒙古文（包括滿文、托忒文、錫伯文、阿禮嘎禮文）、德宏傣文、藏文、維吾爾文／哈薩克文／柯爾克茲文和彝文的文字。共有70,244個漢字；2005年11月8日發布、2006年5月1日實施

**從ASCII、GB2312、GBK、GB18030，這些編碼方法是向下兼容的。**

* **Unicode（中文：萬國碼、國際碼、統一碼、單一碼）**

是電腦科學領域裡的一項業界標準。它對世界上大部分的文字系統進行了整理、編碼，使得電腦可以用更為簡單的方式來呈現和處理文字。 

 

* **UTF-8（8-bit Unicode Transformation Format）**

是一種針對Unicode的可變長度字元編碼，也是一種字首碼。它可以用來表示Unicode標準中的任何字元，且其編碼中的第一個位元組仍與ASCII相容，這使得原來處理ASCII字元的軟體無須或只須做少部份修改，即可繼續使用。因此，它逐漸成為電子郵件、網頁及其他儲存或傳送文字的應用中，優先採用的編碼。

 

總結：

```
ASCII 255個字符 1bytes

        --->  1980年 GB2312  6763個漢字

        --->  1995年 GBK1.0   2萬漢字以上

        --->  2000年 GB18030 27,533個漢字

        --->  unicode 2bytes

        --->  utf-8  en: 1byte, zh: 3 bytes
```

現在來個小實驗，用 Python2.7 版的來執行，看不加入字符編碼會有什麼結果？

```
#!/usr/bin/env python
name = "你好，世界"
print(name)

----------------------輸出結果---------------------- 

SyntaxError: Non-ASCII character '\xe4' in file coding.py on line 4, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
``` 

上面錯誤訊息是說因為沒有加入字符編碼，無法解譯出中文，所以噴Error，解法其實很簡單，可以參考上面錯誤訊息裡的網址，會告訴你要加入字符編碼，寫法如下：

```
#!/usr/bin/python
# -*- coding: utf-8 -*-
``` 

若是 Python3的話，會有什麼樣的結果？

```
#!/usr/bin/env python3

name = "你好，世界"
print(name)

----------------------輸出結果---------------------- 

你好，世界
```

為什麼 Python3 不用加入字符編碼？是因為預設 Python3 默認就是用 utf-8，所以執行時就會直接打印出來。

 