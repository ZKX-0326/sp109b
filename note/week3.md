# 系統程式第三週筆記

* lexer.c
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define TMAX 10000000
#define SMAX 100000

enum { Id, Int, Keyword, Literal, Char };

char *typeName[5] = {"Id", "Int", "Keyword", "Literal", "Char"};

char code[TMAX];
char strTable[TMAX], *strTableEnd=strTable;
char *tokens[TMAX];
int tokenTop=0;
int types[TMAX];

#define isDigit(ch) ((ch) >= '0' && (ch) <='9')

#define isAlpha(ch) (((ch) >= 'a' && (ch) <='z') || ((ch) >= 'A' && (ch) <= 'Z'))

int readText(char *fileName, char *text, int size) {
  FILE *file = fopen(fileName, "r");
  int len = fread(text, 1, size, file);
  text[len] = '\0';
  fclose(file);
  return len;
}

/* strTable =
#\0include\0"sum.h"\0int\0main\0.....
*/
char *next(char *p) {
  while (isspace(*p)) p++;

  char *start = p; //         include "sum.h"
                   //         ^      ^
                   //  start= p      p
  int type;
  if (*p == '\0') return NULL;
  if (*p == '"') {
    p++;
    while (*p != '"') p++;
    p++;
    type = Literal;
  } else if (*p >='0' && *p <='9') { // 數字
    while (*p >='0' && *p <='9') p++;
    type = Int;
  } else if (isAlpha(*p) || *p == '_') { // 變數名稱或關鍵字
    while (isAlpha(*p) || isDigit(*p) || *p == '_') p++;
    type = Id;
  } else { // 單一字元
    p++;
    type = Char;
  }
  int len = p-start;
  char *token = strTableEnd;
  strncpy(strTableEnd, start, len);
  strTableEnd[len] = '\0';
  strTableEnd += (len+1);
  types[tokenTop] = type;
  tokens[tokenTop++] = token;
  printf("token=%s\n", token);
  return p;
}

void lex(char *fileName) {
  char *p = code;
  while (1) {
    p = next(p);
    if (p == NULL) break;
  }
}

void dump(char *strTable[], int top) {
  for (int i=0; i<top; i++) {
    printf("%d:%s\n", i, strTable[i]);
  }
}

int main(int argc, char * argv[]) {
  readText(argv[1], code, sizeof(code));
  puts(code);
  lex(code);
  dump(tokens, tokenTop);
}
```
> 將程式碼進行分解成一個個字串
```
relax@DESKTOP-MP5A0EO MINGW64 ~/OneDrive/文件/code/sp/03-compiler/02-lexer (master)
$ gcc lexer.c -o lexer

relax@DESKTOP-MP5A0EO MINGW64 ~/OneDrive/文件/code/sp/03-compiler/02-lexer (master)
$ ./lexer sum.c
#include "sum.h"

int main() {
  int t = sum(10);
  printf("sum(10)=%d\n", t);
}
token=#
token=include
token="sum.h"
token=int
token=main
token=(
token=)
token={
token=int
token=t
token==
token=sum
token=(
token=10
token=)
token=;
token=printf
token=(
token="sum(10)=%d\n"
token=,
token=t
token=)
token=;
token=}
0:#
1:include
2:"sum.h"
3:int
4:main
5:(
6:)
7:{
8:int
9:t
10:=
11:sum
12:(
13:10
14:)
15:;
16:printf
17:(
18:"sum(10)=%d\n"
19:,
20:t
21:)
22:;
23:}
```

* 習題IF
```
void IF(){
  int ifBegin = nextLabel();
  int ifEnd = nextLabel();
  emit("(L%d)\n", ifBegin);
  skip("if");
  skip("(");
  int e = E();
  emit("if not T%d goto L%d\n", e, ifEnd);
  skip(")");
  STMT();
  emit("goto L%d\n", ifBegin);
  emit("(L%d)\n", ifEnd);
}
```
* 執行結果
```
relax@DESKTOP-MP5A0EO MINGW64 ~/OneDrive/文件/code/sp1/03-compiler/03b-compiler2 (master)
$ ./compiler test/if.c
i = 1;
if (i == 1) i = i + 1;
========== lex ==============
token=i
token==
token=1
token=;
token=if
token=(
token=i
token===
token=1
token=)
token=i
token==
token=i
token=+
token=1
token=;
========== dump ==============
0:i
1:=
2:1
3:;
4:if
5:(
6:i
7:==
8:1
9:)
10:i
11:=
12:i
13:+
14:1
15:;
============ parse =============
t0 = 1
i = t0
(L0)
t1 = i
t2 = 1
t3 = t1 == t2
if not T3 goto L1
t4 = i
t5 = 1
t6 = t4 + t5
i = t6
goto L0
(L1)
```