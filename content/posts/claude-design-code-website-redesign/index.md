---
title: "我用 Claude Design + Claude Code 更新了補給工具網站，流程比想像中痛苦但結果還不錯"
date: 2026-05-20T00:00:00+08:00
description: "用 Claude Design 做 mockup、Claude Code 實作，把三鐵補給工具的 UI 翻新一遍。流程不是全程順暢，但設計到開發的銜接比自己手刻好很多。"
tags: ["Claude", "AI工具", "Side Project", "Web開發", "三鐵"]
cover:
  image: "cover.jpg"
  relative: true
---

我有一個自己在用的三鐵工具網站（[bike-power-calculator.web.app](https://bike-power-calculator.web.app)），三個頁面：功率計算、補給規劃、補給品資料庫。功能做得差不多了，但 UI 一直讓我不太滿意，手機版更是幾乎不能用。

一直拖著沒處理，因為 UI 設計是我最頭痛的部分。我甚至認真考慮過要不要去學 Figma。後來看到 Claude Design，就想說試試看。

結論先說：**流程比我想像中麻煩跟費時，但成果我覺得還不錯。**

---

### 用 Claude Design 先確認方向

第一步是把網站連結丟給 Claude Design，它會主動問很多問題：主要使用情境、視覺方向、語言、痛點、希望保留什麼、想要幾個對比方向、配色偏好。不需要想太清楚，回答到哪算哪，它會幫你補齊。

它一次做出三個完整可互動的 prototype 並排比較，用的是我網站真實的欄位（FTP、體重、配速、賽事距離 51.5/113/226）。我說不喜歡哪一個，它就砍掉重做，加了 %FTP 強度徽章、T1/自行車/跑步分段卡片、手機比賽模式的大字倒數計時。

這個階段最有價值的地方是：**layout、component 結構、互動邏輯在寫程式前就定好了**，之後改需求的成本比直接改程式碼低很多。

---

### Claude Code 接手實作：先踩了一個很貴的坑

設計方向確認後，Claude Design 會自動打包一個 handoff 套件：README（版面結構、欄位 spec、互動行為、實作順序）、design tokens、React 設計原始碼、原版備份。把這包東西丟給 Claude Code，理論上它應該能照著做。

**一開始我的做法完全錯誤。** 我照claude design跟我說的，直接叫它「幫我把 index.html 改成新設計」，它就開始整檔重寫。第一次跑到一半撞到 output limit，輸出截斷、檔案壞掉。我重來，它又整檔重寫，再次撞限制。兩次就把 5 小時的 token 從 0% 用到 100%，什麼都沒完成。

後來才摸出有效的做法，核心概念是：**一頁一次、用 Edit 工具而非整檔重寫、每個區塊做完先停下來確認。**

實際的 prompt 流程大概是這樣：

**Step 1：先讓它讀懂全貌，但不要動檔案**
```
請讀 README.md 並告訴我你理解的：
1. 三頁的新版設計重點是什麼
2. 共用的 design tokens 有哪些
3. 需要保留的現有功能清單

不要開始改檔案，先確認理解一致。
```

**Step 2：從最簡單的開始（共用 tokens）**
```
請只做這件事：
把 tokens.css 的所有 CSS 變數複製到三個 HTML 檔案的 <style> 開頭。
保留原本的所有 CSS 規則，只新增 tokens。
用 Edit 工具，不要整檔重寫。
```

**Step 3：一頁一頁來**
```
請只改 index.html。
參考 page-power-calc.jsx，把 UI 改成新版設計。

規則：
1. 保留所有現有的 JavaScript 函式
2. 保留所有 input 的 id，讓計算邏輯不用改
3. 用 Edit/Replace 工具分次改，不要 Write 整檔
4. 每改完一個區塊先停下來讓我確認

做完讓我先看效果，不要急著動 nutrition.html。
```

如果它還是想整檔重寫，直接打斷：「停。請改用 Edit 工具，一次只改一個 section。先改 nav，做完讓我看。」

用這個方法，三個頁面的手機版大概 2 天完成。中間遇到一些瀏覽器的坑——iOS Safari 的 `100vh` 不等於可視高度、flex 的 `min-height: auto` 陷阱、`env(safe-area-inset-bottom)` 在新款 iPhone 的處理——這些以前要 Google 很久，現在跟 Code 說「按鈕被蓋住」，它會找出根本原因並解釋清楚再修掉。

---

### 幾個老實說

**Claude Design 一開始一直跑不動。** 不知道是伺服器問題還是我的 prompt 太複雜，反覆失敗了好幾次，直接把我一週的 token 用完。所幸 Claude Design 的 token 配額和 Pro Plan 是獨立的，不然我可能連 Claude Code 都沒得用。

**這個流程確實讓人想升 Max Plan。** Design 把 token 燒完，Code 又要跑一堆 Edit。如果是比較密集的開發期，Max Plan 應該會順很多，我自記就是一直在等用量更新。

**成果還行。** 自己做的真的學習成本太高，而且UI 一致性很難維持，這次因為有設計系統打底，三個頁面的視覺語言是統一的。

---

對於沒有設計師、又要顧多個頁面一致性的 side project，Design + Code 的分工是有效的。只是要有心理準備，中間有些地方不會那麼順，token 也要抓寬一點。
