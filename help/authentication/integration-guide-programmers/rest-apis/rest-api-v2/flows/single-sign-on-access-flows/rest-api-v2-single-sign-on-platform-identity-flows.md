---
title: 單一登錄 - Platform身份 - 流量
description: REST API v2 - 單一登錄 - Platform身份 - 流量
exl-id: 5200e851-84e8-4cb4-b068-63b91a2a8945
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '1856'
ht-degree: 0%

---

# 使用平臺身份流進行單點登錄 {#single-sign-on-platform-identity-full-flows}

>[!IMPORTANT]
>
> 本頁面上的內容僅供參考。 使用此 API 需要 Adobe Systems 的最新許可證。 不允許未經授權的使用。

>[!IMPORTANT]
>
> REST API V2實作受到[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)檔案的限制。

>[!MORELIKETHIS]
>
> 請確定也造訪[REST API V2常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)。

Platform Identity 方法使多個應用程式能夠在使用Adobe Pass服務時使用唯一的平台標識符在裝置或平台級別實現單一登入 （SSO）。

應用程式負責使用Adobe Pass系統外部的特定標識服務或資料庫裝置檢索唯一平台標識碼有效負載。

應用程式負責將此唯一平臺標識碼有效負載作為指定它的所有請求的 / `X-Roku-Reserved-Roku-Connect-Token` 標頭的一部分`Adobe-Subject-Token`。

如需`Adobe-Subject-Token` / `X-Roku-Reserved-Roku-Connect-Token`標頭的詳細資訊，請參閱[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)檔案。

>[!MORELIKETHIS]
> 
> * [Amazon SSO逐步指南](/help/authentication/integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
> * [Roku SSO逐步指南](/help/authentication/integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)

## 使用平台身分識別透過單一登入執行驗證 {#perform-authentication-through-single-sign-on-using-platform-identity}

### 先決條件 {#prerequisites-perform-authentication-through-single-sign-on-using-platform-identity}

在使用平台身分識別透過單一登入執行驗證流程之前，請確定符合以下先決條件：

* 平台必須提供識別服務或資料庫，此服務或資料庫會傳回相同裝置或平台上所有應用程式上一致的`JWS`或`JWE`裝載資訊。
* 第一個串流應用程式必須擷取唯一的平台識別碼，並包含`JWS`或`JWE`裝載，做為所有指定該識別碼的要求之[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)標頭的一部分。
* 第一個串流應用程式必須選取MVPD。
* 第一個串流應用程式必須起始驗證工作階段，才能使用選取的MVPD登入。
* 第一個串流應用程式必須在使用者代理程式中使用選取的MVPD進行驗證。
* 第二個串流應用程式必須擷取唯一的平台識別碼，並包含`JWS`或`JWE`裝載，做為所有指定該識別碼的要求之[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)標頭的一部分。

>[!IMPORTANT]
>
> 假設
>
> <br/>
> 
> * 第一個串流應用程式支援使用者互動以選取MVPD。
> * 第一個串流應用程式支援使用者互動，以在使用者代理程式中使用選取的MVPD進行驗證。

### 工作流程 {#workflow-perform-authentication-through-single-sign-on-using-platform-identity}

執行以下指定步驟，使用平台身分識別透過單一登入實作驗證流程，如下圖所示。

![使用平台識別透過單一登入執行驗證](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-platform-identity-flow.png)

*使用平台識別透過單一登入執行驗證*

1. **擷取平台識別碼：**&#x200B;第一個串流應用程式會呼叫Adobe Pass系統之外的識別服務或程式庫，以取得與唯一平台識別碼相關聯的`JWS`或`JWE`裝載。

1. **傳回唯一平台識別碼為JWS或JWE：**&#x200B;第一個串流應用程式會驗證回應資料，以確保符合基本安全性條件：
   * 承載未過期。
   * 承載已簽署或加密。

1. **建立驗證工作階段：**&#x200B;第一個串流應用程式會收集所有必要的資料，藉由呼叫「工作階段」端點來啟動驗證工作階段。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱[建立驗證工作階段](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API檔案：
   > 
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`、`mvpd`、`domainName`和`redirectUrl`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭
   >
   > <br/>
   > 
   > 串流應用程式在提出要求之前，必須確定其中包含唯一平台識別碼的有效值。
   >
   > <br/>
   > 
   > 如需`Adobe-Subject-Token` / `X-Roku-Reserved-Roku-Connect-Token`標頭的詳細資訊，請參閱[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)檔案。

1. **指示下一個動作：**&#x200B;工作階段端點回應包含必要的資料，可引導第一個串流應用程式瞭解下一個動作。

   >[!IMPORTANT]
   >
   > 如需工作階段回應中所提供資訊的詳細資訊，請參閱[建立驗證工作階段](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API檔案。
   > 
   > <br/>
   > 
   > 「工作階段」端點會驗證請求資料，以確保符合基本條件：
   >
   > * 必需&#x200B;_的_&#x200B;參數和標頭必須有效。
   > * 所提供的 `serviceProvider` 和 `mvpd` 之間的整合必須處於活動狀態。
   >
   > <br/>
   > 
   > 如果驗證失敗，將生成錯誤回應，提供符合增強錯誤規範](../../../../features-standard/error-reporting/enhanced-error-codes.md)文檔的其他[信息。

1. **在用戶代理中打開URL：** 會話終結點回應包含以下數據：
   * 可用于 `url` 在 MVPD 登入 頁面中啟動交互式身份驗證。
   * `actionName`屬性已設定為「驗證」。
   * `actionType`屬性設定為「互動式」。

   如果Adobe Pass後端未識別有效的設定檔，第一個串流應用程式會開啟使用者代理程式，以載入提供的`url`，並向驗證端點提出要求。 此流程可能包含數個重新導向，最終將使用者引導至MVPD登入頁面並提供有效認證。

1. **完成MVPD驗證：**&#x200B;如果驗證流程成功，使用者代理程式互動會在Adobe Pass後端儲存一般設定檔，並到達提供的`redirectUrl`。

1. **擷取特定程式碼的設定檔：**&#x200B;第一個串流應用程式會傳送要求至設定檔端點，以收集擷取設定檔資訊所需的所有資料。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱特定程式碼](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API檔案的[擷取設定檔：
   > 
   > * 所有&#x200B;_必要的_&#x200B;引數，如`serviceProvider`、`code`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭

   >[!TIP]
   >
   > 建議：串流應用程式可以等待使用者代理程式到達提供的`redirectUrl`，以檢查一般設定檔是否已成功產生並儲存。

1. **尋找一般設定檔：** Adobe Pass伺服器會根據收到的引數和標頭識別有效的設定檔。

1. **返回有關常規設定檔的信息：** 配置文件終結點回應包含有關找到的與收到的參數和標頭關聯的設定檔的信息。

   >[!IMPORTANT]
   >
   > 有關設定檔回應中提供的資訊的詳細信息，請参閱 [特定代碼](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API 的檢索設定檔文檔。
   > 
   > <br/>
   > 
   > 設定文件端點驗證請求數據，以確保滿足基本條件：
   >
   > * 必需&#x200B;_的_&#x200B;參數和標頭必須有效。
   >
   > <br/>
   > 
   > 如果驗證失敗，將生成錯誤回應，提供符合增強錯誤規範](../../../../features-standard/error-reporting/enhanced-error-codes.md)文檔的其他[信息。

1. **繼續決策流程：**&#x200B;第一個串流應用程式可以繼續後續的決策流程。

   >[!IMPORTANT]
   >
   > 串流應用程式在提出要求之前，必須確定其中包含唯一平台識別碼的有效值。
   >
   > <br/>
   > 
   > 如需`Adobe-Subject-Token` / `X-Roku-Reserved-Roku-Connect-Token`標頭的詳細資訊，請參閱[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)檔案。

1. **檢索平台標識碼：** 第二個流式處理應用程式調用Adobe Pass系統外部的身份服務或資料庫，以獲取 `JWS` 與唯一平臺標識碼關聯的 or `JWE` 有效負載。

1. **返回唯一平台標識碼為 JWS 或 JWE：** 第二個流式處理應用程式驗證響應數據，以確保滿足基本安全條件：
   * 有效負載未過期。
   * 承載已簽署或加密。

1. **擷取設定檔：**&#x200B;第二個串流應用程式會收集所有必要資料，藉由傳送要求給設定檔端點來擷取所有設定檔資訊。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱[擷取設定檔](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API檔案：
   > 
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭
   >
   > <br/>
   > 
   > 串流應用程式在提出要求之前，必須確定其中包含唯一平台識別碼的有效值。
   >
   > <br/>
   > 
   > 如需`Adobe-Subject-Token` / `X-Roku-Reserved-Roku-Connect-Token`標頭的詳細資訊，請參閱[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)檔案。

1. **尋找單一登入設定檔：** Adobe Pass伺服器會根據收到的引數和標題，識別有效的單一登入設定檔。

1. **傳回有關單一登入設定檔的資訊：**&#x200B;設定檔端點回應包含有關所找到之設定檔的資訊，此設定檔與收到的引數和標題相關聯。

   >[!IMPORTANT]
   >
   > 如需設定檔回應中所提供資訊的詳細資訊，請參閱[擷取設定檔](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) API檔案。
   >
   > <br/>
   > 
   > 設定檔端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   >
   > <br/>
   > 
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的額外資訊。

1. **繼續決策流程：**&#x200B;第二個串流應用程式可以繼續後續的決策流程。

   >[!IMPORTANT]
   >
   > 串流應用程式在提出要求之前，必須確定其中包含唯一平台識別碼的有效值。
   >
   > <br/>
   > 
   > 如需`Adobe-Subject-Token` / `X-Roku-Reserved-Roku-Connect-Token`標頭的詳細資訊，請參閱[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)檔案。

## 使用平台身分識別透過單一登入擷取授權決策{#performing-authorization-flow-using-platform-identity-single-sign-on-method}

### 先決條件 {#prerequisites-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

在使用平台身分識別透過單一登入執行授權流程之前，請確保符合以下先決條件：

* 平台必須提供識別服務或資料庫，此服務或資料庫會傳回相同裝置或平台上所有應用程式上一致的`JWS`或`JWE`裝載資訊。
* 第二個串流應用程式必須擷取唯一的平台識別碼，並包含`JWS`或`JWE`裝載，做為所有指定該識別碼的要求之[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)標頭的一部分。
* 第二個串流應用程式必須先擷取授權決定，才能播放使用者選取的資源。

>[!IMPORTANT]
>
> 假設
> 
> <br/>
> 
> * 第一個串流應用程式已執行驗證，並包含[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)要求標頭的有效值。

### 工作流程 {#workflow-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

執行以下指定步驟，使用平台身分透過單一登入實作授權流程，如下圖所示。

![使用平台識別透過單一登入擷取授權決定](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-platform-identity-flow.png)

*使用平台識別透過單一登入擷取授權決定*

1. **擷取平台識別碼：**&#x200B;第二個串流應用程式會呼叫Adobe Pass系統之外的識別服務或程式庫，以取得與唯一平台識別碼相關聯的`JWS`或`JWE`裝載。

1. **傳回唯一平台識別碼為JWS或JWE：**&#x200B;第二個串流應用程式會驗證回應資料，以確保符合基本安全性條件：
   * 承載未過期。
   * 承載已簽署或加密。

1. **擷取授權決定：**&#x200B;第二個串流應用程式會呼叫Decisions Authorized端點，收集所有必要資料以取得特定資源的授權決定。

   >[!IMPORTANT]
   >
   > 請參考[使用特定mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API擷取授權決定，以取得以下詳細資訊：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`、`mvpd`和`resources`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`和`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭
   >
   > <br/>
   > 
   > 串流應用程式在提出要求之前，必須確定其中包含唯一平台識別碼的有效值。
   >
   > <br/>
   > 
   > 如需`Adobe-Subject-Token` / `X-Roku-Reserved-Roku-Connect-Token`標頭的詳細資訊，請參閱[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) / [X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)檔案。

1. **尋找單一登入設定檔：** Adobe Pass伺服器會根據收到的引數和標題，識別有效的單一登入設定檔。

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

1. **使用媒體權杖開始串流：**&#x200B;第二個串流應用程式使用媒體權杖播放內容。

1. **傳回`Deny`決定，詳細資料：** Decisions Authorize端點回應包含`Deny`決定和依循[增強式錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的錯誤承載。

   >[!IMPORTANT]
   >
   > 有關決策回應中提供的資訊的詳細資訊，請參閱 [使用特定 mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) API 檢索授權決策文件。
   > 
   > <br/>
   > 
   > 決策授權端點驗證請求數據，以確保滿足基本條件：
   >
   > * 必需&#x200B;_的_&#x200B;參數和標頭必須有效。
   > * 所提供的 `serviceProvider` 和 `mvpd` 之間的整合必須處於活動狀態。
   >
   > <br/>
   > 
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的額外資訊。

1. **處理`Deny`決定詳細資料：**&#x200B;第二個串流應用程式會處理回應中的錯誤資訊，並可使用它來選擇性地在使用者介面上顯示特定訊息。

>[!NOTE]
>
> 預先授權流程的步驟與授權流程的步驟相同，但使用的端點是[使用特定mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)檔案擷取預先授權決定中描述的端點。
