---
title: 基本授權 — 主要應用程式 — 流程
description: REST API V2 — 基本授權 — 主要應用程式 — 流程
exl-id: 46bc9326-966e-44fc-8546-2f58be01b7bc
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---

# 主要應用程式內執行的基本授權流程 {#basic-authorization-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> REST API V2實作受到[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)檔案的限制。

Adobe Pass驗證許可權內的&#x200B;**授權流程**&#x200B;可讓串流應用程式判斷MVPD是否允許或拒絕使用者串流內容的請求。 如果決定為`Permit`，回應會包含媒體權杖。 Adobe Pass伺服器會簽署媒體權杖，並容許串流應用程式在發行串流之前，使用媒體權杖驗證器程式庫來檢查其真實性。

透過媒體權杖驗證器程式庫的驗證，應該發生在連結在許可權鏈中的串流應用程式後端服務上，以從CDN發行資料流。

## 使用特定mvpd擷取授權決策 {#retrieve-authorization-decisions-using-specific-mvpd}

### 先決條件 {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

使用特定MVPD擷取授權決定之前，請確定符合下列先決條件：

* 串流應用程式必須具備使用其中一個基本驗證流程為MVPD成功建立的有效一般設定檔：
   * [在主要應用程式內執行驗證](rest-api-v2-basic-authentication-primary-application-flow.md)
   * [使用預先選取的mvpd在次要應用程式內執行驗證](rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [在次要應用程式內執行驗證，而不預先選取mvpd](rest-api-v2-basic-authentication-secondary-application-flow.md)
* 串流應用程式必須先擷取授權決定，才能播放使用者選取的資源。

### 工作流程 {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

請依照指定的步驟，使用在主要應用程式內執行的特定MVPD來實作基本授權流程，如下圖所示。

![使用特定mvpd擷取授權決定](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png)

*使用特定mvpd擷取授權決定*

1. **擷取授權決定：**&#x200B;串流應用程式會呼叫Decisions Authorized端點，收集所有必要資料以取得特定資源的授權決定。

   >[!IMPORTANT]
   >
   > 請參考[使用特定mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API擷取授權決定，以取得以下詳細資訊：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`、`mvpd`和`resources`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`和`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭

1. **尋找一般設定檔：** Adobe Pass伺服器會根據收到的引數和標頭識別有效的設定檔。

1. **擷取所要求資源的MVPD決定：** Adobe Pass伺服器會呼叫MVPD授權端點，以取得從串流應用程式接收之特定資源的`Permit`或`Deny`決定。

1. **傳回`Permit`決定，媒體權杖：**&#x200B;決定授權端點回應包含`Permit`決定和媒體權杖。

   >[!IMPORTANT]
   >
   > 請參閱使用特定mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API檔案的[擷取授權決定，以取得決定回應中提供的詳細資訊。
   > 
   > <br/>
   > 
   > Decisions Authorize端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   > * 提供的`serviceProvider`與`mvpd`之間的整合必須是作用中。
   >
   > <br/>
   > 
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的額外資訊。

1. **使用媒體權杖開始串流：**&#x200B;串流應用程式使用媒體權杖播放內容。

1. **傳回`Deny`決定，詳細資料：** Decisions Authorize端點回應包含`Deny`決定和依循[增強式錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的錯誤承載。

   >[!IMPORTANT]
   >
   > 請參閱使用特定mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API檔案的[擷取授權決定，以取得決定回應中提供的詳細資訊。
   > 
   > <br/>
   > 
   > Decisions Authorize端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   > * 提供的`serviceProvider`與`mvpd`之間的整合必須是作用中。
   >
   > <br/>
   > 
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的額外資訊。

1. **處理`Deny`決定詳細資料：**&#x200B;串流應用程式會處理回應中的錯誤資訊，並可使用它選擇性地在使用者介面上顯示特定訊息。
