# 系統程式第五週筆記

## 虛擬機
```
狹義的虛擬機:
    虛擬機:模擬處理器指令集的軟體
    模擬器:模擬電腦行為的軟體
廣義的虛擬機:
    有些軟體能模擬指令集和電腦行為，像是VMware、VirtualPC、Virtual box
    在多數情況下會以虛擬機來統稱狹義的虛擬機和模擬器
```

## 三種虛擬機架構
```
記憶體機(Memory Machine): 可直接對記憶體變數進行運算
暫存器機(Register Machine): 必須將變數載入暫存器中，才能進行運算
堆疊機(Stack Machine): 會取出堆疊上層元素進行運算，並將結果存回堆疊中
```

## HelloWorld Java
![picture](https://github.com/ZKX-0326/sp109b/blob/main/note/picture/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202021-05-04%20202402.png)

## For的改良
* 修改及新增的程式碼
```
// #define emit printf
int isTempIr = 0;
char tempIr[100000], *tempIrp = tempIr;
#define emit(...) ({ \
  if (isTempIr){ \
    sprintf(tempIrp, __VA_ARGS__); \
    tempIrp += strlen(tempIrp);\
  }\
  else { \
    printf(__VA_ARGS__);\
  }\
})

void FOR(){
  int forBegin = nextLabel();
  int forEnd = nextLabel();
  emit("(L%d)\n", forBegin);
  skip("for");
  skip("(");
  ASSIGN();
  int e = E();
  emit("if not T%d goto L%d\n", e, forEnd);
  skip(";");
  isTempIr = 1;
  int e1 = E();
  isTempIr = 0;
  char e1str[10000];
  strcpy(e1str, tempIr);
  skip(")");
  STMT();
  emit("%s\n", e1str);
  emit("goto L%d\n", forBegin);
  emit("(L%d)", forEnd);
}

```