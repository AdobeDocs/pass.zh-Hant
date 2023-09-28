---
title: Apple SSO概觀
description: Apple SSO概觀
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1452'
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

1. 擁有 [視訊訂閱者單一登入權益](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) 已設定至其Apple開發人員帳戶。 請聯絡Apple以啟用 [視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount) 以取得您的Apple團隊ID。

1. 透過為每個所需的整合(Channel x MVPD)和所需的平台(iOS / tvOS)啟用單一登入（是） [Adobe Primetime TVE控制面板](https://console.auth.adobe.com/).

1. 使用Apple驗證團隊提供的下列兩個解決方案之一，整合Adobe Pass SSO工作流程：

   - Adobe Pass Authentication REST API可支援平台單一登入(SSO)驗證，適用於在iOS、iPadOS或tvOS上執行之使用者端應用程式的一般使用者。 另請參閱 [Apple SSO逐步指南(REST API)](/help/authentication/apple-sso-cookbook-rest-api.md).

   - Adobe Pass Authentication AccessEnabler iOS/tvOS SDK可支援平台單一登入(SSO)驗證，適用於在iOS、iPadOS或tvOS上執行之使用者端應用程式的一般使用者。 另請參閱 [Apple SSO逐步指南(iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md).

   - **<u>專業秘訣：</u>** 為了能夠存取使用者的訂閱資訊，使用者必須授予應用程式繼續的許可權，類似於提供裝置的相機或麥克風的存取權。 必須為每個應用程式要求此許可權，裝置將會儲存使用者的選擇。 請記住，使用者可以移至應用程式設定（電視提供者許可權存取）或部分，變更其決定 *`Settings -> TV Provider`* 在iOS/iPadOS上或 *`Settings -> Accounts -> TV Provider`* 在tvOS上。

   - **<u>專業秘訣：</u>** 我們建議在應用程式進入前景狀態時請求使用者的許可權，但這只是一個建議，因為應用程式可以檢查 [存取許可權](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) 在要求使用者驗證之前，隨時提供使用者的訂閱資訊。 此外，AccessEnabler iOS/tvOS SDK API也會在需要時自動要求使用者的許可權。

   - **<u>專業秘訣：</u>** 我們建議您說明單一登入(SSO)使用者體驗的優點，以鼓勵拒絕授予許可權存取訂閱資訊的使用者。 請記住，使用者可以移至應用程式設定（電視提供者許可權存取）或部分，變更其決定 *`Settings -> TV Provider`* 在iOS/iPadOS上或 *`Settings -> Accounts -> TV Provider`* 在tvOS上。

此結果應會建立符合下列使用者流程的體驗，建議您先諮詢這些使用者流程，然後再開始開發應用程式：

- [IPHONE / IPAD](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf) 使用者流程
- [APPLE TV](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) 使用者流程


>[!IMPORTANT]
>
> 當單一登入功能為 **已啟用** 適用於iOS/tvOS **和** 以Apple為例 **已上線（支援）或選擇器** MVPD來自Apple SSO工作流程的驗證/登出流程將同時涉及Apple和Adobe Pass驗證解決方案，而所有其他流程（授權、預先授權、中繼資料等） 將僅由Adobe Pass驗證提供服務。


>[!IMPORTANT]
>
> 當單一登入功能為 **已停用** 適用於iOS/tvOS **或** 以Apple為例 **未上線（不支援）** MVPD驗證/登出流程會從Apple SSO工作流程後援至僅由Adobe Pass驗證服務的一般工作流程。


>[!IMPORTANT]
>
> Apple SSO工作流程的一個主要好處是以單熒幕驗證使用者流程表示；當單一登入功能為時，也可在Apple電視上傳送該流程 **已啟用** 適用於tvOS **和** 以Apple為例 **已上線（支援）** MVPDs。


### MVPD {#MVPD}

若要受益於單一登入(SSO)使用者體驗，一個MVPD必須：



1. 在Apple端加入Apple SSO工作流程。 請聯絡Apple以協助入門流程。
1. 提供可處理使用者登入表單的JavaScript TVML應用程式。 請聯絡Apple以接收適當的檔案。
1. 提供字串值，代表Apple在上線流程中指派的提供者識別碼。 請聯絡Adobe Pass驗證以執行設定變更。

</br>

## 常見問題集 {#FAQ}

1. 如果Apple SSO工作流程發生問題，使用AccessEnabler iOS/tvOS SDK的應用程式是否能夠回覆為一般驗證流程？
   - 這是可行的，但需要對 [Adobe Primetime TVE控制面板](https://console.auth.adobe.com/). 此 *啟用單一登入* 必須設定於 *否* 用於所需的整合(Channel x MVPD)和所需的平台(iOS/tvOS)。
   - 應用程式只有在呼叫後才會確認組態變更 [setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) API可在使用AccessEnabler iOS/tvOS SDK時使用。
1. 應用程式會知道透過其他裝置或其他應用程式上的平台SSO登入而導致驗證發生的時間嗎？
   - 將無法取得此資訊。
1. 應用程式會知道透過相同裝置上的平台SSO登入而導致驗證發生的時間嗎？
   - 此資訊屬於使用者中繼資料索引鍵的一部分： *tokenSource*，此情況下應傳回字串值： 「Apple」。
1. 如果使用者登入時前往「 」 *`Settings -> TV Provider`* 在iOS/iPadOS上或 *`Settings -> Accounts -> TV Provider`* 是否在tvOS區段上使用未與應用程式整合的MVPD？
   - 當使用者啟動應用程式時，將不會透過Apple SSO工作流程驗證使用者。 因此，應用程式必須退回一般驗證流程，並顯示自己的MVPD選擇器。
1. 如果使用者登入時前往「 」 *`Settings -> TV Provider`* 在iOS/iPadOS上或 *`Settings -> Accounts -> TV Provider`* 在tvOS區段上，使用 *啟用單一登入* 設定於 *否* 於 [Adobe Primetime TVE控制面板](https://console.auth.adobe.com/) 適用於iOS/tvOS平台？
   - 當使用者啟動應用程式時，將不會透過Apple SSO工作流程驗證使用者。 因此，應用程式必須退回一般驗證流程，並顯示自己的MVPD選擇器。
1. 如果使用者有Apple未上線（不支援）的MVPD，但存在於Apple選擇器中，會發生什麼情況？
   - 當使用者啟動應用程式時，使用者將僅透過Apple SSO工作流程選取MVPD，而未完成驗證流程。 因此，應用程式必須退回一般驗證流程，但可以使用已選取的MVPD。
1. 如果使用者有Apple未上線（不支援）的MVPD，會發生什麼情況？
   - 當使用者啟動應用程式時，使用者會透過Apple SSO工作流程選取「其他電視提供者」選擇器。 因此，應用程式必須退回一般驗證流程，並顯示自己的MVPD選擇器。
1. 如果使用者的MVPD透過媒體降級 [Adobe Primetime TVE控制面板](https://console.auth.adobe.com/)？
   - 當使用者啟動應用程式時，將會透過降級機制(而非透過Apple SSO工作流程)來驗證使用者。
   - 使用者應該可以順暢地使用體驗，而應用程式則會透過 *N010* 警告程式碼，以備使用AccessEnabler iOS/tvOS SDK時使用。
1. MVPD使用者ID在Apple SSO和非Apple SSO驗證流程之間會變更嗎？
   - 預期的使用者ID不會變更，但需要為每個選取的提供者驗證。
1. 驗證TTL是否會有任何變更？
   - Adobe Pass驗證將繼續遵循程式設計師整合每個MVPD所需的TTL。
   - 透過Apple SSO從一個程式設計人員應用程式導覽至另一個程式設計人員應用程式時，第二個應用程式將擁有其對應程式設計人員x MVPD整合的TTL （它不會共用第一個驗證之應用程式的TTL）

|                                      | Adobe Pass驗證TTL已過期 | Adobe Pass驗證TTL有效 |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Apple的裝置代號TTL已過期** | 使用者未驗證（MVPD選擇器應會出現） | 使用者已完成驗證，且TTL為其Adobe Pass驗證權杖的剩餘時間 |
| **Apple的裝置代號TTL有效** | 使用者會以無訊息方式驗證，並以TVE儀表板中指定的TTL取得另一個Adobe Pass驗證權杖 | 使用者已完成驗證，且TTL為其Adobe Pass驗證權杖的剩餘時間 |

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
