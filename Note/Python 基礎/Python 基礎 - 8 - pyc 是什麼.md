### pyc 是什麼

Python2.7 版中，只要執行 `.py` 的檔案後，即會馬上產生一個 `.pyc` 的檔案，而在 Python3 版中，執行 `.py` 的檔案後，即會產生一個叫 `__pycache__` 的目錄，裡面也會有一個 `.pyc` 的檔案，就拿剛剛之前的 `sys_login.py` 來說，當我用 Python3 執行時，就會有一個 `__pycache` 的目錄，裡面就會產生一個檔案叫 `sys_login.cpython-35.pyc`。

那這個檔案有什麼作用呢？

### Python 是一門解釋型程式語言？

有人說，Python 是一門解釋性的程式語言，直到發現有 `*.pyc` 檔案的存在後，開始覺得哪怪怪的，而那個 `c` 應該指的就是 `compiled`  的縮寫吧 !!!  為了理清這個問題到底是為什麼，下文會大約介紹一下。

### 解釋型語言和編譯型語言 
電腦是不能夠識別高級語言的，所以當我們運行一個高級語言時，就需要一個 `翻譯機`  來把高級語言轉變成電腦所能識別機器語言的過程，這個過程就分成二個類型，第一個是編譯型語言，第二種是解釋型語言。


* 編譯型語言在執行程序之前，會先通過編釋器對程序執行一個編譯的過程，把程序轉變成機器語言，運行時就不需要翻譯，直接就可以執行了，其中最典型的例子就是 `C語言`。


* 解釋型語言就是不用透過編釋的過程，而是在程序運行時，通過解釋器對程序逐行作出解釋，然後直接運行，最典型的例子就是 `Ruby` 跟 `Python`。

總結來說，因為編譯型語言在程序運行之前，就已經對程序做出了翻譯，所以運行中少掉了翻譯的這個動作，所以執行效率比較高一點，但這也不是絕對的，有一些解釋型語言 `(Java)` 也可以透過解釋器的優化在來對程序做出翻譯時，對整個程序做出優化，從而在效率上超過編譯型語言。

`從而在效率上超過編譯型語言` → 這句話不完全正確，應該是說在大多數的情況下，它的速度跟編譯型語言比較接近。此外，隨著 Java語言的興起，我們又不能把語言純粹地分成解釋型語言和編譯型語言。

用 Java 來舉例好了， Java 首先是通過編譯成 [Java字節碼文件](https://zh.wikipedia.org/wiki/Java%E5%AD%97%E8%8A%82%E7%A0%81)，然後再運行時通過解釋器給解釋成機器語言，所以我們會說 Java 是一種先編譯後解釋的語言。


### Python 到底是什麼呢？
其實 Python 和 Java、C# 是一樣，也是一門基於虛擬機的語言，我們先從表面上簡單地解釋一下 Python 程序的運行過程好了。

當我們在命令行中輸入 `python3 hello_world.py` 時，其實是觸發了 `Python的解釋器` ，告訴 `解釋器: Hello, 起床工作啦，別發懶` ， 其實 Python 執行的第一項工作跟 Java 一樣，是編釋，只是我們看不到而已。 

這是 Java 在命令行中執行一個 Java 程序:

```
$ javac hello.java

$ java hello
```

只是我們在使用 `intellij` 之類的 IDE，已經幫我們把`先編譯再解釋`結合在一起了，而 Python 其實也是一樣執行了這樣的一個過程，所以我們應該說 `Python是一門先編譯後解釋的程式語言`。


### 簡述 Python 的運行過程

在說這個問題之前，我們先來說二個概念， `PyCodeObject 和 pyc 文件`。

我們在硬碟中看到的 `pyc`不用多說，其實`PyCodeObject`就是 Python 編譯器真正編譯成的結果，我們來舉個簡單的例子。

當 Python 程序運行時，編譯的結果會暫時保存在記憶體中的 `PyCodeObject`中，當 Python 程序結束時，`Python解釋器會把PyCodeObject寫入到pyc的檔案裡`。

當 Python 程序第二次運行時，首先會在硬碟中尋找 `.pyc`檔，如果有找到，就直接載入，如果沒有找到，就重覆上面的過程。

但還有個問題，如果當 Python 的代碼，如果有做異動的話，那又是怎麼運作的呢？

**首先一樣會在硬碟中尋找 .pyc 檔，如果有找到，並且發現 Python 的原始碼有被異動時，就會再檢查時間，而這個時間是指 hello.pyc 文件的更新時間和 hello.py 文件的更新時間相比，假設 hello.py 的時間比較新，則代表有修改過檔案，則重新編譯，反之，就直接載入**，簡單地說就是去判斷二個檔案的更新時間後，再決定到底要不要編譯或是直接載入。

所以我們應該這樣定位 `PyCodeObject 和 pyc 文件`， `pyc 文件其實是 PyCodeObject 的一種持久性的保存方式`。





參考資料：

* [Java字節碼文件](https://zh.wikipedia.org/wiki/Java%E5%AD%97%E8%8A%82%E7%A0%81)
* [CodeData](http://www.codedata.com.tw/java/java-tutorial-the-1st-class-3-hello-world/)
* [昀淡風輕](http://jktt5230qq.blogspot.tw/2013/07/java-cmd.html)
* [谈谈 Python 程序的运行原理](http://www.restran.net/2015/10/22/how-python-code-run/)
* [Python 的 .py 和 .pyc 檔有什麼不同 ?](http://www.arthurtoday.com/2010/02/python-py-pyc.html)
* [Python源码剖析--Pyc文件解析](https://my.oschina.net/renwofei423/blog/17404)

