---
title: 如何在並行監視中區分VOD和即時內容
description: 如何在並行監視中區分VOD和即時內容
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---


# 做法：在並行監視中區分VOD和即時內容 {#dist-vod-live}

**問：** 並行監視服務能否區分正在播放的內容型別（即時內容與隨選影片）？



**答：** 並行監視無法直接區分即時內容和隨選影片(VOD)。 視訊播放器必須知道正在播放的內容型別，並在播放期間傳送此資訊。 [工作階段初始化呼叫](/help/concurrency-monitoring/cm-api-overview.md#session-initial) （並行監視的必要專案）。 一般工作流程看起來像這樣：

1. 並行監視客戶定義一組他們想要實作規則的中繼資料（例如content-type=live|vod、device-type=mobile|console|desktop）。
1. 並行監視團隊會實作所需的原則。 範例：
   1. 如果content-type=live，max streams=3，最新wins
   1. 如果content-type=vod，max streams=1，最新wins

1. 當一般使用者播放內容時，視訊播放器必須針對建立為原則一部分的中繼資料欄位傳送值。

1. 並行監視服務會根據定義的原則和收到的值，發佈決定（播放/停止）。

1. 視訊播放器必須遵循決定，系統才能運作。



## 相關資訊 {#related-info-vod-live-dist}

* [並行監視標準中繼資料屬性](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [並行監視API概觀](/help/concurrency-monitoring/cm-api-overview.md)
