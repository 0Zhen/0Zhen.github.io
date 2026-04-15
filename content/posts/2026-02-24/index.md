---
title: "網路不是魔法
一次搞懂 15 個核心概念"
date: 2026-02-24T11:25:20Z
description: "從你按下 Enter 的那一刻起，資料包經歷了什麼？HTTP、DNS、IP、NAT、Port……這些詞彙每天出現在工程師的世界，卻對大多數人像咒語一樣陌生。"
canonicalURL: "https://medium.com/@chrislee8613/%E7%B6%B2%E8%B7%AF%E4%B8%8D%E6%98%AF%E9%AD%94%E6%B3%95-%E4%B8%80%E6%AC%A1%E6%90%9E%E6%87%82-15-%E5%80%8B%E6%A0%B8%E5%BF%83%E6%A6%82%E5%BF%B5-8bb306dc4a98"
cover:
  image: "https://cdn-images-1.medium.com/max/800/1*oE8jTY4Q73u-0CQEqCrhfw.png"
  relative: false
tags: ["網路"]
---
從你按下 Enter 的那一刻起，資料包經歷了什麼？HTTP、DNS、IP、NAT、Port……這些詞彙每天出現在工程師的世界，卻對大多數人像咒語一樣陌生。

![https://www.freepik.com/free-vector/flat-internet-day-illustration_13184285.htm#fromView=search&page=1&position=7&uuid=a74753ec-8930-4529-bcc0-0d9ce684ef17&query=internet](https://cdn-images-1.medium.com/max/800/1*oE8jTY4Q73u-0CQEqCrhfw.png)
*https://www.freepik.com/free-vector/flat-internet-day-illustration_13184285.htm#fromView=search&page=1&position=7&uuid=a74753ec-8930-4529-bcc0-0d9ce684ef17&query=internet*

你每天都在用網路，但你知道當你在瀏覽器輸入 `google.com` 按下 Enter，接下來發生了什麼事嗎？這篇文章用你能理解的語言，把網路底層的運作方式講清楚。

## 📍 IP 位址 — 網路世界的門牌號碼

在現實世界，每棟房子都有地址，快遞才能找到你。在網路世界，每一台連網的設備都需要一個 IP 位址（IP Address），它就是設備在網路上的「門牌號碼」。

目前最常見的格式叫 IPv4，長這樣：`192.168.1.100`，由四組 0–255 的數字組成，用點隔開，總共 32 個 bit，理論上能表示約 43 億個地址。聽起來很多，但全球設備數早就超過了這個數字，所以現在也在推廣 128 bit 的 IPv6，地址數量幾乎是無限的。

![公網 IP 是你家的完整郵寄地址，全台灣只有你這一個；內網 IP 是社區公寓裡的房號，只在社區內部有意義，外面的人根本不知道。](https://cdn-images-1.medium.com/max/800/1*zR1KXF2OVCbPSkBfsoIjvg.png)
*公網 IP 是你家的完整郵寄地址，全台灣只有你這一個；內網 IP 是社區公寓裡的房號，只在社區內部有意義，外面的人根本不知道。*

## 🗺️ 網段與子網路遮罩 — 劃分地盤

IP 位址不是一個個孤立存在的，它們被分成一組一組的「網段（Network Segment）」來管理。就像一個城市被劃分成不同的行政區，同一個網段的設備可以直接溝通，不同網段之間就要透過路由器才能往來。

判斷哪些 IP 屬於同一個網段，靠的是「子網路遮罩（Subnet Mask）」，常見格式例如 `255.255.255.0`，或者寫成更簡潔的 CIDR 表示法：`/24`。

![](https://cdn-images-1.medium.com/max/800/1*5ltFIHEXFnDvYxLcMSWltw.png)

遮罩中「`255`」的部分表示「這段 bit 屬於網路識別」，「`0`」的部分表示「這段 bit 是主機編號」。遮罩越大（如 `/16`），網段包含的 IP 就越多；遮罩越小（如 `/30`），網段裡能用的 IP 就越少。

## 🚪 Gateway（閘道） — 離開的出口

當你的設備要傳資料到不同網段（或者上網）時，它不能直接衝出去，它要先把封包交給「預設閘道（Default Gateway）」。閘道通常就是你家路由器的內網 IP，例如 `192.168.1.1`。

![你住在一棟公寓裡。要去隔壁棟，你得先走到社區的大門口（閘道），讓警衛（路由器）幫你指路。沒有閘道，你就出不去。](https://cdn-images-1.medium.com/max/800/1*IURfXCOctnaFHZ-lAgU4Jg.png)
*你住在一棟公寓裡。要去隔壁棟，你得先走到社區的大門口（閘道），讓警衛（路由器）幫你指路。沒有閘道，你就出不去。*

## 🔀 路由器 — 網路世界的交通指揮

路由器（Router）的工作就是幫封包找到最佳路徑，把資料從一個網段「路由」到另一個網段。你家的路由器身兼兩職：對內是所有設備的閘道，對外是你與 ISP（網路服務商）之間的橋樑。

路由器內部有一張「路由表（Routing Table）」，記錄著「要到哪個網段，就往哪個介面送」的規則。網際網路上的骨幹路由器每天都在交換彼此的路由資訊（透過 BGP 協定），讓封包能夠在全球數萬個網路之間正確流動。

## 🔁 NAT — 讓多台設備共用一個公網 IP

前面說到公網 IPv4 位址只有 43 億個，早就不夠用了。解決方法之一就是 NAT（Network Address Translation，網路位址轉換）。

你家裡有手機、電腦、平板，但 ISP 只給你一個公網 IP。路由器用 NAT 把所有設備的內網 IP，統一翻譯成那一個公網 IP 對外發送；收到回應時，再根據記錄把封包分發給對應的內部設備。

![](https://cdn-images-1.medium.com/max/800/1*rFuTkbZ1iB7ifOFJwPybdA.png)

這也是為什麼你家的設備從外面無法被直接連線 — — NAT 擋在前面，外部請求不知道要找哪台設備。這其實也帶來了一定的安全性（雖然它不是防火牆）。

## 🚢 Port（埠） — 同一個 IP 上的多個服務入口

一台伺服器的 IP 位址只有一個，但它可能同時跑著網站、郵件、SSH 等多種服務。那怎麼區分？靠的是 Port（埠） — — 一個 0 到 65535 的數字，附加在 IP 後面，指定要找哪一個服務。

![IP 是一棟大樓的地址，Port 是大樓裡各個部門的分機號碼。「台北市中山路 1 號，分機 80」 — — 你就找到網頁服務了。](https://cdn-images-1.medium.com/max/800/1*y6WFgMcTGz-svXHv4cj4Ig.png)
*IP 是一棟大樓的地址，Port 是大樓裡各個部門的分機號碼。「台北市中山路 1 號，分機 80」 — — 你就找到網頁服務了。*

## 📖 DNS — 網路世界的電話簿

電腦只認識 IP 位址，但人類記不住 `142.250.185.78`，所以我們發明了好記的名字，叫做「網域名稱（Domain Name）」，例如 `google.com`。而 DNS（Domain Name System）就是負責把名字翻譯成 IP 位址的系統。

![](https://cdn-images-1.medium.com/max/800/1*2c6PX253DGHqjEbWsXiJzw.png)

整個查詢過程是分層的：從「根 DNS 伺服器」問 `.com` 在哪，再從 `.com` 的 DNS 問 `google.com` 在哪，最後拿到 IP。這個過程叫做「遞迴查詢」，通常在幾毫秒內完成，而且結果會被快取，所以你感覺不到延遲。

## 🏷️ 網域（Domain） — 你在網路上的名字

網域名稱有層次結構，從右到左讀：`blog.example.com` 裡，`.com` 是「頂級域（TLD）」，`example` 是「二級域」，`blog` 是「子域（Subdomain）」。

常見的頂級域有 `.com`（商業）、`.org`（組織）、`.tw`（台灣）、`.edu`（教育）等。你向「域名註冊商」付費購買的是二級域的使用權，之後你可以自己設定任意的子域。

![](https://cdn-images-1.medium.com/max/800/1*HoPD1XLjS8yfzBJOkLfhdA.png)

## 📡 HTTP 與 HTTPS — 資料怎麼傳

有了 IP、有了 DNS 找到伺服器地址，接下來要說「用什麼語言溝通」。HTTP（HyperText Transfer Protocol）就是瀏覽器和伺服器之間的共同語言，定義了如何請求資源、如何回應。

![](https://cdn-images-1.medium.com/max/800/1*U02M9U3IqltzhO6Jglm5cw.png)

HTTPS 就是 HTTP + TLS 加密。所有傳輸的內容都被加密，即使有人在中間攔截封包，也看不到你在看什麼、傳了什麼。現代瀏覽器對沒有 HTTPS 的網站會顯示「不安全」警告。

HTTP 的請求方法中，`GET` 是讀取資料，`POST` 是送出資料，`PUT`/`PATCH` 是更新，`DELETE` 是刪除——這套規則也是現代 REST API 的基礎。

## 📶 ISP — 幫你接上網際網路的那條線

ISP（Internet Service Provider，網際網路服務提供商）是讓你能上網的電信或網路公司，在台灣例如中華電信、台灣大哥大、遠傳、亞太電信等。

你家的寬頻合約就是和 ISP 簽的。ISP 把自己的網路骨幹連接到全球的網際網路交換節點（IX），你的每一個網路請求都要先經過 ISP 的基礎設施才能到達外面的世界。ISP 也是分配你公網 IP 的那個角色。

不同 ISP 的網路品質、速度、以及連往國際的線路品質都不同，這也是為什麼在台灣用某些 ISP 連美國伺服器特別快，用另一家卻很慢。

## 🔗 把所有概念串起來：你按下 Enter 的那一刻

現在我們可以完整地描述「在瀏覽器輸入 `https://google.com` 按 Enter」這件事背後發生的一切：

**第一步，DNS 查詢。** 瀏覽器先查本機快取，沒有的話就問路由器（閘道），路由器問 ISP 的 DNS 伺服器，遞迴查詢後拿到 Google 的 IP，例如 `142.250.185.78`。

**第二步，建立連線。** 瀏覽器用你的內網 IP（如 `192.168.1.100`）加上一個臨時 Port，對 Google IP 的 Port 443（HTTPS）發起 TCP 連線。封包到達路由器，NAT 把你的內網位址替換成公網 IP。

**第三步，加密握手。** TLS 協定在瀏覽器和 Google 之間建立加密通道，雙方交換憑證和加密金鑰，確保接下來的通訊沒有人能竊聽。

**第四步，HTTP 請求與回應。** 加密通道建立後，瀏覽器發送 HTTP GET 請求，Google 伺服器回應 200 OK 並傳回 HTML 內容，瀏覽器開始渲染頁面。

**第五步，封包回程。** Google 的回應封包帶著你的公網 IP，經過 ISP 路由回到你家路由器，NAT 查表找到是你的電腦發的請求，把封包送進來，頁面出現在你眼前。

以上這一切，通常在 200 毫秒以內完成。

## 📋 概念速查表

![](https://cdn-images-1.medium.com/max/800/1*Ean976KT-OaryOz9pJCS4w.png)
