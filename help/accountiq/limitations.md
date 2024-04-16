---
title: 限制
description: 瞭解帳戶IQ中程式設計人員的限制和隔離模式MVPD。
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# 限制 {#limitations}

D2C和TV Everywhere版本的Account IQ為串流供應商提供使用情況和訂閱共用分析。 然而，目前的1.3版有某些限制，未來版本將解決這些限制。

* 此 [整體共用分數](/help/accountiq/data-panels.md#overall-sharing-score) 目前包含 [共用層級](/help/accountiq/data-panels.md#sharing-level) 和 [來自共用帳戶的使用情況](/help/accountiq/data-panels.md#usage-from-shared-accounts). 未來版本將包含更多量度。

* 在控制面板上定義區段或使用模式時， [區段中的影片類別](/help/accountiq/data-panels.md#video-categories-segment)， [依管道和MVPD分享分數](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds)、和 [視訊類別的使用模式分佈](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) 報表最多只能顯示20個的資料 [視訊類別](product-concepts.md#video-category-def). 包含超過20個視訊類別的區段不會在這些報表中顯示資料。

* 目前，匯出帳戶統計資料的選項僅限於匯出1000個帳戶。

* 定義作業時，選取的選項 [區段型別](/help/accountiq/operations.md#segment) 僅限於 **固定帳戶數量**. 此 **帳戶數量可變** 選項將在未來版本中提供。

* 此 **基準測試**， **偵測模型**， **動作**、和 **設定** 左側導覽中的區段目前已停用，並將在未來版本中提供。

* 建立操作時，只能套用兩種 [動作](/help/accountiq/operations.md#action)  — 並行監視規則與外部動作。

* 目前，您只能 [建立](/help/accountiq/operations.md#create-new-operation) 和 [排程](/help/accountiq/operations.md#schedule) 作業。 未來的版本可讓您暫停、繼續並完整管理這些專案。

* 選取「粒度」和「時間間隔」時，您只能分析一週或一個月的資料。

* 您無法將隔離模式MVPD加入至含有任何其他MVPD的區段定義中。 有些MVPD無法唯一識別多個程式設計師管道中的訂閱者。 因此，TV Everywhere程式設計師會分別處理這些MVPD。



