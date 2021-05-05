# IF
> 參考03b-compiler2.c中的while的寫法
<pre><code>
void IF(){
  int ifBegin = nextLabel();
  int ifMid = nextLabel();
  int ifEnd = nextLabel();
  emit("(L%d)\n", ifBegin);
  skip("if");
  skip("(");
  int e = E();
  emit("if not T%d goto L%d\n", e, ifMid); //若判斷式不成立則進入下一個條件式
  skip(")");
  STMT();
  emit("goto L%d\n", ifEnd); //表示完成條件式中的code，並結束整個條件式
  emit("(L%d)\n", ifMid);
  if(isNext("else")){ //else if和else的條件式
    skip("else");
    STMT();
    emit("(L%d)\n",ifEnd); //表示完成條件式中的code，並結束整個條件式
  }
}
</code></pre>

* if.c
<pre><code>
if (a>3) {
  t=1;
} else if(a<=0){
  t=2;
} else{
  t=3;
}
</code></pre>

* 執行結果
```
relax@DESKTOP-MP5A0EO MINGW64 ~/OneDrive/文件/code/sp/03-compiler/03b-compiler2 (master)
$ ./compiler test/if.c
if (a>3) {      
  t=1;
} else if(a<=0){
  t=2;
} else{
  t=3;
}
========== lex ==============
token=if
token=(
token=a
token=>
token=3
token=)
token={
token=t
token==
token=1
token=;
token=}
token=else
token=if
token=(
token=a
token=<=
token=0
token=)
token={
token=t
token==
token=2
token=;
token=}
token=else
token={
token=t
token==
token=3
token=;
token=}
========== dump ==============
0:if
1:(
2:a
3:>
4:3
5:)
6:{
7:t
8:=
9:1
10:;
11:}
12:else
13:if
14:(
15:a
16:<=
17:0
18:)
19:{
20:t
21:=
22:2
23:;
24:}
25:else
26:{
27:t
28:=
29:3
30:;
31:}
============ parse =============
(L0)
t0 = a
t1 = 3
t2 = t0 > t1
if not T2 goto L1
t3 = 1
t = t3
goto L2
(L1)
(L3)
t4 = a
t5 = 0
t6 = t4 <= t5
if not T6 goto L4
t7 = 2
t = t7
goto L5
(L4)
t8 = 3
t = t8
(L5)
(L2)
```