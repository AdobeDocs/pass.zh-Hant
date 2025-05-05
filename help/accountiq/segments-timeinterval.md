---
title: 區段和時間間隔
description: 定義同類群組或選取訂閱者區段，以評估在Account IQ中使用圖形工具與報表的頻道檢視器帳戶共用可能性與模式。
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# 區段和時間間隔 {#segment-timeinterval}

登入Account IQ時，位於控制面板上方的區段和時間間隔面板可讓您定義訂閱者[區段](product-concepts.md#segmet-def)。 此面板有助於篩選結果並顯示訂閱者共用行為和模式的報告。 目前預設會選取名為&#x200B;**您屬性**&#x200B;中所有帳戶的區段，您可以在其中檢視下列選項：

![](assets/new-segment-selector-collapsed.png){align="left"}

*具有摺疊區段摘要的區段和時間間隔面板*

**A.**&#x200B;目前選取的區段名稱&#x200B;**B.**&#x200B;開啟區段清單&#x200B;**C.**&#x200B;編輯區段&#x200B;**D.**&#x200B;建立新區段&#x200B;**E.**&#x200B;詳細程度和時間間隔選擇器&#x200B;**F.**&#x200B;圖示以展開區段摘要&#x200B;**G.**&#x200B;摺疊的區段摘要&#x200B;**H.**&#x200B;所選時間間隔的區段帳戶數

>[!NOTE]
>
> 收合的區段摘要會顯示在Account IQ的TV Everywhere版本中使用的[視訊類別](product-concepts.md#video-category-def)。 如果您以D2C服務身分登入，這些標籤會顯示您公司的特定視訊類別。

深入瞭解[如何從左側面板的&#x200B;**區段**&#x200B;索引標籤建立](work-with-segments.md#create-new-segment)及[管理區段](work-with-segments.md#manage-segment)。

## 區段選取 {#segment-selection}

若要選取特定區段，請遵循下列步驟：

1. 導覽至區段和時間間隔面板中的&#x200B;**[!UICONTROL Open segment]**&#x200B;選項。
1. 選取您要檢視其帳戶共用報表的&#x200B;**區段名稱**。

   ![](assets/open-segment.png){align="left"}

   *選取區段名稱*

   >[!NOTE]
   >
   > 上一個影像中顯示的視訊類別，例如&#x200B;**MVPD**、**程式設計人員**&#x200B;和&#x200B;**頻道**，代表在Account IQ的TV Everywhere版本中使用的標籤。 如果您以D2C服務身分登入，這些標籤會顯示您公司的特定視訊類別。

1. 選取&#x200B;**[!UICONTROL Open segment]**。


## 詳細程度和時間間隔選擇 {#granularity-timeinterval}

**詳細程度和時間間隔**&#x200B;選擇器可讓您指定每週/每月彙總的日期和持續時間，以觀察訂閱者共用行為。 預設選擇為本週。

![詳細程度和時間間隔](assets/granularity-timeinterval-weekwise.png){align="left"}

*詳細程度和時間間隔對話方塊*

**A.**&#x200B;詳細程度與時間間隔選擇器&#x200B;**B.**&#x200B;往下月/周的向右箭號&#x200B;**C.**&#x200B;按周/月選擇詳細程度的選項&#x200B;**D.**&#x200B;目前選取的時間間隔&#x200B;**E.**&#x200B;往上月/周的向左箭號

您可以使用以下步驟修改持續時間：

1. 從日期選擇器選取&#x200B;**[!UICONTROL Granularity and Time Interval]**。

1. 從&#x200B;**[!UICONTROL Aggregate By]**&#x200B;選項中選取&#x200B;**[!UICONTROL Week]**&#x200B;或&#x200B;**[!UICONTROL Month]**&#x200B;以設定您評估的粒度。

1. 選取詳細程度後，您就可以使用向前或向後箭頭來瀏覽時間範圍。

1. 選取要評估的特定時段。

1. 選取&#x200B;**[!UICONTROL Apply]**&#x200B;以確保您的選取生效。

這可讓您將問題陳述定義為「在12月選擇當週觀看頻道X、Y和Z的MVPD A訂閱者」。

## 區段摘要 {#segment-summary}

D2C服務和所有地方的電視的區段摘要都類似。 每個Account IQ版本的影片類別將會不同。

選取 <img alt= "展開區段摘要" src="./assets/expand-segment-summary.svg" width="25">圖示可檢視詳細的區段摘要。 它也會顯示所選時段內訂閱者帳戶的數目及其播放要求的資訊。

+++ D2C服務

![](assets/segment-panel-d2c.png){align="left"}

D2C服務的&#x200B;*區段摘要*

>[!NOTE]
>
>上一張影像中顯示的[視訊類別](product-concepts.md#video-category-def)，例如區段中的&#x200B;**區域**&#x200B;和&#x200B;**內容型別**&#x200B;只是範例。 當您登入Account IQ時，這些標籤會顯示您公司的特定影片類別。

**區段摘要**&#x200B;包含下列定義區段的條件：

區段&#x200B;**中的**&#x200B;[&#x200B;地區和內容型別](product-concepts.md#video-category-def)參考與視訊串流關聯的中繼資料標籤，這些視訊串流由帳戶共用報表中表示的共用帳戶觀看。

區段&#x200B;**中的**&#x200B;[&#x200B;量度](product-concepts.md#metric)參考訂閱者必須符合哪些屬性或條件才能在帳戶共用報表中識別。

+++

+++ 到處都是電視

![](assets/segment-panel-programmers-mvpd.png){align="left"}

程式設計師/MVPD的&#x200B;*區段摘要*

**區段摘要**&#x200B;包含下列定義區段的條件：

區段&#x200B;**中的**&#x200B;[&#x200B;程式設計師](product-concepts.md#programmer-def)是指內容提供者，其視訊資料流由帳戶共用報表中呈現的共用帳戶觀看。

區段&#x200B;**中的**&#x200B;[&#x200B;頻道](product-concepts.md#channel-def)是指其視訊資料流由帳戶共用報表中表示的共用帳戶所觀看的頻道。

區段&#x200B;**中的**&#x200B;[ MVPD](product-concepts.md#mvpd-def)是指與訂閱者相關聯的多視訊節目經銷商，以便在帳戶共用報表中識別。

區段&#x200B;**中的**&#x200B;[&#x200B;量度](product-concepts.md#metric)參考訂閱者必須符合哪些屬性或條件才能在帳戶共用報表中識別。

+++
