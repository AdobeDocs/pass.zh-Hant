---
title: Apple SSO概觀
description: Apple SSO概觀
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# Apple SSO概觀 {#apple-sso-overview}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 簡介 {#Introduction}

Apple提供的API可讓使用者在裝置系統層級登入其電視提供者帳戶，而不需依個別應用程式進行驗證。

因此，Apple與Adobe Pass驗證攜手合作，為iPhone、iPad和Apple電視擁有者在電視隨處生態系統建立平台單一登入(SSO)使用者體驗。

若要在Apple裝置上享有單一登入(SSO)使用者體驗，必須先完成先決條件清單。

</br>

## 必要條件 {#Prerequisites}

必要條件可能適用於一或多個與TVE業務相關的實體，例如程式設計師、MVPD、Adobe Pass驗證或Apple。

</br>

### 程式設計師 {#Programmer}

為了受益於單一登入(SSO)使用者體驗，一位程式設計師必須：

1. 請至少使用Xcode 8版和iOS/tvOS 10版。

1. 將[視訊訂閱者單一登入權利](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on)設定為其Apple開發人員帳戶。 請連絡Apple，為您的Apple團隊識別碼啟用[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)。

1. 透過[Adobe Primetime TVE Dashboard](https://console.auth.adobe.com/)，為每個所需的整合(Channel x MVPD)和所需的平台(iOS / tvOS)啟用單一登入（是）。

1. 使用Apple驗證團隊提供的下列兩個解決方案之一，整合Adobe Pass SSO工作流程：

   - Adobe Pass Authentication REST API可支援平台單一登入(SSO)驗證，適用於在iOS、iPadOS或tvOS上執行之使用者端應用程式的一般使用者。 另請參閱[Apple SSO逐步指南(REST API)](/help/authentication/apple-sso-cookbook-rest-api.md)。

   - Adobe Pass Authentication AccessEnabler iOS/tvOS SDK可支援平台單一登入(SSO)驗證，適用於在iOS、iPadOS或tvOS上執行之使用者端應用程式的一般使用者。 另請參閱[Apple SSO逐步指南(iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md)。

   - **<u>專業秘訣：</u>**&#x200B;若要存取使用者的訂閱資訊，使用者必須授予應用程式繼續的許可權，類似於提供裝置攝影機或麥克風的存取許可權。 必須為每個應用程式要求此許可權，裝置將會儲存使用者的選擇。 請記住，使用者可以透過移至應用程式設定（電視提供者許可權存取），或從iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;移至區段來變更其決定。

   - **<u>專業秘訣：</u>**&#x200B;我們建議在應用程式進入前景狀態時要求使用者的許可權，但這只是建議，因為應用程式可以在要求使用者驗證之前，隨時檢查[存取使用者訂閱資訊的許可權](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)。 此外，AccessEnabler iOS/tvOS SDK API也會在需要時自動要求使用者的許可權。

   - **<u>專業秘訣：</u>**&#x200B;我們建議您說明單一登入(SSO)使用者體驗的優點，以鼓勵拒絕提供許可權存取訂閱資訊的使用者。 請記住，使用者可以透過移至應用程式設定（電視提供者許可權存取），或從iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;移至區段來變更其決定。

此結果應會建立符合下列使用者流程的體驗，建議您先諮詢這些使用者流程，然後再開始開發應用程式：

- [iPhone / iPad](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf)使用者流程
- [Apple TV](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf)使用者流程


>[!IMPORTANT]
>
> 當iOS/tvOS **和**&#x200B;的「單一登入」功能為&#x200B;**啟用**&#x200B;時(在Apple **已上線（支援）或選擇器** MVPD的情況下)，來自Apple SSO工作流程的驗證/登出流程將同時涉及Apple和Adobe Pass驗證解決方案，而所有其他流程（授權、預先授權、中繼資料等） 將僅由Adobe Pass驗證提供服務。


>[!IMPORTANT]
>
> 當iOS/tvOS的「單一登入」功能已&#x200B;**停用** **或** (若是Apple **未上線（不支援）** MVPD)時，驗證/登出流程將會從Apple SSO工作流程遞補為僅由Adobe Pass驗證服務的一般工作流程。


>[!IMPORTANT]
>
> Apple SSO工作流程的主要收益是以單熒幕驗證使用者流程表示，當Apple TV的Single Sign-On功能為tvOS **啟用**&#x200B;時&#x200B;**，以及Apple**&#x200B;已上線（支援）**MVPD的情況下**&#x200B;時，也可以在TV上傳送該流程。


### MVPD {#MVPD}

若要受益於單一登入(SSO)使用者體驗，請選
MVPD必須：



1. 在Apple端加入Apple SSO工作流程。 請聯絡Apple以協助入門流程。
1. 提供可處理使用者登入表單的JavaScript TVML應用程式。 請聯絡Apple以接收適當的檔案。
1. 提供字串值，代表Apple在上線流程中指派的提供者識別碼。 請聯絡Adobe Pass驗證以執行設定變更。

</br>

## 常見問題集 {#FAQ}

1. 如果Apple SSO工作流程發生問題，使用AccessEnabler iOS/tvOS SDK的應用程式是否能夠回覆為一般驗證流程？
   - 這是可能的，但需要在[Adobe Primetime TVE儀表板](https://console.auth.adobe.com/)上執行設定變更。 必須將&#x200B;*啟用單一登入*&#x200B;設定在&#x200B;*NO*，才能使用所需的整合(Channel x MVPD)和所需的平台(iOS/tvOS)。
   - 應用程式只有在呼叫[setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) API之後，才能確認組態變更，以防它使用AccessEnabler iOS/tvOS SDK。
1. 應用程式會知道透過其他裝置或其他應用程式上的平台SSO登入而導致驗證發生的時間嗎？
   - 將無法取得此資訊。
1. 應用程式會知道透過相同裝置上的平台SSO登入而導致驗證發生的時間嗎？
   - 此資訊屬於使用者中繼資料金鑰： *tokenSource*&#x200B;的一部分，在此案例中應傳回字串值： &quot;Apple&quot;。
1. 如果使用者使用未與應用程式整合的MVPD前往iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS區段上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;登入，會發生什麼情況？
   - 當使用者啟動應用程式時，將不會透過Apple SSO工作流程驗證使用者。 因此，應用程式必須退回一般驗證流程，並顯示自己的MVPD選擇器。
1. 如果使用者使用MVPD (在適用於iOS/tvOS平台的[iOS TVE儀表板](https://console.auth.adobe.com/)的&#x200B;*NO*&#x200B;上設定&#x200B;*啟用單一登入*)登入Adobe Primetime/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS區段上的&#x200B;*`Settings -> Accounts -> TV Provider`*，會發生什麼情況？
   - 當使用者啟動應用程式時，將不會透過Apple SSO工作流程驗證使用者。 因此，應用程式必須退回一般驗證流程，並顯示自己的MVPD選擇器。
1. 如果使用者有Apple未上線（不支援）的MVPD，但存在於Apple選擇器中，會發生什麼情況？
   - 當使用者啟動應用程式時，使用者將僅透過Apple SSO工作流程選取MVPD，而未完成驗證流程。 因此，應用程式必須退回一般驗證流程，但可以使用已選取的MVPD。
1. 如果使用者有Apple未上線（不支援）的MVPD，會發生什麼情況？
   - 當使用者啟動應用程式時，使用者會透過Apple SSO工作流程選取「其他電視提供者」選擇器。 因此，應用程式必須退回一般驗證流程，並顯示自己的MVPD選擇器。
1. 如果使用者的MVPD透過[Adobe Primetime TVE Dashboard](https://console.auth.adobe.com/)的媒體降級，會發生什麼情況？
   - 當使用者啟動應用程式時，將會透過降級機制(而非透過Apple SSO工作流程)來驗證使用者。
   - 使用者應該可以順暢地使用體驗，而若應用程式使用AccessEnabler iOS/tvOS SDK，則會透過&#x200B;*N010*&#x200B;警告程式碼通知應用程式。
1. MVPD使用者ID在Apple SSO和非Apple SSO驗證流程之間會變更嗎？
   - 預期的使用者ID不會變更，但需要為每個選取的提供者驗證。
1. 驗證TTL是否會有任何變更？
   - Adobe Pass驗證將繼續遵循程式設計師整合每個MVPD所需的TTL。
   - 透過Apple SSO從一個程式設計人員應用程式導覽至另一個程式設計人員應用程式時，第二個應用程式將擁有其對應程式設計人員x MVPD整合的TTL （它不會共用第一個驗證之應用程式的TTL）

|                                      | Adobe Pass驗證TTL已過期 | Adobe Pass驗證TTL有效 |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Apple的裝置Token TTL已過期** | 使用者未驗證（MVPD選擇器應會出現） | 使用者已完成驗證，且TTL為其Adobe Pass驗證權杖的剩餘時間 |
| **Apple的裝置Token TTL有效** | 使用者會以無訊息方式驗證，並以TVE儀表板中指定的TTL取得另一個Adobe Pass驗證權杖 | 使用者已完成驗證，且TTL為其Adobe Pass驗證權杖的剩餘時間 |

<!--

## Resources {#Resources}

- [Apple SSO Cookbook (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md)
- [Apple SSO Cookbook (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md)
- [Sign in with your TV provider on your iPhone, iPad, or iPod touch](https://support.apple.com/en-us/HT207035)
- [Use your pay TV or cable provider with Apple TV](https://support.apple.com/en-us/HT207035)
- [TV providers that let you sign in on your iPhone, iPad, or Apple TV](https://support.apple.com/en-us/HT208084)
- [TV Provider Authentication](https://developer.apple.com/design/human-interface-guidelines/tvos/system-capabilities/tv-provider-authentication/)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
