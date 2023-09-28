---
title: 匯出共用分數較高的帳戶的資訊
description: 匯出共用分數較高的帳戶的資訊。
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 1%

---

# 匯出共用分數較高的帳戶的資訊 {#export-account-info-high-score}

[!UICONTROL Account IQ] 可讓您選擇根據前1000個訂閱者帳戶的帳戶共用詳細資訊 [共用機率](/help/accountiq/product-concepts.md#account-sharing-probability-def). 匯出的CSV檔案中的資料會依訂閱者帳戶的共用機率遞減順序排序，即在 [區段](/help/accountiq/product-concepts.md#segment-def)，代表 [指定的時間範圍](/help/accountiq/product-concepts.md#time-frame-def).

匯出帳戶共用資訊的選項位於 [一般使用報表](/help/accountiq/general-usage-reports.md) 和 [共用帳戶報表](/help/accountiq/shared-acc-reports.md) 頁面。

>[!NOTE]
>
>下載的CSV檔案中，「一般使用」和「共用帳戶」報表頁面的數字不同。 這是因為「一般使用報表」頁面有額外的篩選器，供程式設計師為裝置數、IP和郵遞區號選取臨界值。 因此，從「一般使用」報表匯出的資料會以套用的其他臨界值篩選器為基礎。

![一般用途中的匯出選項](assets/export.png)

若要匯出訂閱者的帳戶共用資訊，請執行下列動作：

1. 請依照中的步驟定義所需的區段 [如何定義區段及選取時間範圍](/help/accountiq/howto-select-segment-timeframe.md) 評估來源： [區段和時間範圍](/help/accountiq/segments-timeframe.md) 面板。

1. 選取 **[!UICONTROL Export top 1000 accounts]** 用於匯出1000個擁有最高共用機率訂閱者的帳戶資訊的選項。

當您使用匯出選項時，具有最高共用機率（針對定義的時間範圍）的1000個帳戶的統計資料會下載到您本機電腦的Downloads資料夾。

>[!NOTE]
>
>您可以使用任何可讀取CSV檔案的應用程式(例如Microsoft Excel)來開啟下載的CSV檔案。

![以csv格式匯出的資料](assets/exported-csv.png)

*圖：以CSV格式匯出的共用帳戶資料*

## 匯出報告中的欄 {#columns-in-export}

**周/月**

您在「 」上選取的周或月 **[!UICONTROL Granularity and Time Frame]** 區段選擇器中的選項，用於搜尋共用統計資料。

**MVPD**

如果您是程式設計師使用者，欄會顯示訂戶帳號屬於哪個MVPD。

**訂閱者ID**

我們連續討論的特定帳戶。

**裝置數下限**

裝置的實際數量（該串流內容）幾乎肯定大於為特定帳戶指定的裝置最小數量。

>[!NOTE]
>
>裝置的實際數量（該串流內容）當然大於為特定帳戶指定的裝置最小數量。

**人員最小數量**

使用這些裝置使用作用中串流內容的絕對最小人數。

>[!NOTE]
>
>實際人數（該串流內容）幾乎肯定遠遠大於為特定帳戶指定的最小人數。

**[!UICONTROL # IPs]**

從中對內容進行串流的IP位址數量。

**[!UICONTROL # Locations]**

內容串流來源的位置數（根據郵遞區號）。

**[!UICONTROL # Cities]**

串流已發生的城市數。

**[!UICONTROL # States]**

串流已發生的州數。

**[!UICONTROL # Clusters]**

不同數量 [叢集](/help/accountiq/product-concepts.md#cluster-def) 已進行串流的位置。

**[!UICONTROL Geographic span (miles)]**

和帳戶相關聯的串流位置之間的最大距離。

**[!UICONTROL # AuthN OK]**

使用者在該期間使用該帳戶登入的次數。

**[!UICONTROL # AuthZ OK]**

MVPD授權該帳戶資料流或授予該帳戶存取權（內容）的次數。

>[!NOTE]
>
>此 **[!UICONTROL # AuthZ OK]** 與 **[!UICONTROL # Play Requests]**；它小於 **[!UICONTROL # Play Requests]** 因為Adobe通常會在24小時內快取針對MVPD取得的授權。

**[!UICONTROL # Play Requests]**

時段內的實際串流數量。

**[!UICONTROL # Channels]**

帳戶在時段內觀看的不同管道總數。

>[!NOTE]
>
>**[!UICONTROL # Channels]** 包括不一定屬於登入程式設計人員的管道。
>
>因為帳戶曾觀看您的頻道，但在此期間也存取了其他頻道，所以會顯示此帳戶號碼。

**使用模式**

此欄中的數字是識別碼，對應到我們識別所有使用者帳戶的14個模式之一。

*表格：轉存的CSV對應中的使用模式識別碼與使用模式*

| ID | 1 | 2 | 3 | 4 | 5和8 | 6 | 7 | 9 | 10和11 | 12 | 13 | 14 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 使用模式 | 一般使用者 | 旅行者或通勤者 | 大家庭 | 親朋好友 | 社交群組共用 | 一大群朋友 | 同時串流 | 社群共用 | 不確定的行為 | 小型家庭 | 第二個首頁 | 使用方式異常 |

{style="table-layout:auto"}

**共用機率**

共用機率是指特定帳戶共用其認證的機率。

>[!NOTE]
>
> 系統會使用所有科目（在選取的區段中）的分享機率平均值來計算 [共用層級](/help/accountiq/dashboard.md#sharing-level) 的 [彙總的共用分數](/help/accountiq/dashboard.md#aggregated-sharing).
