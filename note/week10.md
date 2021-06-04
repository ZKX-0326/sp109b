# 系統程式第十週筆記\
## 單行程系統&多工系統
![picture](https://github.com/ZKX-0326/sp109b/blob/main/note/picture/10-14-638.jpg)

## 競爭情況(race condition)
* race
> ![picture](https://github.com/ZKX-0326/sp109b/blob/main/note/picture/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202021-06-03%20215608.png)
在多thread或多CPU的情況下，兩邊能共用某些變數，但可能造成修改某變數時，會讓該變數的值出現錯誤
* norace
> ![picture](https://github.com/ZKX-0326/sp109b/blob/main/note/picture/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202021-06-03%20215753.png)
> 在修改共用變數前上鎖，修改後解鎖
* deadlock
> ![picture](https://github.com/ZKX-0326/sp109b/blob/main/note/picture/%E8%9E%A2%E5%B9%95%E6%93%B7%E5%8F%96%E7%95%AB%E9%9D%A2%202021-06-03%20220407.png)
> 當兩個程式在互相等待對方的資源，但都把該資源鎖上時，會發生死結(當機)

# 排程方法
![picture](https://github.com/ZKX-0326/sp109b/blob/main/note/picture/10-23-638.jpg)

# 記憶體管理
* C語言中的記憶體分配與回收
分配:malloc()
回收:free()
* 記憶體分配策略
> ![picture](https://github.com/ZKX-0326/sp109b/blob/main/note/picture/10-35-638.jpg)

# 常見的MMU硬體
* 重定位暫存器
> ![picture](https://github.com/ZKX-0326/sp109b/blob/main/note/picture/10-40-638.jpg)
* 基底界限暫存器
> ![picture](https://github.com/ZKX-0326/sp109b/blob/main/note/picture/10-41-638.jpg)
* 分段表
> ![picture](https://github.com/ZKX-0326/sp109b/blob/main/note/picture/10-42-638.jpg)
* 分頁表
> ![picture](https://github.com/ZKX-0326/sp109b/blob/main/note/picture/10-43-638.jpg)