---
title: "跟我一起玩樹莓派 #1：從零開始學 Linux 基本指令"
date: 2026-03-13T09:11:51Z
description: "這是「跟我一起玩樹莓派」系列的第一篇。這個系列會帶你從完全不懂 Linux，一步一步打造自己的家用伺服器。"
canonicalURL: "https://medium.com/@chrislee8613/%E8%B7%9F%E6%88%91%E4%B8%80%E8%B5%B7%E7%8E%A9%E6%A8%B9%E8%8E%93%E6%B4%BE-1-%E5%BE%9E%E9%9B%B6%E9%96%8B%E5%A7%8B%E5%AD%B8-linux-%E5%9F%BA%E6%9C%AC%E6%8C%87%E4%BB%A4-e48bb5390ccc"
cover:
  image: "https://cdn-images-1.medium.com/max/800/0*wcaW8a5WYiSBKVaD.jpg"
  relative: false
tags: ["Linux", "樹莓派", "VS Code"]

---
![https://micro.rohm.com/tw/deviceplus/wp-content/uploads/2023/11/what-is-raspberrypi_01_2.jpg](https://cdn-images-1.medium.com/max/800/0*wcaW8a5WYiSBKVaD.jpg)
*https://micro.rohm.com/tw/deviceplus/wp-content/uploads/2023/11/what-is-raspberrypi_01_2.jpg*

> *這是「跟我一起玩樹莓派」系列的第一篇。這個系列會帶你從完全不懂 Linux，一步一步打造自己的家用伺服器。*

## 為什麼要學 Linux？

買了樹莓派，開機之後面對黑黑的終端機畫面，不知道要打什麼 — — 這是很多人的第一個障礙。

但其實 Linux 的基本指令不多，學會二三十個就能應付日常操作。這篇文章會帶你從頭學起，每個指令都搭配實際的例子，讓你在樹莓派上邊做邊學。

## 你需要準備什麼

- Raspberry Pi（任何型號都可以）
- MicroSD 卡（建議 32GB 以上）
- 電源線
- 一台電腦

樹梅派我是從大陸的[微雪電子](https://www.waveshare.net/)購入，價格很優惠而且有很多相關套件可以買。

先去官網下載 **Raspberry Pi Imager**，把 Raspberry Pi OS 燒錄到 SD 卡。燒錄時記得在設定裡：

- 開啟 **SSH**
- 設定 **Wi-Fi** 帳號密碼
- 設定使用者名稱和密碼

這樣開機後就可以直接從電腦連線，不需要接螢幕。

## 第一次開機

把 SD 卡插進樹莓派，接上電源，等待約 1 分鐘讓系統啟動。

到路由器管理頁面，或用以下指令找到樹莓派的 IP：

```bash
# Windows 在命令提示字元輸入
ping raspberrypi.local
# 或是去路由器查看連線設備清單
```

找到 IP 之後，接下來就可以 SSH 連線了（下一篇會詳細介紹），但這篇我們先專注在 Linux 指令本身。

## 檔案與目錄操作

Linux 的一切都是檔案，學會操作檔案就學會了一半的 Linux。

## 看你在哪裡

```bash
pwd
```

`pwd` 是 "Print Working Directory" 的縮寫，告訴你現在在哪個資料夾。

```bash
/home/pi
```

## 看這裡有什麼

```bash
ls
```

列出當前目錄的所有檔案和資料夾。

```bash
ls -la
```

加上 `-la` 可以看到隱藏檔案（以 `.` 開頭的檔案）和詳細資訊，包括權限、大小、修改時間。

## 移動位置

```bash
cd Documents        # 進入 Documents 資料夾
cd ..               # 回到上一層
cd ~                # 回到家目錄（/home/pi）
cd /                # 去到根目錄
```

## 建立資料夾

```bash
mkdir 我的資料夾
mkdir -p a/b/c      # 一次建立多層資料夾
```

## 建立檔案

```bash
touch hello.txt     # 建立一個空白檔案
```

## 複製、移動、刪除

```bash
cp 原檔案 目標位置       # 複製檔案
cp -r 原資料夾 目標位置  # 複製整個資料夾
```

```
mv 原檔案 新名稱         # 移動或重新命名
```

```
rm 檔案名稱              # 刪除檔案
rm -r 資料夾名稱         # 刪除整個資料夾（小心使用！）
```

> *⚠️ Linux 沒有資源回收桶，刪掉就是真的刪掉了。*

## 查看與編輯檔案

## 看檔案內容

```bash
cat 檔案名稱            # 顯示整個檔案內容
less 檔案名稱           # 可以上下捲動（按 q 離開）
head -n 10 檔案名稱     # 看前 10 行
tail -n 10 檔案名稱     # 看後 10 行
tail -f 記錄檔          # 即時追蹤檔案更新（看 log 很好用）
```

## 編輯檔案

```typescript
nano 檔案名稱
```

`nano` 是最簡單的文字編輯器：

- `Ctrl + O` 儲存
- `Ctrl + X` 離開
- `Ctrl + W` 搜尋

## 系統資訊

## 硬碟空間

```bash
df -h
```

看每個磁碟的使用狀況，`-h` 讓數字以人類易讀的格式顯示（GB、MB）。

## 記憶體使用

```sql
free -h
```

## 目前跑了什麼程序

```css
top
```

即時顯示系統資源使用狀況，按 `q` 離開。

```typescript
ps aux
```

列出所有正在執行的程序。

## 停掉某個程序

```bash
kill 程序ID
```

先用 `ps aux | grep 程式名稱` 找到程序 ID，再用 `kill` 停掉它。

## 套件管理

樹莓派用 `apt` 來安裝、更新、移除軟體。

```csharp
sudo apt update              # 更新套件清單
sudo apt upgrade -y          # 更新所有已安裝的套件
sudo apt install 套件名稱    # 安裝套件
sudo apt remove 套件名稱     # 移除套件
```

## 權限與 sudo

Linux 有嚴格的權限控制，有些指令需要管理員權限才能執行。

```typescript
sudo 指令
```

`sudo` 讓你以管理員（root）身份執行指令，系統會要求你輸入密碼確認。

## 了解檔案權限

```bash
ls -la
```

看到類似這樣的輸出：

```r
-rw-r--r-- 1 pi pi 1234 Jan 1 12:00 hello.txt
drwxr-xr-x 2 pi pi 4096 Jan 1 12:00 Documents
```

前面的 `rwxr-xr-x` 是權限：

- `r` = 讀取
- `w` = 寫入
- `x` = 執行
- 分成三組：**擁有者**、**群組**、**其他人**

```bash
chmod 755 檔案名稱    # 改變權限
chown pi:pi 檔案名稱  # 改變擁有者
```

## 網路相關

```bash
ping google.com          # 測試網路連線
curl https://example.com # 發送 HTTP 請求
hostname -I              # 查看自己的 IP 位址
```

## 實用小技巧

## Tab 自動補全

打指令或路徑時按 `Tab`，系統會自動補全，是省時間的好習慣。

## 上下鍵查看歷史指令

按 `↑` 可以叫出上一個指令，不用重複輸入。

## Ctrl + C 緊急停止

程式跑到一半想停掉，按 `Ctrl + C` 強制中斷。

## 管道（Pipe）

```typescript
指令1 | 指令2
```

把第一個指令的輸出，當作第二個指令的輸入。

```perl
# 例如：列出所有程序，只顯示包含 docker 的行
ps aux | grep docker
```

## 重新導向

```bash
指令 > 檔案名稱    # 把輸出存到檔案
指令 >> 檔案名稱   # 把輸出附加到檔案末尾
```

## 練習題

學指令最好的方式就是動手做，試試看在樹莓派上完成這些任務：

1. 在家目錄建立一個叫 `practice` 的資料夾
2. 進入這個資料夾，建立三個檔案：`a.txt`、`b.txt`、`c.txt`
3. 用 `nano` 在 `a.txt` 裡寫一段文字
4. 把 `a.txt` 複製成 `a_backup.txt`
5. 刪除 `b.txt`
6. 用 `ls -la` 確認資料夾裡的狀況
7. 查看目前硬碟空間

## 小結

這篇學到的指令：

![](https://cdn-images-1.medium.com/max/800/1*3aRO005SpTpYPpAPEA0Mjw.png)
