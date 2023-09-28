---
title: 共用帳戶報表
description: 共用帳戶報表
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---

# 共用帳戶報表 {#shared-accounts-reports}

「共用帳戶報表」會依選取的共用機率範圍來劃分度量（例如裝置數和裝置型別），例如 **[!UICONTROL Over Moderate Probability]** 和 **[!UICONTROL Over Low Probability]** 用於目前區段。

然後，這些範圍可作為使用者定義的臨界值，且圖表會根據選取的臨界值更新。

科目IQ會根據科目共用機率，將已定義節段的所有訂閱者科目分類為具有下列五個分類的科目：

* 非常高(80%-100%)
* 高(60%-80%)
* 中等(40%-60%)
* 低(20%-40%)
* 極低(0%-20%)

## 帳戶共用機率 {#accounts-sharing-probability}

此處的環圈圖會分類並顯示不同機率類別的訂閱者帳戶百分比（和絕對數字）。

紅線會標籤使用者在下列位置選取的臨界值範圍： [目前區段中的科目超過臨界值](#threshold-selector) 面板。

![](assets/accounts-sharing-probability-pie.png)

長條圖在Y軸上為各種共用機率類別繪製帳戶數（繪製在X軸上）。

![](assets/accounts-sharing-probability-bar.png)

紅線標示臨界值的範圍，可在長條圖中進行調整。 在長條圖中調整的臨界值會反映在環圈圖的臨界值範圍內。

<!--![](assets/shared-accounts-rep.gif)-->

### 目前區段中的科目超過臨界值{#threshold-selector}

此面板可讓您從下列範圍中選取一個範圍，作為訂閱者帳戶的臨界值（根據其共用機率）：

* 帳戶 **非常低** 共用 **機率**

* 帳戶 **過低** 共用 **機率**

* 帳戶 **超過稽核** 共用 **機率**

* 帳戶 **過高的** 共用 **機率**

![](assets/threshold-selector-shared-accounts.png)

選取臨界值後，面板會顯示所選區段中所有訂戶帳戶的帳戶百分比（和數目）。

## 區段 — 播放請求總計 {#play-request-out-total}

環形圖顯示區段中訂閱者發出的播放要求百分比（和數量），並讓您比較不在定義區段中的訂閱者發出的播放要求。

![](assets/play-req-outof-total.png)

當您在環形圖上移動游標時，它也會顯示來自不同機率範圍的訂戶百分比和數字。

<!--![](assets/play-request-total.gif)-->

## Segment — 每個帳戶的平均裝置數{#avg-devices-account}

長條圖顯示目前區段中的訂閱者和目前區段以外的訂閱者所使用的每種裝置型別的平均裝置數。

![](assets/avg-devices-per-acc.png)

## 區段 — 每個帳戶每個期間的郵遞區號 {#zip-codes-period-account}

此圖表會通知您在一個時間範圍內從不同位置使用內容的訂閱者人數。

![](assets/zip-period-account.png)

您可以放大以縮小並檢檢視形中某條圖的具體內容，該圖形繪製了一系列位置。

<!--![](assets/zip-code-period.gif)-->

## 區段 — 地理範圍/期間/帳戶 {#geo-span-period-account}

此長條圖會根據不同地理範圍（以英里為單位）繪制訂閱者帳戶數量。 此範圍是以時間範圍內訂閱者已串流處理之位置間的最大距離為基礎。

<!--Total number of users ...

How many accounts are within 99 miles of each other.....and how many are apart. 

Based on points on the map.-->

![](assets/geogr-span-account.png)

當您選取代表某個地理距離範圍的橫條時，它會展開該範圍以顯示更多詳細資訊。

<!--![](assets/geo-span-period-acc.gif)-->
