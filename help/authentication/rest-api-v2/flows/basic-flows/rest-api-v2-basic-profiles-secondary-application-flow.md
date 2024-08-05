---
title: 基本設定檔 — 次要應用程式 — 流量
description: REST API V2 — 基本設定檔 — 次要應用程式 — 流量
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '371'
ht-degree: 0%

---


# 在次要應用程式內執行的基本設定檔流程 {#basic-profiles-flow-secondary-application}

Adobe Pass驗證許可權內的&#x200B;**設定檔流程**&#x200B;可讓次要應用程式存取作用中使用者登入的相關資訊。

基本設定檔流程可讓您查詢下列案例：

* [擷取特定程式碼的設定檔](#retrieve-profile-for-specific-code)

## 擷取特定程式碼的設定檔 {#retrieve-profile-for-specific-code}

### 必要條件 {#prerequisites-retrieve-profile-for-specific-code}

在擷取特定驗證代碼的設定檔之前，請確定您符合下列必要條件：

* 次要應用程式（具有用來與MVPD執行互動式驗證的`code`）想要擷取特定驗證程式碼的設定檔。

### 工作流程 {#workflow-retrieve-profile-for-specific-code}

請依照指定的步驟，針對在次要應用程式內執行的特定驗證程式碼，實作基本設定檔擷取流程，如下圖所示。

![擷取特定程式碼的設定檔](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-profile-within-secondary-application-for-specific-code.png)

*擷取特定程式碼的設定檔*

1. **擷取特定程式碼的設定檔：**&#x200B;次要應用程式會收集所有必要資料，藉由傳送要求至設定檔端點來擷取該特定驗證程式碼的設定檔資訊。

   如需下列詳細資訊，請參閱特定程式碼](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md) API檔案的[擷取設定檔：
   * 所有&#x200B;_必要的_&#x200B;引數，如`serviceProvider`和`code`
   * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`
   * 所有&#x200B;_選用的_&#x200B;引數和標頭

1. **尋找一般設定檔：** Adobe Pass伺服器會根據收到的引數和標頭識別有效的設定檔。

1. **傳回關於一般設定檔的資訊：**&#x200B;設定檔端點回應包含關於所找到之設定檔的相關資訊，這些設定檔與收到的引數和標題相關聯。

   請參閱特定程式碼](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md) API檔案的[擷取設定檔，以取得設定檔回應中提供的詳細資訊。

   >[!NOTE]
   >
   > 設定檔端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   >
   > <br/>
   > 
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](../../../enhanced-error-codes.md)檔案的額外資訊。

1. **表示驗證流程已完成，成功：**&#x200B;如果Profiles端點回應包含設定檔，次要應用程式會處理回應，並可以使用它選擇性地在使用者介面上顯示特定訊息。

1. **表示驗證流程發生問題：**&#x200B;如果Profiles端點回應不包含設定檔，次要應用程式會處理回應，並可以使用它選擇性地在使用者介面上顯示特定訊息。
