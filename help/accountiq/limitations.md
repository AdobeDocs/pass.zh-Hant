---
title: 限制和已知問題
description: 產品中的已知問題。
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 2ced89dc1f77d2c090b599c40e778f3054f1a8dd
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# 已知問題和限制 {#known-issues}

Adobe致力於透過其產品提供強大的功能及順暢的使用者體驗。 Account IQ的目前版本（1.0版）為高度信賴的串流提供者提供使用情況和訂閱共用分析。 不過，下列限制將在即將發行的版本中解決。

* 在控制面板或報告頁面中定義同類群組時，目前沒有選項可新增度量，例如 **裝置數** 以調整區段。 此功能將在未來版本中提供。

* 在預估個別帳戶的共用分數時，帳戶IQ會採取保守的方法，讓公司可高度信賴地執行共用作業。 但是，這種方法傾向於低估彙總多個帳戶時的共用總量。

* 此 [整體共用分數](/help/accountiq/dashboard.md#overall-sharing-score) 目前僅限中的因數 [共用層級](/help/accountiq/dashboard.md#sharing-level) 和 [共用帳戶的使用情況](/help/accountiq/dashboard.md#usage-from-shared-accounts). 未來版本會將其他量度納入考量。

* 在控制面板或報告頁面中定義同類群組時，MVPD和管道的選擇器目前缺乏搜尋機制。

* 在控制面板或報告頁面中定義同類群組時，限制最多只能選取10個MVPD和程式設計人員（或個別管道）。

* 截至目前，匯出帳戶統計資料的選項僅限於匯出1000個帳戶。

* 要選取的選項 [區段型別](#segment-type) 定義操作時，限製為 **固定帳戶數量**. 此 **帳戶數量可變** 選項將在即將推出的版本中提供。

* 左側導覽中的「基準」、「偵測模型」、「區段」、「快照」和「規則」區段目前已停用，未來版本將提供。

* 建立時 [作業](/help/accountiq/operation-affecting-user-segment.md)，您只能識別兩種 [動作](/help/accountiq/operation-affecting-user-segment.md) 截至目前 — 並行監視規則與外部動作。

* 目前，作業只能建立和 [已排程](/help/accountiq/operation-affecting-user-segment.md#action). 未來的版本可讓您暫停、繼續並完整管理這些專案。

* 由於使用的資料集較為有限，隔離模式無法真正反映共用的數量。 因此，隔離模式中的MVPD無法與任何其他MVPD做比較。 <!--do we need to separate out this limitation, which is from a different persona i.e. only for Programmer persona?-->

* 當您定義新的 [區段](/help/accountiq/segments-timeframe.md) 您可以為作業新增量度。 但如果您選取已儲存的區段，就無法新增更多量度來調整區段。

* 詳細程度和時間範圍選擇器限製為一週或一個月，這表示只能在一週或一個月內評估資料。

* 預先定義的間隔目前在詳細程度和時間範圍選擇器中是停用的，並將在未來的版本中提供。
