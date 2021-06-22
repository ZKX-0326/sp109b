# 期末報告: C4編譯器

## 此專案來源
* 此專案為參考 [Robert Swierczek](https://github.com/rswier/c4) 的程式碼，並在我理解後加上註解，但是因為我還沒辦法完全理解，故只有部分註解，其餘部分我會在理解後補上

* [版權說明](https://github.com/rswier/c4/blob/master/LICENSE)

## 簡介
* C4是一個小型的C語言編譯器，以一個C語言源碼文件、528行C語言代碼及4個函數製成，且具備詞法分析、語法分析、簡單的語意檢查、代碼生成以及運行時環境(虛擬機)
* C4編譯完成後，會產生字節碼，然後由虛擬機執行
* C4同時也能做到自我編譯

## Code
* [程式碼](https://github.com/ZKX-0326/sp109b/blob/main/final/C4/c4.c)

## 執行成果
* 編譯hello.c
> ![hello](https://github.com/ZKX-0326/sp109b/blob/main/final/C4/picture/hello.png)
> 『hello, world』為hello.c輸出，『exit(0) cycle = 9』則是由C4輸出，表示正常運作並結束以及hello.c共生成9條代碼
* 以C4自我編譯後的C4編譯hello.c
> ![C4hello](https://github.com/ZKX-0326/sp109b/blob/main/final/C4/picture/C4hello.png)
> 『exit(0) cycle = 9』由被C4編譯的C4輸出，表示正常運作並結束以及hello.c共生成9條代碼
> 『exit(0) cycle = 25983』則是一開始由GCC編譯的C4輸出，表示正常運作並結束以及c4.c共生成25983條代碼

## 參考資料
* [C in four function (c4) Compiler](https://hackmd.io/@srhuang/Bkk2eY5ES)
* [C4：4個函數，528行代碼實現可自舉的C語言編譯器](https://kknews.cc/zh-tw/code/zrkmqga.html)
* [有哪些关于c4 - C in four function 编译器的文章？](https://www.zhihu.com/question/28249756)
* [陳鍾誠老師在gitlab的sp/C1-c4](https://gitlab.com/ccc109/sp/-/tree/master/C1-c4)
* [rswier/c4](https://github.com/rswier/c4)