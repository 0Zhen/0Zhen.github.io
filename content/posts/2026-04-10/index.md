---
title: "射出成型設計全解說：從流長比到模具廠對應，工程師必看的完整筆記"
date: 2026-04-10T01:25:25Z
description: "這篇文章整理了塑膠射出成型（Injection Molding）在產品設計端與模具端最核心的知識點，適合產品設計師、機構工程師、模具工程師，以及想深入了解製造知識的人閱讀。"
canonicalURL: "https://medium.com/@chrislee8613/%E5%B0%84%E5%87%BA%E6%88%90%E5%9E%8B%E8%A8%AD%E8%A8%88%E5%85%A8%E8%A7%A3%E8%AA%AA-%E5%BE%9E%E6%B5%81%E9%95%B7%E6%AF%94%E5%88%B0%E6%A8%A1%E5%85%B7%E5%BB%A0%E5%B0%8D%E6%87%89-%E5%B7%A5%E7%A8%8B%E5%B8%AB%E5%BF%85%E7%9C%8B%E7%9A%84%E5%AE%8C%E6%95%B4%E7%AD%86%E8%A8%98-48c95851bdfa"
cover:
  image: "https://cdn-images-1.medium.com/max/800/0*KKg-v2fWAJW75OAE.png"
  relative: false
tags: ["射出成型", "模具", "製造", "機構設計"]
---
![http://www.rlhonor.com/tp/image/20140826/20140826154261576157.png](https://cdn-images-1.medium.com/max/800/0*KKg-v2fWAJW75OAE.png)
*http://www.rlhonor.com/tp/image/20140826/20140826154261576157.png*

> *這篇文章整理了塑膠射出成型（Injection Molding）在產品設計端與模具端最核心的知識點，適合產品設計師、機構工程師、模具工程師，以及想深入了解製造知識的人閱讀。*

## 一、流長比（L/t Ratio）：料流走得到嗎？

**流長比**（L/t，Flow Length to Thickness Ratio）是衡量熔融塑膠能否順利充填模穴的關鍵指標：

- **L**：熔膠從澆口（Gate）流到最遠端的距離
- **t**：該流道區域的肉厚（Wall Thickness）

每種材料都有其流長比上限，超過這個值，熔膠在到達末端前就已降溫固化，導致**充填不足（Short Shot）**。

最常見的解決方案是**多點進澆** — — 在多個位置開設澆口，縮短每股料流行走的距離。但代價是：**多股料流匯合必然產生結合線（Weld Line）**，工程師需要判斷結合線位置是否影響外觀或結構強度。

> *💡 設計原則：盡量讓結合線落在非外觀面、非受力區域。*

![流長比 L/t 材料對照表](https://cdn-images-1.medium.com/max/800/1*YtnNydiStpB4OUOay4FmLQ.png)
*流長比 L/t 材料對照表*

各材料 L/t 範圍差異顯著：PS、PA 流動性最佳（可達 300+），PC 與 PVC 流動性較差（僅 90–160），設計時需特別注意澆口布置。加玻纖後 L/t 上限約再打 7–8 折。

## 二、肉厚設計：均勻才是王道

肉厚設計是射出成型中最容易被忽略、卻影響最深遠的環節。**肉厚急遽變化**可能引發兩大問題：

**遲滯現象（Hesitation Effect）**：料流從厚肉區進入薄肉區時，薄處流阻大，料流速度驟降甚至停滯，表面出現流痕或充填不均。

**真空泡（Void）與縮痕（Sink Mark）**：厚肉區外皮先固化，內部塑膠持續收縮卻無法補料 — — 收縮在內部形成真空泡，在表面形成縮痕。

## 結構筋（Rib）的肉厚規則

```typescript
結構筋壁厚 ≤ 主肉厚 × 50%
```

這個原則能有效避免結構筋根部產生縮痕或 Void。Boss（螺絲柱）底部常用「火山口（Moat Cut）」設計 — — 在根部環狀挖槽，進一步減少縮水。

## 三、材料選用：六大考量面向

選材不只是挑「強度夠」的料號，完整評估需涵蓋六個面向：

![](https://cdn-images-1.medium.com/max/800/1*KZ2KUmEBfrLJa0X4WZ1VZg.png)

## 四、物性表怎麼看？兩個容易誤解的數據

## 熱變形溫度（HDT）≠ 長期使用溫度

物性表上的 **HDT（Heat Deflection Temperature）** 是在特定負載（1.82 MPa）下，樣品彎曲達 0.25mm 時的溫度 — — 是短時間量測值，**不代表可以長期在該溫度使用**。長期使用溫度（RTI）通常遠低於 HDT。

## UL 746C → 耐候性評級

戶外或長期暴露於紫外線的產品，需查閱 **UL 746C**（抗 UV 紫外線、抗老化），直接影響材料長期外觀與機械性能穩定性。

![塑膠材料耐熱溫度（HDT）比較](https://cdn-images-1.medium.com/max/800/1*F4vuw6H-Lf0EMF2yvtyqJA.png)
*塑膠材料耐熱溫度（HDT）比較*

三個溫度等級清楚呈現：通用料（PP/PE/PS）HDT 75–100°C、工程料（ABS/PC/PA）100–220°C、特殊工程料（PEEK/PEI/LCP）超過 200°C — — 但成本也大幅躍升。

## 五、高分子聚合物分類

```typescript
高分子聚合物
├── 橡膠（Rubber）
├── 矽膠（Silicone）
└── 塑膠（Plastic）
    ├── 熱固性（Thermoset）  → 固化後不可再加工，如 Epoxy、Phenolic
    └── 熱塑性（Thermoplastic）
        ├── 結晶性（Crystalline）
        └── 非結晶性（Amorphous）
```

## 結晶性 vs 非結晶性

![](https://cdn-images-1.medium.com/max/800/1*7Vpv6Bj51ySLqu9H620l9A.png)

> *💡 選材口訣：要透明選非結晶、要耐化學選結晶、兩者都要考慮 PC。*

## 六、材料價格 vs 耐熱溫度：選材全局觀

![材料價格 vs 耐熱溫度氣泡圖](https://cdn-images-1.medium.com/max/800/1*G8GvFxHkxhVdJlqNmM_6ZA.png)
*材料價格 vs 耐熱溫度氣泡圖*

這張圖呈現選材最核心的取捨 — — **耐溫越高、價格越貴**：

- **左下（通用料）**：PP、PE、PS — 低價低耐溫，適合消費品大量生產
- **中區（工程料）**：ABS、PC、PA、POM — 性價比高，覆蓋大多數工業應用
- **右上（特殊工程料）**：PEEK（$90/kg）、PEI（$35/kg） — 航太、醫療等高規格場景

氣泡大小代表流動性（L/t 上限）：PA 和 PS 氣泡最大，充填最容易；PC 和 PSU 氣泡小，需要更高射壓或更多澆口。

## 七、表面粗糙度規格對照

模具表面規格是設計端與模具廠溝通的共同語言，目前業界常用三套系統：

## SPI（美國塑膠工業協會）

![](https://cdn-images-1.medium.com/max/800/1*EuNBWUvSsr7wT0DU19XJrw.png)

> *⚠️ 常見誤解：SPI 是美國塑膠工業協會制定，不是 DME 發明的。DME 只是製作實物樣品板販售。*

## VDI 3400（德國工程師協會）

由 **Verein Deutscher Ingenieure** 制定，專門針對放電加工（EDM）表面：

![](https://cdn-images-1.medium.com/max/800/1*uGhPuB4BOwlziq9brQ9Oug.png)

## Charmilles 號數

![](https://cdn-images-1.medium.com/max/800/1*x1E7CCBwxLe2JP047DpT0g.png)

**33號** Ra = 4.50 μm，屬粗加工等級，**不是外觀規格**，肉眼可見明顯麻點。

## 八、對應模具廠：設計端需要懂的模具知識

## 模穴數 / 兩板模 vs 三板模

![](https://cdn-images-1.medium.com/max/800/1*Y9V4QRHq9oURSxk4_AYtlg.png)

**加熱澆道（Hot Runner）**：無料頭、縮短週期，適合高產量；但模具成本高、換色困難。

![](https://cdn-images-1.medium.com/max/800/1*KbkTymMIWe52dwaAODACkQ.png)

## 機台噸數

```bash
鎖模力（噸）= 投影面積（cm²）× 材料係數
PP ≈ 0.32  /  ABS ≈ 0.40  /  PC ≈ 0.55  ton/cm²
```

## 模具鋼材

![](https://cdn-images-1.medium.com/max/800/1*4h0Wpma1T2vzRDet-0IxkA.png)

## 冷卻水路 / 頂針 / 排氣

- **冷卻**：佔週期 60–70%；進出水溫差控制在 3°C 以內；複雜件考慮隨形水路（Conformal Cooling）

![傳統水路 vs 隨形水路對比](https://cdn-images-1.medium.com/max/800/1*JHsZ5slIhVz1qQxHNvZBNg.png)
*傳統水路 vs 隨形水路對比*

- **頂針**：均勻分布避免頂白；外觀面改用頂塊或氣頂
- **排氣**：排氣槽深 0.02–0.05mm，設於分模線與末端充填處；不足會導致燒焦或結合線加劇

## 總結：射出成型設計的核心思維

1. **流長比** → 決定澆口數量 → 影響結合線位置
2. **肉厚均勻** → 決定縮痕/Void 風險 → 影響外觀品質
3. **材料特性** → 決定收縮率 → 影響尺寸精度與翹曲
4. **表面規格** → 決定後加工需求 → 影響模具成本
5. **模具設計** → 決定週期與品質 → 影響量產成本

掌握這些知識，設計師才能在產品開發初期做出對製造友善（DFM）的決策，避免開模後才修改的高昂代價。
