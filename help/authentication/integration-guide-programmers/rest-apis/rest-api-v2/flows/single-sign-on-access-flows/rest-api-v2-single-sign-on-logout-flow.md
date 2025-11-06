---
title: 單一登出 — 流量
description: REST API V2 — 單一登出 — 流量
exl-id: d7092ca7-ea7b-4e92-b45f-e373a6d673d6
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# 單一登出流程 {#single-logout-flow}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> REST API V2實作受到[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)檔案的限制。

>[!MORELIKETHIS]
>
> 請確定也造訪[REST API V2常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)。

## 針對特定mvpd啟動單一登出 {#initiate-single-logout-for-specific-mvpd}

### 先決條件 {#prerequisites-initiate-single-logout-for-specific-mvpd}

在起始特定MVPD的單一登出之前，請確定符合下列先決條件：

* 第二個串流應用程式必須具備有效的單一登入設定檔，且已使用其中一個單一登入驗證流程成功為MVPD建立：
   * [使用平台身分識別透過單一登入執行驗證](rest-api-v2-single-sign-on-platform-identity-flows.md)
   * [使用服務權杖，透過單一登入執行驗證](rest-api-v2-single-sign-on-service-token-flows.md)
* 第二個串流應用程式在需要登出MVPD時，必須起始單一登出流程。

>[!IMPORTANT]
> 
> 假設
>
> <br/>
> 
> * 第一和第二串流應用程式會取得與`JWS`或`JWE`相同的唯一平台識別碼裝載，或與`JWS`相同的唯一使用者識別碼裝載。

### 工作流程 {#workflow-initiate-single-logout-for-specific-mvpd}

執行以下指定步驟，為特定MVPD實施單一登出流程，如下圖所示。

![啟動特定mvpd的單一登出](../../../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-initiate-single-logout-for-specific-mvpd-flow.png)

*啟動特定mvpd的單一登出*

1. **啟動Adobe Pass登出：**&#x200B;串流應用程式會呼叫Adobe Pass登出端點，收集所有必要的資料以啟動登出流程。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱特定mvpd[&#x200B; API的](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)起始登出：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`、`mvpd`和`redirectUrl`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭
   >
   > <br/>
   >
   > 串流應用程式在提出請求之前，必須確定其中包含唯一平台識別碼或唯一使用者識別碼的有效值。
   >
   > <br/>
   > 
   > 如需`Adobe-Subject-Token`標頭的詳細資訊，請參閱[Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)檔案。
   > 
   > <br/>
   > 
   > 如需`AD-Service-Token`標頭的詳細資訊，請參閱[AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)檔案。

1. **尋找一般和單一登入設定檔：** Adobe Pass伺服器會根據收到的引數和標題，識別一般和單一登入的有效設定檔。

1. **刪除一般和單一登入設定檔：** Adobe Pass伺服器會從Adobe Pass後端刪除已識別的一般和單一登入設定檔。

1. **指示下一個動作：** Adobe Pass登出端點回應包含必要的資料，可引導串流應用程式瞭解下一個動作。

   >[!IMPORTANT]
   >
   > 如需登出回應中提供的詳細資訊，請參閱特定mvpd[&#x200B; API的](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)Initiate登出。
   > 
   > <br/>
   > 
   > Adobe Pass登出端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   > * 提供的`serviceProvider`與`mvpd`之間的整合必須是作用中。
   >
   > <br/>
   > 
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的額外資訊。

1. **表示登出完成：**&#x200B;如果MVPD不支援登出流程，串流應用程式會處理回應，並可以使用它選擇性地在使用者介面上顯示特定訊息。

1. **啟動MVPD登出：**&#x200B;如果MVPD不支援登出流程，串流應用程式會處理回應，並使用使用者代理程式來啟動與MVPD的登出流程。 此流程可能包含數個重新導向至MVPD系統的動作。 然而，結果是MVPD會執行內部清理，並將最終登出確認傳送回Adobe Pass後端。

1. **表示登出完成：**&#x200B;串流應用程式可以等待使用者代理程式到達提供的`redirectUrl`，並且可以使用它作為訊號，以選擇在使用者介面上顯示特定訊息。

>[!NOTE]
>
> 如果是從第一個串流應用程式啟動，單一登出流程的步驟將與上述步驟相同。
