---
title: 共用帳戶報表
description: 共用帳戶報表
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---

# 共用帳戶報表 {#shared-accounts-reports}

「共用科目報表」提供另一組圖表與圖表，以反映目前節段的共用行為與沖銷。 例如，目前區段的&#x200B;**[!UICONTROL Over Moderate Probability]**&#x200B;和&#x200B;**[!UICONTROL Over Low Probability]**。

## 帳戶共用機率 {#accounts-sharing-probability}

此環圈圖和長條圖顯示屬於特定共用機率範圍的訂閱者帳戶百分比（和絕對數字）。 這些範圍的定義如下：

* 非常高(80%-100%)
* 高(60%-80%)
* 中等(40%-60%)
* 低(20%-40%)
* 極低(0%-20%)

紅線標示目前區段](#threshold-selector)面板中[帳戶超過臨界值選取的臨界值範圍，淺紅色區域包含超過該臨界值的所有帳戶總數。

![](assets/accounts-sharing-probability-pie.png)

長條圖會針對每個範圍（繪製在x軸上），在y軸上繪製屬於每個範圍的帳戶數。

![](assets/accounts-sharing-probability-bar.png)

紅色區域同樣會標籤目前的臨界值，而淺紅色區域則包含超過該臨界值的所有帳戶總數。

>[!NOTE]
>
> 長條圖的y軸為對數。

### 目前區段中的科目超過臨界值{#threshold-selector}

此面板可讓您選取上方環形圖和長條圖的臨界值範圍。 四個選項包括：

* 帳號&#x200B;**超低**&#x200B;共用&#x200B;**機率**

* 帳號&#x200B;**低**&#x200B;共用&#x200B;**機率**

* 帳號&#x200B;**超過稽核**&#x200B;共用&#x200B;**機率**

* 帳號&#x200B;**超過高**&#x200B;共用&#x200B;**機率**

![](assets/threshold-selector-shared-accounts.png)

選取臨界值後，面板會顯示所選區段中所有訂戶帳戶的帳戶百分比（和數目）。

## 區段播放請求總計 {#play-request-out-total}

環形圖顯示區段中訂閱者發出的播放要求百分比（和數量），可讓您比較不在定義區段中的訂閱者發出的播放要求。

![](assets/play-req-outof-total.png)

當您將游標移至環圈圖上時，它也會顯示來自不同機率範圍的訂戶百分比和數字。

<!--![](assets/play-request-total.gif)-->

## 區段平均每個帳戶的裝置數{#avg-devices-account}

長條圖顯示目前區段中的訂閱者目前使用以及目前區段以外的訂閱者目前使用之每種型別的平均裝置數。

![](assets/avg-devices-per-acc.png)

## 每個帳戶每個期間的區段郵遞區號 {#zip-codes-period-account}

此圖表會通知您目前區段中，在指定時間間隔內從不同位置使用內容（如郵遞區號所測量）的訂閱者人數。

![](assets/zip-period-account.png)

>[!NOTE]
>
>您可以放大代表一組以上郵遞區號之長條，以&#x200B;**+** （加號） （例如10+）表示，方法是按兩下這些長條。


## 每個帳戶的每個期間的區段 — 地理範圍 {#geo-span-period-account}

此長條圖會以英里為單位，繪製出從屬於不同地理範圍之地點消費內容的訂閱者帳戶數量。 此範圍是以時間間隔期間訂閱者已串流處理之位置間的最大距離為基礎。

![](assets/geogr-span-account.png)

>[!NOTE]
>
> 您可以按兩下長條圖，放大代表一組以上地理距離的長條，以&#x200B;**+** （加號） （例如，1000+）表示。

>[!MORELIKETHIS]
>
>* 瞭解如何使用[匯出前1000個帳戶](/help/accountiq/export-acc-information.md)選項，使用共用帳戶報表中的篩選器，匯出所選區段中前1000名訂閱者的報表。
