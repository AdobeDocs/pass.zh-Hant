---
title: 帳戶IQ中的作業
description: 帳戶IQ中的作業包括執行動作，以在訂閱者帳戶上執行自動化和大量作業並追蹤其效果。
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# 作業 {#operations-tab-next-steps}

當您使用分析訂閱者的使用模式，並識別所選區段的密碼共用例項後， [!DNL Account IQ] analytics，您可以透過中稱為作業的重點程式採取目標行動 [!DNL Account IQ].

**作業** 可讓您有效地追蹤和管理一組帳戶的認證共用，以減少密碼共用並改善重要訂閱者的體驗。

您可以將動作套用至已定義的 [區段](/help/accountiq/product-concepts.md#segment-def) 解決特定密碼共用問題 [時間間隔](/help/accountiq/product-concepts.md#time-interval-def) 並排程作業，以便在未來日期執行。 這些動作包括限制以最小化密碼共用，或簡化非共用帳戶的限制。

使用操作時，您不僅可以指定動作及其範圍，還可以評估其結果。

透過評估結果，您可以調整策略以最佳化效果，無論透過轉換借款人、減少認證共用還是減少流失。

您可以透過操作執行各種功能：

* [檢視作業報表](#operation-reports)
* [建立新作業](#create-new-operation)
* [停止操作](#stop-operation)

## 檢視作業報表 {#operation-reports}

您可以透過作業報告來檢閱作業的效果。 若要檢視作業報表，請選取 **作業** 標籤下的 **動作** 帳戶IQ應用程式的左側面板中。 此時會顯示系統中可用的操作清單。 您可以表格格式存取每個作業的關鍵詳細資訊。 詳細資料包括：

* 作業的名稱
* 目前狀態（例如，已排程、執行中、已結束、錯誤或已停止）
* 進度完成百分比
* 套用作業的目標對象或區段
* 為作業選取的動作型別
* 作業的開始日期
* 作業的結束日期
* 建立作業的日期
* 作業的上次修改日期

![](assets/operations-page.png)

*帳戶IQ中現有作業的清單和詳細資訊*

選取所需的 **作業名稱** 從作業清單中移除。 系統會顯示下列報表：

### 作業績效 {#operation-performance}

作業績效會提供最上線讀出，以彙總作業期間受影響的科目數目、作業進度，以及節段中科目的整體共用分數 [評估期](/help/accountiq/product-concepts.md#evaluation-period-def).

![作業效能報表](assets/operation-performance.png)

*作業效能報表*

**答：** 受影響的帳戶 **B.** 作業進度 **C.** 整體共用分數

#### 受影響的帳戶 {#impacted-accounts}

此數字顯示受作業評估期間所執行動作影響的訂戶帳號計數。

#### 作業進度 {#operation-progress}

此量測計顯示超出計畫排程的天數與作業完成百分比。

#### 整體共用分數 {#overall-sharing-score}

此線圖代表 [整體共用分數](/help/accountiq/data-panels.md#overall-sharing-score)，包括作業評估期間每週共用帳戶的共用層級和使用情況。

### 作業影響：節段中的科目 {#impact-accounts}

此報表會顯示為棧疊的欄點陣圖，以說明作業隨著時間而產生的影響。

![作業對區段圖表中的帳戶的影響](assets/accounts-in-segment.png)

*作業對區段圖表中的帳戶的影響*

x軸代表作業的 [評估期](/help/accountiq/product-concepts.md#evaluation-period-def)，而y軸會指出作業區段中的帳戶狀態。 圖形中的每一個長條都會分成三種顏色：

* 粉紅色代表符合此作業中所使用之區段條件的帳戶數。

* 藍色代表在作業每週或每月中，原本在區段中但不符合區段條件的作用中帳戶數 [評估期](/help/accountiq/product-concepts.md#evaluation-period-def).

* 灰色代表評估期間處於非使用中狀態的帳戶。

>[!NOTE]
>
>第一個粉色列代表評估期間開始時符合作業節段條件的科目數目。

此圖表會說明隨著時間推移帳戶行為相對於原始條件的變化（例如，共用機率超過90且使用5部以上的裝置變為非使用中）。

### 作業影響：共用帳戶量度 {#impact-shared-accounts}

共用帳戶量度提供作業期間作業區段中的訂閱者帳戶共用層級和播放要求的概述 [評估期](/help/accountiq/product-concepts.md#evaluation-period-def).

#### 共用層級 {#share-level}

此線圖代表 [共用層級](/help/accountiq/data-panels.md#sharing-level) 作業評估期間內的每一週。

![共用層級線圖](assets/share-level.png){width="550" align="left"}

*共用層級線圖*

#### 播放要求數目 {#play-requests}

此線圖代表 [播放請求](/help/accountiq/general-usage-reports.md#playreq-uniquesubs) 作業評估期間的每一週。

![播放請求數折線圖](assets/number-play-requests.png){width="550" align="left"}

*播放請求數折線圖*

### 作業影響：一般使用量度 {#impact-general-usage}

一般使用量度提供作業期間作業區段中的平均裝置、IP和位置數量概覽 [評估期](/help/accountiq/product-concepts.md#evaluation-period-def).

#### 裝置數 {#devices}

此線圖代表平均值 [裝置數](/help/accountiq/general-usage-reports.md#devices-week-account) 作業評估期間的每一週。

![裝置數量折線圖](assets/number-devices.png){width="550" align="left"}

*裝置數量折線圖*

#### IP和位置數量 {#IPs-locations}

此線圖代表平均值 [IP數量](/help/accountiq/general-usage-reports.md#ip-week-account) 和 [位置](/help/accountiq/general-usage-reports.md#locations-week-account) 作業評估期間的每一週。

![IP數量和位置折線圖](assets/number-ips-locations.png){width="550" align="left"}

*IP數量和位置折線圖*

若要關閉報表並返回首頁面 **作業** 頁面，選取 **作業** 標籤下的 **動作** 在左側面板中。

## 建立新作業 {#create-new-operation}

當您前往 **作業** 標籤下的 **動作** 在左側面板中，選取 **建立新作業** 在頂端 **作業** 頁面。

若要建立新作業，請遵循下列各節中的指示：

* [作業詳細資料](#operation-details)
* [區段](#segment)
* [動作](#action)
* [排程](#schedule)

### 作業詳細資料 {#operation-details}

在此段落中，輸入作業的名稱 **作業名稱**.

>[!TIP]
>
>說明操作的目的或動作的性質 **操作名稱** 以快速識別。 的選項 **新增說明和標籤** 將在未來版本中提供。

![在作業詳細資料中新增作業名稱](assets/operation-details.png)

*新增操作名稱*

### 區段 {#segment}

在本節中，按一下 **選取區段** 並選擇要使用此作業的區段。 瞭解 [如何選取區段](/help/accountiq/segments-timeinterval.md#segment-selection).

選取區段後，請使用 <img alt= "展開區段摘要" src="./assets/expand-segment-summary.svg" width="25"> 圖示以檢視詳細的區段摘要。 深入瞭解 [區段摘要](segments-timeinterval.md#segment-summary).

![選取區段和時間間隔](assets/select-segment-timeinterval.png)

*選取區段和時間間隔*

>[!NOTE]
>
>此 [視訊類別](product-concepts.md#video-category-def) 顯示於前一個影像，例如 **MVPDs**， **程式設計師**、和 **頻道** 代表用於電視版Account IQ的標籤。 如果您以D2C服務身分登入，這些標籤會顯示您公司的特定視訊類別。

如有需要，請使用 <img alt= "編輯區段" src="./assets/edit-segment.svg" width="25"> 圖示以編輯所選區段或  <img alt= "建立新區段" src="./assets/create-new-segment.svg" width="25"> 圖示以建立新區段。 如需詳細資訊，請參閱 [建立新區段](work-with-segments.md#create-new-segment) 或 [編輯區段](work-with-segments.md#edit-segment).

>[!IMPORTANT]
>
>**區段型別** 已命名 **[!UICONTROL Fixed number of accounts]** 目前預設為選取。 要選取的選項 **[!UICONTROL Variable number of accounts]** 將在即將發行的版本中提供。

選取 **詳細程度和時間間隔** 以監視特定期間的作業。 進一步瞭解 [如何選取詳細程度和時間間隔](/help/accountiq/segments-timeinterval.md#granularity-timeinterval).

### 動作 {#action}

在此段落中，選擇 **動作** 您想要在下拉式選單中選取的區段上執行。

![選取動作型別](assets/apply-actions.png)

*選取動作型別*

有兩個可用選項：

* 選取 **CM原則** 與帳戶IQ整合的並行監視系統。

* 選取 **外部動作** 以建立及處理帳戶IQ外部且未與Account IQ系統整合的工作流程。

>[!NOTE]
>
>外部動作不一定會和密碼共用直接相關，但仍會影響其效能，例如啟動新季節。

### 排程 {#schedule}

在此區段中，選取 **開始日期** 和 **結束日期** 從日期選擇器設定作業的啟動。

>[!IMPORTANT]
>
>目前，預設啟用 **開始日期** 和 **結束日期** 設為 **日期**. 要選取的選項 **當滿足條件時** 和 **手動** 將在即將發行的版本中提供。

>[!NOTE]
>
>請確認開始日期和結束日期與中選取用來評估的詳細程度一致 **步驟4**.

* 如果您已選擇按周彙總粒度，請選取以周為單位的開始和結束日期（例如，第10週）。
* 如果您已選擇按月彙總粒度，請選取以月為單位的開始和結束日期。

![從日期選擇器選取開始日期和結束日期](assets/add-schedule.png)

*從日期選擇器選取開始日期和結束日期*

**答：** 開始日期選擇器 **B.** 結束日期選取器

>[!NOTE]
>
>此 **開始日期** 必須晚於評估期間和目前日期，而 **結束日期** 必須晚於開始日期與目前日期，才能排程及執行未來期間的作業。

選取 **儲存操作** 在頂端 **作業** 頁面以處理新作業。

## 停止操作 {#stop-operation}

您只能停止目前在中執行的作業 **執行中** 狀態。 若要停止現有作業，請遵循下列步驟：

1. 導覽至 **作業** 標籤下的 **動作** 帳戶IQ應用程式左側導覽中的。
1. 選取 **選項** 要停止的作業功能表。

   ![選取選項功能表以停止作業](assets/stop-operation.png)

   *選取「選項」功能表以停止作業*

1. 選取 **停止**.



