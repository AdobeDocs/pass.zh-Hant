---
title: 使用主控台應用程式記錄檔對AccessEnabler iOS/tvOS SDK進行除錯
description: 使用主控台應用程式記錄檔對AccessEnabler iOS/tvOS SDK進行除錯
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---

# （舊版）使用主控台應用程式記錄檔對AccessEnabler iOS/tvOS SDK進行除錯 {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

## 概觀

本檔案的範圍是擷取並呈現AccessEnabler的iOS/tvOS SDK記錄機制的演化，連同一些實用的詳細資訊，以便使用主控台應用程式記錄來偵錯AccessEnabler架構。

## 記錄機制狀態

AccessEnabler iOS/tvOS記錄機制的用途是發出有用的訊息，用於疑難排解使用AccessEnabler架構的應用程式可能因此而遇到的問題。

### AccessEnabler iOS/tvOS 3.5.0和更新版本

從AccessEnabler iOS/tvOS 3.5.0版開始，記錄機制會匯入下列改善作為變更：

* AccessEnabler架構使用Apple建議的[OSLog](https://developer.apple.com/documentation/os/oslog)實作。

* AccessEnabler架構引入依據子系統&#x200B;**com.adobe.pass.AccessEnabler**&#x200B;篩選主控台應用程式記錄檔的功能。 SDK發出的所有訊息均屬於com.adobe.pass.AccessEnabler。

* AccessEnabler架構引入依據任何（前置詞）篩選主控台應用程式記錄檔的功能： **[AccessEnabler]**。 SDK發出的所有訊息都會加上前置詞[AccessEnabler]。

* AccessEnabler架構引入依據類別： **debug**、**error**&#x200B;以及上述兩個條件的任何一個來篩選主控台應用程式記錄檔的功能： Subsystem或Any （前置詞）。

## 使用主控台應用程式記錄檔進行除錯

根據所調查的問題，您可能希望納入或排除AccessEnabler架構發出的記錄訊息，因此，您可以在下方找到一些實用的詳細資料，這些詳細資料可能有助於您進行調查期間和使用主控台應用程式記錄時。


### AccessEnabler iOS/tvOS 3.5.0和更新版本

#### 包含 {#including}

首先，為了能夠檢視AccessEnabler架構所發出的任何記錄訊息，您&#x200B;**必須**&#x200B;在主控台應用程式的「動作」區段中選取「包含資訊訊息」和「包含偵錯訊息」，如下圖所示。

![](/help/authentication/assets/include-info-debug-msg.png)


為了能夠偵錯AccessEnabler iOS/tvOS SDK的功能和&#x200B;**檢視** AccessEnabler架構記錄，您可以：

* 使用等於com.adobe.pass.AccessEnabler值的&#x200B;**Subsystem**&#x200B;選項在主控台應用程式中搜尋，如下圖所示。

![](/help/authentication/assets/subsys-console-app.png)

* 使用&#x200B;**任何**&#x200B;選項(包含
  [AccessEnabler]值，如下圖所示。

![](/help/authentication/assets/any-optn-console-app.png)

除了上述兩個條件以外，您也可以搭配&#x200B;**子系統**&#x200B;或&#x200B;**任何（前置詞）**&#x200B;使用&#x200B;**類別**&#x200B;選項，明確搜尋AccessEnabler iOS/tvOS SDK發出的&#x200B;**偵錯**&#x200B;或&#x200B;**錯誤**&#x200B;層級訊息。

#### 排除

為了能夠更好地偵錯其他元件的功能和&#x200B;**排除** AccessEnabler架構記錄檔，您可以：

* 使用不等於com.adobe.pass.AccessEnabler值的&#x200B;**子系統**&#x200B;選項在主控台應用程式中搜尋。
* 使用不包含&#x200B;**AccessEnabler**&#x200B;值的[Any]選項在主控台應用程式中搜尋。

## 報告問題

當您向Adobe Pass驗證回報問題時，請考慮下列建議：

* 請試著提供重製步驟。
* 請嘗試提供發生問題的作業系統版本和裝置型號。
* 請嘗試提供遇到問題的AccessEnabler iOS/tvOS SDK版本。
* 請嘗試使用[包含](#including)區段中顯示的兩個選項之一，擷取並附加所有AccessEnabler iOS/tvOS SDK記錄訊息。
