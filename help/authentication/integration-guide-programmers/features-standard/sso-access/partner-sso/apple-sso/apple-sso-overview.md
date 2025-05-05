---
title: Apple SSO概觀
description: Apple SSO概觀
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1260'
ht-degree: 0%

---

# Apple SSO概觀 {#apple-sso-overview}

>[!IMPORTANT]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

Apple讓使用者能在裝置系統層級登入其電視提供者帳戶，而不需依應用程式逐一驗證。

Adobe Pass驗證與Apple合作，為iPhone、iPad和Apple電視擁有者在電視各處生態系統建立合作夥伴單一登入(SSO)使用者體驗。

若要在Apple裝置上享有單一登入(SSO)使用者體驗，您必須完成以下列出的必要條件。

最終結果應會建立符合下列使用者流程的體驗，建議您先諮詢後再開始開發應用程式：

* iPhone和iPad[&#128279;](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf)裝置的單一登入(SSO) 使用者流程。
* Apple TV[&#128279;](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf)裝置的單一登入(SSO) 使用者流程。

## 先決條件 {#apple-sso-prerequisites}

上線的先決條件可能適用於一或多個參與TVE業務的實體，例如程式設計師、MVPD、Adobe Pass驗證或Apple。

### 程式設計師 {#apple-sso-prerequisites-programmer}

為了受益於單一登入(SSO)使用者體驗，一位程式設計師必須：

* 請連絡Apple以啟用[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)，做為您Apple團隊ID的一部分，並設定[視訊訂閱者單一登入權利](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on)，做為您Apple開發人員帳戶的一部分。

   * 使用Xcode 8版或更新版本，以及iOS/tvOS 10版或更新版本。

* 透過[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)，為每個想要的整合和平台(iOS/tvOS)啟用單一登入(SSO)，方法是將`Enable Single Sign On`屬性設定為`Yes`。

| Adobe啟用單一登入 | Apple **已上線（支援）**&#x200B;個MVPD | Apple **挑選器** MVPD | Apple **未上線（不支援）** MVPD |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| 是（已啟用） | 驗證和登出流程將同時涉及Apple和Adobe Pass驗證解決方案，而所有其他流程（授權、預先授權、中繼資料等）將僅由Adobe Pass驗證提供服務。 | 驗證和登出流程會回覆為僅由Adobe Pass驗證服務的一般流程。 | 驗證和登出流程會回覆為僅由Adobe Pass驗證服務的一般流程。 |
| 否（已停用） | 驗證和登出流程會回覆為僅由Adobe Pass驗證服務的一般流程。 | 驗證和登出流程會回覆為僅由Adobe Pass驗證服務的一般流程。 | 驗證和登出流程會回覆為僅由Adobe Pass驗證服務的一般流程。 |

* 使用Adobe Pass驗證提供的下列解決方案之一，針對在iOS、iPadOS或tvOS上執行的使用者端應用程式一般使用者，整合單一登入(SSO)使用者流程。

   * Adobe Pass Authentication REST API V2支援合作夥伴單一登入(SSO)。

     請參閱[Apple SSO逐步指南(REST API V2)](apple-sso-cookbook-rest-api-v2.md)檔案。

   * 舊版Adobe Pass驗證REST API V1支援合作夥伴單一登入(SSO)。

     請參閱[（舊版） Apple SSO逐步指南(REST API V1)](../../../../legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)檔案。

   * 舊版Adobe Pass Authentication AccessEnabler iOS/tvOS SDK支援合作夥伴單一登入(SSO)。

     請參閱[ （舊版） Apple SSO逐步指南(iOS/tvOS SDK)](../../../../legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md)檔案。

### MVPD {#apple-sso-prerequisites-mvpd}

若要受益於單一登入(SSO)使用者體驗，一個MVPD必須：

* 請聯絡Apple，以便在Apple一方開始入門流程。

   * 請求技術檔案，說明如何整合及開發可處理使用者登入表單的JavaScript TVML應用程式。

* 聯絡Adobe Pass驗證以在Adobe一方開始入門流程。

   * 提供字串值，代表Apple在上線流程中指派的電視提供者識別碼。

## 常見問題集 {#FAQ}

* 萬一Apple SSO工作流程發生問題，使用Adobe Pass Authentication AccessEnabler iOS/tvOS SDK的應用程式是否能夠回覆為一般驗證流程？

  這是可能的，但需要透過[Adobe Pass TVE儀表板](https://experience.adobe.com/#/pass/authentication)執行組態變更，以針對所需的整合和平台(iOS/tvOS)，在&#x200B;**NO**&#x200B;上設定&#x200B;**啟用單一登入**。 請注意，使用者端應用程式只有在呼叫[setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3) API之後，才會確認組態變更。


* 應用程式會知道透過Apple SSO登入而導致驗證發生的時間嗎？

  此資訊屬於使用者中繼資料金鑰： *tokenSource*&#x200B;的一部分，在此案例中應傳回字串值： &quot;Apple&quot;。


* 應用程式會知道透過Apple SSO在另一個應用程式上登入後何時發生驗證嗎？

  此資訊無法使用。


* 如果使用者使用未與應用程式整合的MVPD前往iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;區段登入，會發生什麼情況？

  當使用者啟動應用程式時，將不會透過Apple SSO工作流程驗證使用者。 因此，應用程式必須回覆為一般驗證流程，並顯示自己的MVPD選取器。


* 如果使用者使用透過iOS/tvOS平台的[Adobe Pass TVE儀表板](https://experience.adobe.com/#/pass/authentication)在&#x200B;**NO**&#x200B;上設定&#x200B;**啟用單一登入**&#x200B;的MVPD，移至iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;區段進行登入，會發生什麼情況？

  當使用者啟動應用程式時，將不會透過Apple SSO工作流程驗證使用者。 因此，應用程式必須回覆為一般驗證流程，並顯示自己的MVPD選取器。


* 如果使用者有Apple未上線（不支援）的MVPD，但存在於Apple選擇器中，會發生什麼情況？

  當使用者啟動應用程式時，使用者將僅透過Apple SSO工作流程選取MVPD，而未完成驗證流程。 因此，應用程式必須回覆為一般驗證流程，但可以使用已選取的MVPD。


* 如果使用者有Apple未上線（不支援）的MVPD，會發生什麼情況？

  當使用者啟動應用程式時，使用者會透過Apple SSO工作流程選取「其他電視提供者」選擇器。 因此，應用程式必須回覆為一般驗證流程，並顯示自己的MVPD選取器。


* 如果使用者的MVPD透過[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)的媒體降級，會發生什麼情況？

  當使用者啟動應用程式時，將會透過降級機制(而非透過Apple SSO工作流程)來驗證使用者。 使用者應該可以順暢地使用體驗，而若應用程式使用Adobe Pass Authentication AccessEnabler iOS/tvOS SDK，則會透過&#x200B;*N010*&#x200B;警告代碼通知應用程式。


* MVPD使用者ID在Apple SSO和非Apple SSO驗證流程之間會變更嗎？

  預期的使用者ID不會變更，但需要為每個選取的提供者驗證。


* 驗證TTL是否會有任何變更？

  Adobe Pass驗證將繼續遵循程式設計師整合每個MVPD所需的TTL。 透過Apple SSO從一個程式設計人員應用程式導覽至另一個程式設計人員應用程式時，第二個應用程式將擁有其對應程式設計人員x MVPD整合的TTL （不會共用第一個驗證之應用程式的TTL）

|                                      | Adobe Pass驗證TTL已過期 | Adobe Pass驗證TTL有效 |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **Apple的裝置Token TTL已過期** | 使用者未驗證(MVPD選擇器應會出現) | 使用者已完成驗證，且TTL為其Adobe Pass驗證權杖/設定檔的剩餘時間 |
| **Apple的裝置Token TTL有效** | 使用者會透過在TVE儀表板中指定的TTL以無訊息方式驗證，並取得另一個Adobe Pass驗證權杖/設定檔 | 使用者已完成驗證，且TTL為其Adobe Pass驗證權杖/設定檔的剩餘時間 |
