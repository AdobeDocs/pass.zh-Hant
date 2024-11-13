---
title: Apple SSO逐步指南(REST API V2)
description: Apple SSO逐步指南(REST API V2)
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
source-git-commit: e5ef8c0cba636ac4d2bda1abe0e121d0ecc1b795
workflow-type: tm+mt
source-wordcount: '3410'
ht-degree: 0%

---

# Apple SSO逐步指南(REST API V2) {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

Adobe Pass Authentication REST API V2支援在iOS、iPadOS或tvOS上執行之使用者端應用程式的一般使用者進行合作夥伴單一登入(SSO)。

此檔案可作為現有[REST API V2總覽](/help/authentication/rest-api-v2/rest-api-v2-overview.md)的延伸，提供高階檢視和說明如何使用合作夥伴流程](/help/authentication/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)實作[單一登入的檔案。

## 使用合作夥伴流程的Apple單一登入 {#cookbook}

### 先決條件 {#prerequisites}

繼續使用合作夥伴流程進行Apple單一登入之前，請確定符合下列先決條件：

* 串流應用程式必須收集`X-Device-Info`和/或`User-Agent`標題所需的所有必要資料，讓Adobe Pass驗證後端可以識別裝置平台及其功能。 如需`X-Device-Info`標頭的詳細資訊，請參閱[X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)檔案。

* 串流應用程式必須請求存取儲存在裝置層級的使用者訂閱資訊，使用者必須授予應用程式繼續的許可權，類似於提供裝置攝影機或麥克風的存取權。 必須使用Apple的[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)為每個應用程式要求此許可權，裝置將會儲存使用者的選擇。

  我們建議您說明Apple單一登入使用者體驗的優點，以鼓勵拒絕授予許可權存取訂閱資訊的使用者，但請注意，使用者可以移至應用程式設定（電視提供者許可權存取）、iOS和iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;來變更其決定。

  當應用程式進入前景狀態時，串流應用程式可以要求使用者的許可權，因為應用程式可以在要求使用者驗證之前，隨時檢查[存取使用者訂閱資訊的許可權](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)。

>[!IMPORTANT]
>
> 假設
>
> <br/>
>
> * 串流應用程式已完成適用於程式設計師的[上線必要條件](/help/authentication/single-sign-on/partner-single-sign-on/apple-single-sign-on/apple-sso-overview.md#apple-sso-prerequisites-programmer)，且為啟用Apple單一登入使用者體驗所需。

### 工作流程 {#workflow}

執行指定的步驟，使用合作夥伴流程來實作Apple單一登入，如下圖所示。

使用合作夥伴流程![Apple單一登入](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

使用合作夥伴流程&#x200B;*Apple單一登入*

+++答：註冊階段

1. **擷取使用者端認證：**&#x200B;串流應用程式會呼叫使用者端登入端點，以收集擷取使用者端認證所需的所有資料。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱[擷取使用者端認證](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) API檔案：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`software_statement`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Content-Type`、`X-Device-Info`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭

1. **傳回使用者端認證：**&#x200B;使用者端登入端點回應包含與所接收引數和標頭相關聯的使用者端認證的相關資訊。

   >[!IMPORTANT]
   >
   > 請參閱[擷取使用者端認證](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) API檔案，以取得使用者端認證回應中提供的詳細資訊。
   >
   > <br/>
   >
   > Client Register會驗證要求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   >
   > <br/>
   >
   > 如果驗證失敗，將會產生錯誤回應，提供遵守[擷取使用者端認證](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) API檔案的額外資訊。

   >[!TIP]
   >
   > 建議：使用者端認證必須快取，並可無限期使用。

1. **擷取存取Token：**&#x200B;串流應用程式會呼叫使用者端權杖端點，收集擷取存取Token所需的所有資料。

   >[!IMPORTANT]
   >
   > 請參閱[擷取存取Token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md#request) API檔案，以取得下列詳細資訊：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`client_id`、`client_secret`和`grant_type`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Content-Type`、`X-Device-Info`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭

1. **傳回存取權杖：**&#x200B;使用者端權杖端點回應包含與收到的引數和標頭關聯的存取權杖相關資訊。

   >[!IMPORTANT]
   >
   > 請參閱[擷取存取Token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) API檔案，以取得存取Token回應中提供的詳細資訊。
   >
   > <br/>
   >
   > 使用者端權杖會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   >
   > <br/>
   >
   > 如果驗證失敗，將會產生錯誤回應，提供遵守[擷取存取Token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md#error) API檔案的額外資訊。

   >[!TIP]
   >
   > 建議：存取權杖只能在指定的期間內快取和使用（例如24小時存留時間）。 到期後，串流應用程式必須要求新的存取權杖。

+++

+++B.檢查驗證階段

1. **擷取合作夥伴架構狀態：**&#x200B;串流應用程式會呼叫Apple開發的[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)，以取得使用者許可權和提供者資訊。

   >[!IMPORTANT]
   >
   > 請參閱[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)檔案，以取得下列詳細資訊：
   >
   > <br/>
   >
   > * 串流應用程式必須檢查是否有[許可權可存取](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)使用者的訂閱資訊，並僅在使用者允許的情況下才繼續。
   > * 串流應用程式必須為`VSAccountManager`提供[代理人](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)。
   > * 串流應用程式必須提交[要求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)以取得訂閱者帳戶資訊。
   > * 串流應用程式必須等候並處理[中繼資料](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)資訊。
   >
   > <br/>
   >
   > 串流應用程式必須確定它為`VSAccountMetadataRequest`物件中的[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)屬性指定了等於`false`的Boolean值，以表示在此階段無法中斷使用者。

1. **傳回夥伴架構狀態資訊：**&#x200B;串流應用程式會驗證回應資料，以確保符合基本條件：
   * 已授予使用者許可權存取狀態。
   * 使用者提供者對應識別碼存在且有效。
   * 使用者提供者設定檔的到期日（如果有的話）有效。

1. **擷取設定檔：**&#x200B;串流應用程式會收集所有必要資料，藉由傳送要求給設定檔端點來擷取所有設定檔資訊。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱[擷取設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) API檔案：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`和`AP-Partner-Framework-Status`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭
   >
   > <br/>
   >
   > 串流應用程式必須確定其包含合作夥伴架構狀態的有效值，以便擷取的回應可包含「appleSSO」型別設定檔。
   >
   > <br/>
   >
   > 如需`AP-Partner-Framework-Status`標頭的詳細資訊，請參閱[AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)檔案。

1. **傳回有關已找到設定檔的資訊：**&#x200B;設定檔端點回應包含有關已找到與已接收引數和標題相關聯的設定檔的資訊。

1. **選擇設定檔並繼續決策流程：**&#x200B;如果「設定檔」端點回應包含設定檔，串流應用程式會使用其內部邏輯（最終透過與一般使用者互動）來選擇其中一個可用的設定檔，以繼續後續的決策流程。

1. **繼續協力驗證流程：**&#x200B;如果Profiles端點回應不包含設定檔，串流應用程式會繼續協力驗證流程。

+++

+++C.合作夥伴驗證階段

1. **擷取組態：**&#x200B;串流應用程式會收集所有必要的資料，藉由傳送要求給組態端點，以擷取具有作用中整合的MVPD清單。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱特定服務提供者](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request) API的[擷取組態：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`和`X-Device-Info`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭

1. **傳回組態：**&#x200B;組態端點回應包含與服務提供者有效整合之MVPD的相關資訊。

   >[!IMPORTANT]
   >
   > 請參閱特定服務提供者](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response) API檔案的[擷取組態，以取得組態回應中提供的詳細資訊。
   >
   > <br/>
   >
   > 設定端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   >
   > <br/>
   >
   > 如果驗證失敗，將會產生錯誤回應，提供遵守[增強錯誤碼](/help/authentication/enhanced-error-codes.md)的其他資訊

   >[!IMPORTANT]
   >
   > 串流應用程式在繼續下一步作業時，必須確定已處理每個MVPD提供的下列詳細資料：
   >
   > * `enablePlatformServices`：指出MVPD目前是否支援Apple單一登入。
   > * `displayInPlatformPicker`：指出MVPD是否可以在Apple選擇器中顯示。
   > * `boardingStatus`：指出MVPD是否已上線到Apple單一登入。

1. **擷取合作夥伴架構狀態：**&#x200B;串流應用程式會呼叫Apple開發的[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)，以取得使用者許可權和提供者資訊。

   >[!IMPORTANT]
   >
   > 請參閱[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)檔案，以取得下列詳細資訊：
   >
   > <br/>
   >
   > * 串流應用程式必須檢查是否有[許可權可存取](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)使用者的訂閱資訊，並僅在使用者允許的情況下才繼續。
   > * 串流應用程式必須為`VSAccountManager`提供[代理人](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)。
   > * 串流應用程式必須提交[要求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)以取得訂閱者帳戶資訊。
   > * 串流應用程式必須等候並處理[中繼資料](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)資訊。
   >
   > <br/>
   >
   > 串流應用程式必須確定它為`VSAccountMetadataRequest`物件中的[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)屬性指定了等於`true`的Boolean值，以表示在此階段可以中斷使用者選取電視提供者。

1. **傳回夥伴架構狀態資訊：**&#x200B;串流應用程式會驗證回應資料，以確保符合基本條件：
   * 已授予使用者許可權存取狀態。
   * 使用者提供者對應識別碼存在且有效。
   * 使用者提供者設定檔的到期日（如果有的話）有效。

1. **擷取合作夥伴驗證要求：**&#x200B;串流應用程式會收集所有必要的資料，藉由呼叫工作階段合作夥伴端點來啟動驗證工作階段。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱[擷取合作夥伴驗證要求](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request) API檔案：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`和`partner`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`、`Content-Type`、`X-Device-Info`和`AP-Partner-Framework-Status`
   > * 所有&#x200B;_選用的_&#x200B;標頭和引數
   >
   > <br/>
   >
   > 串流應用程式必須確保其包含合作夥伴框架狀態的有效值，以便擷取的回應可能包含合作夥伴驗證請求（SAML請求）。
   >
   > <br/>
   >
   > 如需`AP-Partner-Framework-Status`標頭的詳細資訊，請參閱[AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)檔案。

1. **指示下一個動作：**&#x200B;工作階段合作夥伴端點回應包含必要的資料，可引導串流應用程式瞭解下一個動作。

   >[!IMPORTANT]
   >
   > 請參閱[擷取合作夥伴驗證要求](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response) API檔案，以取得工作階段回應中提供的詳細資訊。
   >
   > <br/>
   >
   > 工作階段合作夥伴端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   > * 提供的`serviceProvider`與`mvpd`之間的整合必須是作用中。
   >
   > <br/>
   >
   > 如果基本驗證失敗，將會產生錯誤回應，提供遵守[增強型錯誤碼](/help/authentication/enhanced-error-codes.md)檔案的額外資訊。
   >
   > <br/>
   >
   > 「工作階段」合作夥伴端點會驗證請求資料，以確保符合合作夥伴單一登入條件：
   >
   >  * Adobe Pass伺服器中的合作夥伴單一登入設定必須有效且已啟用。
   >  * 透過[AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)標頭收到的合作夥伴架構狀態承載必須有效。
   >
   > <br/>
   >
   > 如果合作夥伴單一登入驗證失敗，回應將預設為基本驗證流程。

1. **繼續決策流程：**&#x200B;工作階段夥伴端點回應包含下列資料：
   * `actionName`屬性已設定為「授權」。
   * `actionType`屬性設定為「直接」。

   如果Adobe Pass後端識別有效的設定檔，串流應用程式就不需要使用選取的MVPD重新驗證，因為已經有設定檔可用於後續的決策流程。

1. **繼續基本驗證流程：**&#x200B;工作階段夥伴端點回應包含下列資料：
   * `actionName`屬性已設定為「驗證」或「繼續」。
   * `actionType`屬性設定為「互動」或「直接」。

   如果Adobe Pass後端未識別有效的設定檔，且合作夥伴單一登入驗證失敗，則Adobe Pass伺服器會回覆為基本驗證流程。

   如需基本驗證流程的詳細資訊，請參閱以下檔案：
   * [在主要應用程式內執行驗證](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [使用預先選取的mvpd在次要應用程式內執行驗證](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [在次要應用程式內執行驗證，而不預先選取mvpd](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **繼續使用合作夥伴驗證回應流程擷取設定檔：**&#x200B;工作階段合作夥伴端點回應包含下列資料：
   * `actionName`屬性設定為&quot;partner_profile&quot;。
   * `actionType`屬性設定為「直接」。
   * `authenticationRequest - type`屬性包含合作夥伴架構用於MVPD登入的安全性通訊協定（目前僅設定為SAML）。
   * `authenticationRequest - request`屬性包含傳遞至合作夥伴架構的SAML要求。
   * `authenticationRequest - attributesNames`屬性包含傳遞至合作夥伴架構的SAML屬性。

   如果Adobe Pass後端未識別有效的設定檔，且合作夥伴單一登入驗證通過時，串流應用程式會收到包含動作和資料的回應，並傳遞至合作夥伴架構，以使用MVPD啟動驗證流程。

1. **使用合作夥伴架構完成MVPD驗證：**&#x200B;將先前步驟中取得的合作夥伴驗證要求（SAML要求）轉送至[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)。 如果驗證流程成功，與MVPD的[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)互動會產生合作夥伴驗證回應（SAML回應），此回應會連同合作夥伴架構狀態資訊一併傳回。

   >[!IMPORTANT]
   >
   > 請參閱[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)檔案，以取得下列詳細資訊：
   >
   > <br/>
   >
   > * 串流應用程式必須檢查是否有[許可權可存取](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)使用者的訂閱資訊，並僅在使用者允許的情況下才繼續。
   > * 串流應用程式必須為`VSAccountManager`提供[代理人](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)。
   > * 串流應用程式必須提交訂閱者帳戶資訊的[要求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)，而且必須包含先前步驟中取得的合作夥伴驗證要求（SAML要求）。
   > * 串流應用程式必須等候並處理[中繼資料](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)資訊。
   >
   > <br/>
   >
   > 串流應用程式必須確定它為`VSAccountMetadataRequest`物件中的[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)屬性指定了等於`true`的Boolean值，以表示在此階段可以中斷使用者，以向選取的電視提供者進行驗證。

1. **傳回夥伴驗證回應：**&#x200B;串流應用程式會驗證回應資料，以確保符合基本條件：
   * 已授予使用者許可權存取狀態。
   * 使用者提供者對應識別碼存在且有效。
   * 使用者提供者設定檔的到期日（如果有的話）有效。
   * 合作夥伴驗證回應（SAML回應）存在且有效。

1. **使用合作夥伴驗證回應擷取設定檔：**&#x200B;串流應用程式會收集所有必要的資料，藉由呼叫Profiles合作夥伴端點來建立和擷取設定檔。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱[使用合作夥伴驗證回應](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request) API檔案：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`、`partner`和`SAMLResponse`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`、`Content-Type`、`X-Device-Info`和`AP-Partner-Framework-Status`
   > * 所有&#x200B;_選用的_&#x200B;標頭和引數
   >
   > <br/>
   >
   > 串流應用程式必須確定其包含合作夥伴架構狀態和合作夥伴驗證回應（SAML回應）的有效值，以使擷取的回應可能包含「appleSSO」型別設定檔。
   >
   > <br/>
   >
   > 如需`AP-Partner-Framework-Status`標頭的詳細資訊，請參閱[AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)檔案。

1. **傳回夥伴設定檔的相關資訊：**&#x200B;設定檔端點回應包含夥伴設定檔的相關資訊，包括設定為「appleSSO」的屬性`type`。

   >[!IMPORTANT]
   >
   > 請參閱[使用合作夥伴驗證回應](/help/authentication/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response) API檔案，以取得有關設定檔回應中所提供資訊的詳細資訊。
   >
   > <br/>
   >
   > 設定檔合作夥伴端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   > * 提供的`serviceProvider`與`mvpd`之間的整合必須是作用中。
   >
   > <br/>
   >
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](/help/authentication/enhanced-error-codes.md)檔案的額外資訊。
   >
   > <br/>
   >
   > 設定檔合作夥伴端點會驗證請求資料，以確保符合合作夥伴單一登入條件：
   >
   >  * Adobe Pass伺服器中的合作夥伴單一登入設定必須有效且已啟用。
   >  * 透過[AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)標頭收到的合作夥伴架構狀態承載必須有效。
   >
   > <br/>
   >
   > 如果合作夥伴單一登入驗證失敗，回應將預設為基本設定檔擷取流程。

1. **繼續決策流程：**&#x200B;串流應用程式可以繼續後續的決策流程。

+++

+++ D.決定階段

1. **擷取合作夥伴架構狀態：**&#x200B;串流應用程式會呼叫Apple開發的[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)，以取得使用者許可權和提供者資訊。

   >[!IMPORTANT]
   >
   > 請參閱[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)檔案，以取得下列詳細資訊：
   >
   > <br/>
   >
   > * 串流應用程式必須檢查是否有[許可權可存取](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)使用者的訂閱資訊，並僅在使用者允許的情況下才繼續。
   > * 串流應用程式必須為`VSAccountManager`提供[代理人](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)。
   > * 串流應用程式必須提交[要求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)以取得訂閱者帳戶資訊。
   > * 串流應用程式必須等候並處理[中繼資料](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)資訊。
   >
   > <br/>
   >
   > 串流應用程式必須確定它為`VSAccountMetadataRequest`物件中的[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)屬性指定了等於`false`的Boolean值，以表示在此階段無法中斷使用者。

   >[!TIP]
   > 
   > 建議：串流應用程式可以使用快取值作為合作夥伴架構狀態資訊，建議在應用程式從背景轉換為前景狀態時重新整理。

1. **傳回夥伴架構狀態資訊：**&#x200B;串流應用程式會驗證回應資料，以確保符合基本條件：
   * 已授予使用者許可權存取狀態。
   * 使用者提供者對應識別碼存在且有效。
   * 使用者提供者設定檔的到期日（如果有的話）有效。

1. **擷取預先授權決定：**&#x200B;串流應用程式會呼叫Decisions Preauthorize端點，收集所有必要的資料，以取得資源清單的預先授權決定。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱使用特定mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) API檔案擷取預先授權決定：[
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`、`mvpd`和`resources`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`和`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭
   >
   > <br/>
   >
   > 當選擇的設定檔是「appleSSO」型別設定檔時，串流應用程式在進一步提出請求之前，必須確定它包含合作夥伴框架狀態的有效值。
   >
   > <br/>
   >
   > 如需`AP-Partner-Framework-Status`標頭的詳細資訊，請參閱[AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)檔案。

1. **傳回預先授權決定：**&#x200B;決定預先授權端點回應包含每個資源的`Permit`或`Deny`決定：
   * `Permit`決定表示資源可供播放。 回應不包含媒體Token，因為預先授權流程不能用於播放資源。
   * `Deny`決定表示資源無法播放。 回應包含附在[增強錯誤碼](/help/authentication/enhanced-error-codes.md)檔案的錯誤承載。

   >[!IMPORTANT]
   >
   > 請參閱使用特定mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) API檔案的[擷取預先授權決定，以取得決定回應中提供的詳細資訊。
   >
   > <br/>
   >
   > 決定預先授權端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   > * 提供的`serviceProvider`與`mvpd`之間的整合必須是作用中。
   >
   > <br/>
   >
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](/help/authentication/enhanced-error-codes.md)檔案的額外資訊。

1. **擷取合作夥伴架構狀態：**&#x200B;串流應用程式會呼叫Apple開發的[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)，以取得使用者許可權和提供者資訊。

   >[!IMPORTANT]
   >
   > 請參閱[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)檔案，以取得下列詳細資訊：
   >
   > <br/>
   >
   > * 串流應用程式必須檢查是否有[許可權可存取](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)使用者的訂閱資訊，並僅在使用者允許的情況下才繼續。
   > * 串流應用程式必須為`VSAccountManager`提供[代理人](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate)。
   > * 串流應用程式必須提交[要求](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest)以取得訂閱者帳戶資訊。
   > * 串流應用程式必須等候並處理[中繼資料](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata)資訊。
   >
   > <br/>
   >
   > 串流應用程式必須確定它為`VSAccountMetadataRequest`物件中的[`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed)屬性指定了等於`false`的Boolean值，以表示在此階段無法中斷使用者。

   >[!TIP]
   >
   > 建議：串流應用程式可以使用快取值作為合作夥伴架構狀態資訊，建議在應用程式從背景轉換為前景狀態時重新整理。

1. **傳回夥伴架構狀態資訊：**&#x200B;串流應用程式會驗證回應資料，以確保符合基本條件：
   * 已授予使用者許可權存取狀態。
   * 使用者提供者對應識別碼存在且有效。
   * 使用者提供者設定檔的到期日（如果有的話）有效。

1. **擷取授權決定：**&#x200B;串流應用程式會呼叫Decisions Authorized端點，收集所有必要資料以取得特定資源的授權決定。

   >[!IMPORTANT]
   >
   > 請參考[使用特定mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request) API擷取授權決定，以取得以下詳細資訊：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`、`mvpd`和`resources`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`和`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭
   >
   > <br/>
   >
   > 當選擇的設定檔是「appleSSO」型別設定檔時，串流應用程式在進一步提出請求之前，必須確定它包含合作夥伴框架狀態的有效值。
   >
   > <br/>
   >
   > 如需`AP-Partner-Framework-Status`標頭的詳細資訊，請參閱[AP-Partner-Framework-Status](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)檔案。

1. **傳回授權決定：**&#x200B;決定授權端點回應包含特定資源的`Permit`或`Deny`決定：
   * `Permit`決定表示資源可供播放。 回應包含媒體權杖。
   * `Deny`決定表示資源無法播放。 回應包含附在[增強錯誤碼](/help/authentication/enhanced-error-codes.md)檔案的錯誤承載。

   >[!IMPORTANT]
   >
   > 請參閱使用特定mvpd](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) API檔案的[擷取授權決定，以取得決定回應中提供的詳細資訊。
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
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](/help/authentication/enhanced-error-codes.md)檔案的額外資訊。

+++

+++ D.登出階段

1. **啟動Adobe Pass登出：**&#x200B;串流應用程式會呼叫Adobe Pass登出端點，收集所有必要的資料以啟動登出流程。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱特定mvpd](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request) API的[起始登出：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`、`mvpd`和`redirectUrl`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭

1. **指示下一個動作：** Adobe Pass登出端點回應包含必要的資料，可引導串流應用程式瞭解下一個動作。

   >[!IMPORTANT]
   >
   > 如需登出回應中提供的詳細資訊，請參閱特定mvpd](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response) API的[Initiate登出。
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
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](/help/authentication/enhanced-error-codes.md)檔案的額外資訊。

   >[!IMPORTANT]
   > 
   > 串流應用程式必須確定它表示使用者可以繼續從合作夥伴層級登出。

+++
