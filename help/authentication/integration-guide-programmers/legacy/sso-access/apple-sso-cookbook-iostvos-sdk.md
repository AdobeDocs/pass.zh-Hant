---
title: Apple SSO逐步指南(iOS/tvOS SDK)
description: Apple SSO逐步指南(iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 0%

---

# （舊版） Apple SSO逐步指南(iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

Adobe Pass Authentication AccessEnabler iOS/tvOS SDK支援在iOS、iPadOS或tvOS上執行之使用者端應用程式的一般使用者進行合作夥伴單一登入(SSO)。

此檔案可作為現有AccessEnabler iOS/tvOS SDK檔案的擴充功能，可在[此處](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)找到。

## 逐步指南 {#apple-sso-cookbook-iostvos-sdk-cookbook}

為了從Apple SSO使用者體驗中獲益，應用程式需要整合AccessEnabler iOS/tvOS SDK，並遵循以下顯示的步驟順序。

### 先決條件 {#apple-sso-cookbook-iostvos-sdk-prerequisites}

#### 許可權 {#apple-sso-cookbook-iostvos-sdk-permission}

>[!TIP]
>
> **<u>專業秘訣：</u>**&#x200B;串流應用程式必須要求存取儲存在裝置層級的使用者訂閱資訊，使用者必須授予應用程式繼續的許可權，類似於提供裝置攝影機或麥克風的存取權。 必須使用Apple的[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)為每個應用程式要求此許可權，裝置將會儲存使用者的選擇。

>[!TIP]
>
> **<u>專業秘訣：</u>**&#x200B;我們建議您說明Apple單一登入使用者體驗的優點，以鼓勵拒絕授予許可權存取訂閱資訊的使用者，但請注意，使用者可以變更其決定，方法是移至應用程式設定（電視提供者許可權存取）、iOS和iPadOS上的&#x200B;*`Settings -> TV Provider`*，或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*。

>[!TIP]
>
> **<u>專業秘訣：</u>**&#x200B;當應用程式進入前景狀態時，串流應用程式可以要求使用者的許可權，因為應用程式可以在要求使用者驗證前，隨時檢查[存取使用者訂閱資訊的許可權](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus)。

>[!TIP]
>
> **<u>專業秘訣：</u>**&#x200B;如果使用者未授予其訂閱資訊的存取權，或是與視訊訂閱者帳戶架構的通訊失敗，則AccessEnabler iOS/tvOS SDK將回覆為一般驗證流程。

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                   // Do nothing.
                
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                   // Incentivize users who refuse to give permission to access subscription information by explaining the benefits of the Single Sign-On (SSO) user experience. Please bear in mind that the user can change its decision by going to the application settings (TV Provider permission access) or to the section from Settings -> TV Provider on iOS/iPadOS or Settings -> Accounts -> TV Provider on tvOS.
                   ...
                }
    }
    ... 
```

#### 回呼 {#apple-sso-cookbook-iostvos-sdk-callbacks}

>[!TIP]
>
> **<u>專業秘訣：</u>**&#x200B;實作下列[回呼](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)清單，這些回呼是特定於Apple SSO工作流程的。

* [*presentTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) — 當Apple MVPD選取器即將開啟時觸發回呼。
* [*dissiseTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) — 當Apple MVPD選取器即將關閉時觸發回呼。

#### 錯誤報告 {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>專業秘訣：</u>**&#x200B;實作下列[進階錯誤碼](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)清單，這些是特定於Apple SSO工作流程的。

* ***N003*** — 使用者從Apple MVPD選擇器中選取「其他電視提供者」選項。
* ***N004*** — 使用者從Apple MVPD選擇器中選取電視提供者，但目前的要求者不支援該電視提供者（整合或停用單一登入）。
* ***N005*** — 使用者決定取消一般MVPD選取器或Apple MVPD選取器。
* ***VSA403*** — 應用程式拒絕使用者的TV提供者許可權。
* ***VSA404*** — 應用程式未決定使用者的TV提供者許可權。
* ***VSA503*** — 視訊訂閱者帳戶中繼資料要求失敗，*訊息*&#x200B;欄位中有更多內容。
* ***AAPL / APPL_ERROR*** — 視訊訂閱者帳戶中繼資料要求失敗，*詳細資料*&#x200B;欄位中有更多內容。

### 驗證 {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>秘訣：</u>**&#x200B;請依照下列步驟進行iOS/iPadOS/tvOS實作。

1. 應用程式必須[初始化](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) AccessEnabler iOS/tvOS SDK。


1. 應用程式必須[設定目前的要求者識別碼](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3)。

   **重要：**&#x200B;第二個步驟可能會觸發Apple SSO工作流程專屬的[進階錯誤碼](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)，以防下列其中一項&#x200B;**為true**：

   * ***VSA403*** — 應用程式拒絕使用者的TV提供者許可權。
   * ***VSA404*** — 應用程式未決定使用者的TV提供者許可權。
   * ***APPL*** - AccessEnabler iOS/tvOS SDK與視訊訂閱者帳戶架構之間的通訊發生錯誤。

   第二個步驟會嘗試以無訊息方式將Apple SSO設定檔交換為Adobe驗證Token，以防上述&#x200B;**全部為false**&#x200B;且&#x200B;**以下全部為true**：

   * 使用者的電視提供者許可權已授予應用程式。
   * 使用者已在裝置系統層級登入其電視提供者帳戶。
   * AccessEnabler iOS/tvOS SDK從視訊訂閱者帳戶架構收到使用者的電視提供者識別碼。
   * 透過Adobe Primetime TVE Dashboard啟用使用者的電視提供者與應用程式的整合。
   * 使用者透過應用程式的電視提供者單一登入會透過Adobe Primetime TVE儀表板啟用。
   * 使用者的電視提供者不會透過Adobe Primetime TVE儀表板降級。
   * AccessEnabler iOS/tvOS SDK從視訊訂閱者帳戶架構收到使用者的電視提供者SAML回應。

   **<u>專業秘訣：</u>**&#x200B;此第二個步驟不會觸發任何其他回呼（除了[setRequestorComplete](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete)回呼），因為應用程式並未明確起始驗證。


1. 應用程式必須[檢查驗證狀態](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkauthentication-checkauthn)。

   **重要：**&#x200B;第三個步驟可能會觸發Apple SSO工作流程專屬的[進階錯誤碼](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)，以防下列其中一項&#x200B;**為true**：

   * ***VSA403** — 使用者已登入其電視提供者帳戶，位於
裝置系統層級，但使用者的電視提供者許可權為
已拒絕應用程式。
   * ***VSA404** — 使用者已登入其電視提供者帳戶，位於
裝置系統層級，但使用者的電視提供者許可權
應用程式的未決定。
   * ***APPL\_ERROR** — 使用者已登入其電視提供者
裝置系統層級的帳戶，但與
AccessEnabler iOS/tvOS SDK和視訊訂閱者帳戶
框架發生錯誤。

   **重要：**&#x200B;此第三個步驟會觸發具有&#x200B;[*狀態*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus)&#x200B;等於0的&#x200B;*setAuthenticationStatus*&#x200B;回呼，如果&#x200B;**下列其中一個為true**：

   * 使用者並未在裝置系統層級登入其TV提供者帳戶，或是透過一般驗證流程登入。
   * 使用者已在裝置系統層級或透過一般驗證流程登入其電視提供者帳戶，但使用者的電視提供者驗證權杖TTL已通過。
   * 使用者已在裝置系統層級或透過一般驗證流程登入其電視提供者帳戶，但使用者與應用程式的電視提供者整合已透過Adobe Primetime TVE儀表板停用。
   * 使用者已在裝置系統層級登入其電視提供者帳戶，但使用者使用應用程式的電視提供者單一登入已透過Adobe Primetime TVE儀表板停用。
   * 使用者已在裝置系統層級登入其TV提供者帳戶，但使用者的TV提供者許可權已遭拒。
   * 使用者已在裝置系統層級登入其TV提供者帳戶，但使用者的TV提供者許可權未決定於應用程式。
   * 使用者已在裝置系統層級登入其電視提供者帳戶，但AccessEnabler iOS/tvOS SDK與視訊訂閱者帳戶架構之間的通訊發生錯誤。

   **重要：**&#x200B;此第三個步驟將會觸發&#x200B;[*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus)&#x200B;回呼，其中&#x200B;*狀態*&#x200B;等於1，若上述&#x200B;**全部為false。**


1. 若先前的驗證狀態檢查觸發了[setAuthenticationStatus](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn)回呼，且&#x200B;[*狀態*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus)&#x200B;等於0，應用程式將必須&#x200B;*初始化驗證*。

   **<u>專業秘訣：</u>**&#x200B;實作下列其中一個AccessEnabler iOS/tvOS SDK API [getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN)或[getAuthentication:filter](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN_filter)。

   **重要：**&#x200B;如果下列其中一項[為true](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)，此第四個步驟可能會觸發Apple SSO工作流程專屬的&#x200B;**進階錯誤碼**：

   * ***VSA403*** — 應用程式拒絕使用者的TV提供者許可權。
   * ***VSA404*** — 應用程式未決定使用者的TV提供者許可權。
   * ***VSA503*** - AccessEnabler iOS/tvOS SDK與視訊訂閱者帳戶架構之間的通訊發生錯誤。
   * ***N003*** — 使用者從Apple MVPD選擇器中選取「其他電視提供者」選項。
   * ***N004*** — 使用者從Apple MVPD選擇器中選取電視提供者，但目前的要求者不支援該電視提供者（整合或停用單一登入）。
   * ***N005*** — 使用者決定取消一般MVPD選取器或Apple MVPD選取器。

   **重要：**&#x200B;此第四個步驟會退回至一般驗證流程，方法是觸發上述[進階錯誤碼](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dispProvDialog)的&#x200B;**displayProviderDialog**&#x200B;回呼及[一個](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)，若上述的&#x200B;**一個為true**。

   **重要：**&#x200B;如果使用者選取的電視提供者不支援Apple SSO，但存在於Apple MVPD選擇器中，此第四個步驟會回溯至一般的驗證流程，方法是觸發[navigateToUrl](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2url)或[navigateToUrl:useSVC](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2urlSVC)回呼，以及上述&#x200B;**進階錯誤碼**&#x200B;中的[none](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)。

   **<u>專業秘訣：</u>** AccessEnabler iOS/tvOS SDK會無訊息呼叫[setSelectedPrrovder](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) API，以防使用者選取的電視提供者不支援Apple SSO，但存在於Apple MVPD選取器中。

   **重要：**&#x200B;此第四個步驟會嘗試以無訊息方式交換Apple SSO設定檔以取得Adobe驗證Token，以防上述的&#x200B;**全部為false**&#x200B;且&#x200B;**以下全部為true**：

   * 使用者的電視提供者許可權已授予應用程式。
   * 使用者已登入/目前正在裝置系統層級登入其電視提供者帳戶。
   * AccessEnabler iOS/tvOS SDK從視訊訂閱者帳戶架構收到使用者的電視提供者識別碼。
   * 透過Adobe Primetime TVE Dashboard啟用使用者的電視提供者與應用程式的整合。
   * 使用者透過應用程式的電視提供者單一登入會透過Adobe Primetime TVE儀表板啟用。
   * 使用者的電視提供者不會透過Adobe Primetime TVE儀表板降級。
   * AccessEnabler iOS/tvOS SDK從視訊訂閱者帳戶架構收到使用者的電視提供者SAML回應。

   **<u>專業秘訣：</u>**&#x200B;此第四個步驟將會觸發&#x200B;[*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setAuthNStatus)&#x200B;回呼，不論&#x200B;*狀態*&#x200B;結果為何，因為驗證是由應用程式明確啟動。

### 中繼資料 {#apple-sso-cookbook-iostvos-sdk-metadata}

應用程式可以選擇使用AccessEnabler iOS/tvOS SDK的&#x200B;*tokenSource&quot;* [使用者中繼資料](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) API，判斷是否因為透過合作夥伴SSO登入而發生驗證。

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### 登出 {#apple-sso-cookbook-iostvos-sdk-logout}

[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)未提供API以程式設計方式登出已在裝置系統層級登入其電視提供者帳戶的人員。 因此，若要讓登出完全生效，使用者必須從iOS/iPadOS上的&#x200B;*`Settings -> TV Provider`*&#x200B;或tvOS上的&#x200B;*`Settings -> Accounts -> TV Provider`*&#x200B;明確登出。 使用者可以使用的另一個選項，是從特定應用程式設定區段（TV提供者許可權存取）撤銷存取使用者訂閱資訊的許可權。

>[!TIP]
>
> **<u>秘訣：</u>**&#x200B;透過AccessEnabler iOS/tvOS SDK [登出](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) API來實作此專案。

>[!TIP]
>
> **<u>專業秘訣：</u>**&#x200B;請依照下列步驟進行tvOS實作。

* 應用程式必須從AccessEnabler iOS/tvOS SDK [起始登出](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout)。 這無助於在MVPD端清理工作階段。
* 只有在觸發&#x200B;*`Settings -> Accounts -> TV Provider`* VSA203 [*狀態碼時，應用程式才必須指示/提示使用者從tvOS上的*&#x200B;明確登出](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)。

>[!TIP]
>
> **<u>專業秘訣：</u>**&#x200B;請依照下列步驟實作iOS/iPadOS。

* 應用程式必須從AccessEnabler iOS/tvOS SDK [起始登出](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout)。 這將有助於MVPD端的工作階段清理。
* 只有在觸發&#x200B;*`Settings -> TV Provider`* VSA203 [*狀態碼時，應用程式才能指示/提示使用者從iOS/iPadOS上的*&#x200B;明確登出](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md)。
