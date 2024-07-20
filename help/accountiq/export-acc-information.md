---
title: 匯出共用分數較高的帳戶的資訊
description: 匯出共用分數較高的帳戶的資訊。
exl-id: df41ddd2-fde3-4861-abd4-6e32f0be9ea5
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 1%

---

# 匯出共用分數較高的帳戶的資訊 {#export-account-info-high-score}

[!UICONTROL Account IQ]可讓您根據前1000個訂閱者帳戶的[共用機率](/help/accountiq/product-concepts.md#account-sharing-probability-def)，匯出其帳戶共用詳細資料。 您可以匯出[共用帳戶報表](/help/accountiq/shared-acc-reports.md)頁面上目前[區段](/help/accountiq/product-concepts.md#segment-def)和[指定時間間隔](/help/accountiq/product-concepts.md#time-interval-def)的帳戶共用資訊。

請依照下列步驟，匯出特定節段之訂戶科目的科目共用資訊。

1. 使用您的憑證登入。
1. 瀏覽至&#x200B;**報表**&#x200B;區段下的&#x200B;**共用帳戶**&#x200B;標籤。
1. 從區段和時間間隔面板中選取所需的區段和時間間隔。 瞭解[如何選取區段和時間間隔](segments-timeinterval.md)。

   如有需要，請參閱[建立區段](work-with-segments.md#create-new-segment)或[編輯區段](work-with-segments.md#edit-segment)的說明。

1. 選取位於區段和時間間隔面板右上角的&#x200B;**[!UICONTROL Export top 1000 accounts]**。

   ![匯出前1000個帳戶](assets/export-top-1000-accounts.png)

   *選取[匯出前1000個帳戶]選項*

檔案會自動以.csv格式下載至您的本機電腦。

此檔案包含前1000個帳戶的資料，其根據為目前區段中訂閱者帳戶的共用機率，依遞減順序排列。

以下是匯出的.csv檔案範例。

![已匯出.csv檔案中的資料](assets/exported-csv.png)

*已匯出.csv檔案中的資料*

## 匯出報告中的欄 {#columns-in-export}

**周/月**

在區段選擇器中&#x200B;**[!UICONTROL Granularity and Time Interval]**&#x200B;選項內選取的周或月。

**MVPD**

如果您是程式設計人員，欄會顯示帳戶訂閱的經銷商。

>[!NOTE]
>
> **MVPD**&#x200B;欄僅適用於TV Everyone版本。

**訂閱者識別碼**

特定帳戶的唯一識別碼。

**裝置數下限**

使用者主動串流內容的最小裝置數量。

>[!NOTE]
>
>串流內容的實際裝置數大於為特定帳戶指定的最小裝置數。

**人員數目下限**

使用這些裝置主動串流內容的個人最小數量。

>[!NOTE]
>
>串流內容的個人實際人數大於指派給特定帳戶的最小人數。

**[!UICONTROL # IPs]**

從中對內容進行串流的IP位址數量。

**[!UICONTROL # Locations]**

內容串流來源的位置數（根據郵遞區號）。

**[!UICONTROL # Cities]**

已發生串流活動的城市數。

**[!UICONTROL # States]**

串流活動發生的狀態數。

**[!UICONTROL # Clusters]**

已進行串流的不同[叢集](/help/accountiq/product-concepts.md#cluster-def)數目。

**[!UICONTROL Geographic span (miles)]**

和帳戶相關聯的串流位置之間的最大距離。

**[!UICONTROL # AuthN OK]**

使用者在指定期間使用該帳戶登入的次數。

>[!NOTE]
>
> 有些D2C服務可能無法看到&#x200B;**[!UICONTROL # AuthN OK]**&#x200B;資料，因為它可能未包含在其公司的資料中。

**[!UICONTROL # AuthZ OK]**

MVPD授權資料流或授予該帳戶內容存取權的次數。

>[!NOTE]
>
>**[!UICONTROL # AuthZ OK]**&#x200B;不適用於D2C服務。

>[!NOTE]
>
>對於所有的電視節目，**[!UICONTROL # AuthZ OK]**&#x200B;與&#x200B;**[#個播放要求](/help/accountiq/product-concepts.md##play-requests-def)**&#x200B;的數目相關。 它一律會小於&#x200B;**[!UICONTROL # Play Requests]**，因為Adobe通常會快取來自MVPD的授權約24小時。


**[!UICONTROL # Play Requests]**

指定時段內發生的實際串流數目。

>[!NOTE]
>
>TV Everywhere MVPD版本無法使用[#播放要求](/help/accountiq/product-concepts.md##play-requests-def)欄。

**[!UICONTROL # Channels]**

帳戶在指定期間內觀看的管道總數。

>[!NOTE]
>
> 對於D2C服務&#x200B;**[!UICONTROL # Channels]**，等同於&#x200B;**[!UICONTROL # Video categories]**&#x200B;的數目。

>[!NOTE]
>
>對於隨處可見的電視，其中包含可能不屬於登入程式設計師的頻道。 此帳戶號碼包含您在指定期間存取的頻道和其他頻道。


**使用模式**

這些欄中的值會作為識別碼，對應至我們用來分類所有使用者帳戶的14個模式之一。

<table>
    <tbody>
      <tr>
        <th style="width:10%">ID</th>
        <th style="width:30%">使用模式</th>
      </tr>
      <tr>
        <td>1</td>
        <td>一般使用者</td>
      </tr>
      <tr>
        <td>2</td>
        <td>旅行者或通勤者</td>
      </tr>
      <tr>
        <td>3</td>
        <td>大家庭</td>
      </tr>
      <tr>
        <td>4</td>
        <td>親朋好友</td>
      </tr>
      </tr>
         <td>5和8</td>
         <td>社交群組共用</td>
      </tr>
      </tr>
         <td>6</td>
         <td>一大群朋友</td>
      </tr>
      </tr>
         <td>7</td>
         <td>同時串流</td>
      </tr>
      </tr>
         <td>9</td>
         <td>社群共用</td>
      </tr>
      </tr>
         <td>10和11</td>
         <td>不確定的行為</td>
      </tr>
      </tr>
         <td>12</td>
         <td>小型家庭</td>
      </tr>
      </tr>
         <td>13</td>
         <td>第二個首頁 </td>
      </tr>
      </tr>
         <td>14</td>
         <td>使用方式異常</td>
      </tr>
    </tbody>
  </table>

*匯出的.csv對應中的使用模式識別碼與使用模式*

**共用機率**

特定帳戶共用其認證的可能性。

>[!NOTE]
>
> 選取區段中所有帳戶的平均共用機率，是用來計算[平均共用分數](/help/accountiq/data-panels.md#aggregated-sharing)的[共用層級](/help/accountiq/data-panels.md#sharing-level)。
