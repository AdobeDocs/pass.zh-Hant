---
title: 限制
description: 瞭解Account IQ中程式設計人員的限制和隔離模式MVPD。
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# 限制 {#limitations}

Account IQ的D2C和TV Everywhere版本為串流提供者提供使用情況和訂閱共用分析。 然而，目前的1.3版有某些限制，未來版本將解決這些限制。

* [整體共用分數](/help/accountiq/data-panels.md#overall-sharing-score)目前包含[共用層級](/help/accountiq/data-panels.md#sharing-level)和來自共用帳戶的[使用狀況](/help/accountiq/data-panels.md#usage-from-shared-accounts)。 未來版本將包含更多量度。

* 在儀表板或使用模式上定義區段時，區段中的[視訊類別](/help/accountiq/data-panels.md#video-categories-segment)、[依據管道和MVPD的共用分數](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds)以及視訊類別的[使用模式分佈](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories)報告只能顯示最多20個[視訊類別](product-concepts.md#video-category-def)的資料。 包含超過20個視訊類別的區段不會在這些報表中顯示資料。

* 目前，匯出帳戶統計資料的選項僅限於匯出1000個帳戶。

* 定義Operations時，選取[區段型別](/help/accountiq/operations.md#segment)的選項限製為&#x200B;**固定帳戶數**。 未來版本將提供&#x200B;**帳號變數**&#x200B;選項。

* 左側導覽中的&#x200B;**基準**、**偵測模型**、**動作**&#x200B;和&#x200B;**設定**&#x200B;區段目前已停用，未來版本將提供。

* 建立操作時，您只能套用兩種[動作](/help/accountiq/operations.md#action) — 並行監視規則與外部動作。

* 目前，您只能[建立](/help/accountiq/operations.md#create-new-operation)和[排程](/help/accountiq/operations.md#schedule)作業。 未來的版本可讓您暫停、繼續並完整管理這些專案。

* 選取「粒度」和「時間間隔」時，您只能分析一週或一個月的資料。

* 您無法將隔離模式MVPD加入至含有任何其他MVPD的區段定義中。 有些MVPD無法唯一識別多個程式設計師管道中的訂閱者。 因此，TV Everywhere程式設計師會分別處理這些MVPD。



