---
title: "C 語言標準函式庫完整指南"
date: 2025-02-13T03:00:44Z
description: "一開始在學C 語言的時候，常遇到有些功能可以使用，有些不行，後來才知道，標準函式庫的存在。標準函式庫是每個 C 程式設計師必須掌握的基礎工具。本文將詳細介紹各個重要的標準函式庫、引入方式及其常用函數。"
canonicalURL: "https://medium.com/@chrislee8613/c-%E8%AA%9E%E8%A8%80%E6%A8%99%E6%BA%96%E5%87%BD%E5%BC%8F%E5%BA%AB%E5%AE%8C%E6%95%B4%E6%8C%87%E5%8D%97-6c911ff7d72c"
tags: ["C語言", "程式設計", "嵌入式"]
---
一開始在學C 語言的時候，常遇到有些功能可以使用，有些不行，後來才知道，標準函式庫的存在。標準函式庫是每個 C 程式設計師必須掌握的基礎工具。本文將詳細介紹各個重要的標準函式庫、引入方式及其常用函數。

## Include 指令說明

在 C 語言中，我們使用 `#include` 預處理指令來引入標準函式庫。有兩種主要的引入方式：

### 1. 使用尖括號 < >

```cpp
#include <stdio.h>
#include <stdlib.h>
```

- 這種方式用於引入標準函式庫
- 編譯器會在系統的標準函式庫目錄中查找頭文件
- 建議用於引入所有標準函式庫

### 2. 使用雙引號 “ “

```cpp
#include "myheader.h"
```

- 這種方式主要用於引入自定義的頭文件
- 編譯器會先在當前目錄查找，如果找不到才會到系統目錄查找
- 適用於引入專案中自己創建的頭文件

### 常見引入示例

```cpp
// 基本程序常用引入
#include <stdio.h> // 輸入輸出相關
#include <stdlib.h> // 工具函數相關
// 字符串處理相關
#include <string.h> // 字符串函數
#include <ctype.h> // 字符類型判斷
// 數學計算相關
#include <math.h> // 數學函數
#include <float.h> // 浮點數限制
#include <limits.h> // 整數限制
// 時間相關
#include <time.h> // 時間函數
// 錯誤處理相關
#include <errno.h> // 錯誤碼
#include <assert.h> // 斷言
// 國際化支持
#include <locale.h> // 本地化設置
#include <wchar.h> // 寬字符支持

```

## 最佳實踐

### 1. 避免重複引入

```cpp
#ifndef MYHEADER_H
 #define MYHEADER_H
 // 頭文件內容
 #endif
```

### 2. 引入順序建議

— 標準 C 函式庫
 — 系統相關標準庫
 — 第三方函式庫
 — 專案自定義頭文件

### 3. 只引入需要的

— 避免引入不必要的標準庫
 — 可以減少編譯時間和可能的命名衝突

## 1. stdio.h — 標準輸入輸出函式庫

### 基本輸入輸出函數

- `printf(format,...)` — 格式化輸出
- `scanf(format,..)` — 格式化輸入
- `printf(format,...)` — 格式化輸出
- `scanf(format,...)` — 格式化輸入
- `putchar(char)` — 輸出單個字符
- `getchar()` — 讀取單個字符
- `puts(str)` — 輸出字符串並換行
- `gets(str)` — 讀取一行（不建議使用，因為不安全）

### 檔案操作函數

- `fopen(filename,mode)` — 開啟檔案
 — 模式：`”r”`（讀取），`”w”`（寫入），`”a”`（追加）
- `fclose(file)` — 關閉檔案
- `fprintf(file,format,...)` — 格式化寫入檔案
- `fscanf(file,format,...)` — 從檔案格式化讀取
- `fgets(str,n,file)` — 從檔案讀取一行
- `fputs(str,file)` — 寫入字符串到檔案
- `fseek(file,offset,whence)` — 移動檔案指針
- `ftell(file)` — 獲取當前檔案位置
- `rewind(file)` — 將檔案指針重置到開頭

## 2. stdlib.h — 標準工具函式庫

### 記憶體管理

- `malloc(size)` — 分配指定大小的記憶體
- `calloc(num,size)` — 分配並清零記憶體
- `realloc(ptr,size)` — 重新分配記憶體
- `free(ptr)` — 釋放記憶體

### 字符串轉換

- `atoi(str)` — 字符串轉整數
- `atol(str)` — 字符串轉長整數
- `atof(str)` — 字符串轉浮點數
- `strtol(str,endptr,base)` — 字符串轉長整數，可指定進制
- `strtod(str,endptr)` — 字符串轉雙精度浮點數

### 亂數生成

- `rand()` — 生成偽隨機數
- `srand(seed)` — 設置隨機數種子
- `RAND_MAX` — 隨機數的最大值

### 其他工具函數

- `abs(n) `— 整數絕對值
- `labs(n)` — 長整數絕對值
- `div(numer,denom)` — 整數除法，返回商和餘數
- `qsort(base,num,size,compar)` — 快速排序
- `bsearch(key,base,num,size,compar)` — 二分查找
- `exit(status)` — 終止程序

## 3. string.h — 字符串處理函式庫

### 字符串操作

- `strlen(str)` — 計算字符串長度
- `strcpy(dest,src)` — 複製字符串
- `strncpy(dest,src,n)` — 複製指定長度的字符串
- `stract(dest,src)` — 連接字符串
- `strncat(dest,src,n)` — 連接指定長度的字符串
- `strcmp(str1,str2)` — 比較字符串
- `strncmp(str1,str2,n)` — 比較指定長度的字符串

### 字符串搜索

- `strchr(str,char)` — 查找字符第一次出現的位置
- `strrchr(str,char)` — 查找字符最後一次出現的位置
- `strstr(haystack,needle)` — 查找子字符串
- `strtok(str,delim)` — 分割字符串

### 記憶體操作

- `memcpy(dest,src,n)` — 複製記憶體區域
- `memmove(dest,src,n)` — 移動記憶體區域（處理重疊）
- `memset(ptr,value,n)` — 填充記憶體區域
- `memecmp(ptr1,ptr2,n)` — 比較記憶體區域

## 4. math.h — 數學函式庫

### 基本數學運算

- `pow(x,y)` — x 的 y 次方
- `sqrt(x)` — 平方根
- `cbrt(x)` — 立方根
- `ceil(x)` — 向上取整
- `floor(x)` — 向下取整
- `round(x)` — 四捨五入
- `fabs(x)` — 浮點數絕對值

### 三角函數

- `sin(x)` , `cos(x)` , `tan(x)` — 基本三角函數
- `asin(x)`, `acos(x)`, `atan(x)` — 反三角函數
- `sinh(x)`, `cosh(x)`, `tanh(x)` — 雙曲三角函數

### 對數與指數

- `exp(x)` — e 的 x 次方
- `log(x)` — 自然對數
- `log10(x)` — 以 10 為底的對數
- `log2(x)` — 以 2 為底的對數

## 5. time.h — 時間處理函式庫

### 時間獲取

- `time(time_t*)` — 獲取當前時間戳
- `clock()` — 獲取程序執行時間
- `difftime(time1,time2)` — 計算時間差

### 時間轉換

- `localtime(const time_t*)` — 轉換為本地時間
- `gmtime(const time_t*)` — 轉換為 GMT 時間
- `mktime(struct tm*)` — 轉換時間結構為時間戳
- `strftime()` — 格式化時間字符串

## 6. ctype.h — 字符類型函式庫

### 字符檢測

- `isalpha(c)` — 檢查是否為字母
- `isdigit(c)` — 檢查是否為數字
- `isalnum(c)` — 檢查是否為字母或數字
- `isspace(c)` — 檢查是否為空白字符
- `ispunct(c)` — 檢查是否為標點符號
- `isupper(c)` — 檢查是否為大寫字母
- `islower(c)` — 檢查是否為小寫字母

### 字符轉換

- `toupper(c)` — 轉換為大寫字母
- `tolower(c)` — 轉換為小寫字母

## 最佳實踐

### 1. 記憶體管理

— 使用 malloc() 時必須檢查返回值是否為 NULL
 — 釋放記憶體後將指針設為 NULL
 — 使用 free() 釋放記憶體時要確保指針有效

### 2. 檔案操作

— 開啟檔案後要檢查是否成功
 — 操作完成後要及時關閉檔案
 — 使用 fgets() 代替不安全的 gets()

### 3. 字符串處理

— 使用 strncpy() 代替 strcpy() 防止緩衝區溢出
 — 使用 strncat() 代替 strcat() 控制連接長度
 — 字符串操作前檢查目標緩衝區大小

### 4. 錯誤處理

— 合理使用 errno 進行錯誤處理
 — 函數返回值要進行適當的檢查
 — 使用 perror() 輸出錯誤信息

## 結語

C 語言標準函式庫提供了豐富的功能，掌握這些函式庫可以大大提高編程效率。在實際使用中，要注意：

1. 安全性：避免緩衝區溢出，注意內存洩漏
2. 效率：選擇合適的函數，避免不必要的計算
3. 可靠性：做好錯誤處理，確保程序穩定運行
