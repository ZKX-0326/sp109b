# 系統程式第一週筆記

## 編譯
```
編譯的四個步驟:
前置處理→編譯→組譯→連結
```

* basic
```
用gcc將程式碼編譯成執行檔
gcc sum.c
預設輸出檔案為 a.exe 或 a.out

也可用 -o 參數來指定輸出檔名
gcc sum.c -o sum
此時輸出檔案則是 sum.exe 或 sum.out
```

* link
```
直接將多個檔案編譯並連結產生執行檔
gcc main.c sum.c -o run

將檔案編譯產生組合語言，再進行連結產生執行檔
gcc -S main.c -o main.s
gcc -S sum.c -o sum.s
gcc main.s sum.s -o run

將檔案組譯產生目的檔，再進行連結產生執行檔
gcc -c sum.c -o sum.o
gcc -c main.c -o main.o
gcc main.o sum.o -o run
```

* toolchain
```
將檔案編譯產生組合語言
gcc -S sum.c -o sum.s

將組合語言組譯產生目的檔
gcc -c sum.s -o sum.o

將目的檔與函式庫連結後產生執行檔
gcc sum.o -o sum
```

* cpp
```
將 gcc 改成 g++
```

* make
```
1. makeExe
    用 makefile 將檔案進行編譯
    CC := gcc
    CFLAGS = -std=c99 -O0
    TARGET = run

    all: $(TARGET)

    $(TARGET): sum.c main.c
	    $(CC) $(CFLAGS) $^ -o $@

    clean:
	    rm -f *.o *.exe $(TARGET)

2. makeLib
    用 makefile 先將檔案組譯，並創建資料庫後，再進行連結
    CC := gcc
    AR := ar
    CFLAGS = -std=c99 -O0
    TARGET = run
    LIB = libstat.a

    all: $(TARGET)

    $(TARGET): $(LIB) main.o
	    $(CC) $(CFLAGS) $^ -L ./ -lstat -o $@

    $(LIB): sum.o
	    $(AR) -r $@ $^

    %.o: %.c
	    $(CC) $(CFLAGS) -c $< -o $@

    clean:
	    rm -f *.o *.a *.exe $(TARGET)
```