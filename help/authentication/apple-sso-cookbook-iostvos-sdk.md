---
title: Apple SSO逐步指南(iOS/tvOS SDK)
description: Apple SSO逐步指南(iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '1867'
ht-degree: 0%

---

# Apple SSO逐步指南(iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 簡介 {#Introduction}

Adobe Primetime Authentication AccessEnabler iOS/tvOS SDK可支援平台單一登入(SSO)驗證，適用於在iOS、iPadOS或tvOS上執行之使用者端應用程式的一般使用者，操作方式為所謂的Apple SSO工作流程。

請注意，本檔案可當作現有AccessEnabler iOS/tvOS SDK檔案的擴充功能，您可在其中找到 [此處](/help/authentication/iostvos-sdk-api-reference.md).

</br>

## 逐步指南 {#Cookbook}

為了從Apple SSO使用者體驗中獲益，一個應用程式需要整合AccessEnabler iOS/tvOS SDK，並遵循下面提供的提示順序。

</br>

### 必要條件 {#Prerequisites}

</br>

#### 許可權

>[!TIP]
>
> **<u>專業秘訣：</u>** 為了能夠存取使用者的訂閱資訊，使用者必須授予應用程式繼續的許可權，類似於提供裝置的相機或麥克風的存取權。 必須為每個應用程式要求此許可權，裝置將會儲存使用者的選擇。 請記住，使用者可以移至應用程式設定（電視提供者許可權存取）或部分，變更其決定 *`Settings -> TV Provider`* 在iOS/iPadOS上或 *`Settings -> Accounts -> TV Provider`* 在tvOS上。

>[!TIP]
>
> **<u>專業秘訣：</u>** 我們建議在應用程式進入前景狀態時請求使用者的許可權，但這只是一個建議，因為應用程式可以檢查 [存取許可權](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) 在要求使用者驗證之前，隨時提供使用者的訂閱資訊。 此外，AccessEnabler iOS/tvOS SDK API也會在需要時自動要求使用者的許可權。

>[!TIP]
>
> **<u>專業秘訣：</u>** 如果使用者未授予其訂閱資訊的存取權，或是與視訊訂閱者帳戶架構的通訊失敗，則AccessEnabler iOS/tvOS SDK將會退回一般驗證流程。

>[!TIP]
>
> **<u>專業秘訣：</u>** 我們建議您說明單一登入(SSO)使用者體驗的優點，以鼓勵拒絕授予許可權存取訂閱資訊的使用者。 請記住，使用者可以移至應用程式設定（電視提供者許可權存取）或部分，變更其決定 *`Settings -> TV Provider`* 在iOS/iPadOS上或 *`Settings -> Accounts -> TV Provider`* 在tvOS上。


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

</br>

#### 回呼

>[!TIP]
>
> **<u>專業秘訣：</u>** 實作以下清單 [回呼](/help/authentication/iostvos-sdk-api-reference.md) 這些特定於Apple SSO工作流程。

- [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog)  — 當Apple MVPD選擇器即將開啟時觸發的回呼。
- [*dissisesTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog)  — 當Apple MVPD選擇器即將關閉時觸發的回呼。

</br>

#### 錯誤報告

>[!TIP]
>
> **<u>專業秘訣：</u>** 實作以下清單 [進階錯誤碼](/help/authentication/error-reporting.md) 這些特定於Apple SSO工作流程。

- ***N003***  — 使用者從Apple MVPD選擇器選取「其他電視提供者」選項。
- ***N004***  — 使用者從Apple MVPD選擇器選取目前要求者不支援的電視提供者（整合或停用單一登入）。
- ***N005***  — 使用者決定取消一般MVPD選擇器或Apple MVPD選擇器。
- ***VSA403***  — 應用程式拒絕使用者的TV提供者許可權。
- ***VSA404***  — 應用程式未決定使用者的TV提供者許可權。
- ***VSA503***  — 視訊訂閱者帳戶中繼資料要求失敗，中提供了更多內容 *message* 欄位。
- ***AAPL / APPL_ERROR***  — 視訊訂閱者帳戶中繼資料要求失敗，中提供了更多內容 *詳細資料* 欄位。

</br>

### 驗證 {#Authentication}

>[!TIP]
>
> **<u>秘訣：</u>** 請依照下列步驟實作iOS/iPadOS/tvOS。

1. 應用程式必須 [初始化](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) AccessEnabler iOS/tvOS SDK。
1. 應用程式必須 [設定目前的要求者識別碼](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **重要：** 此第二個步驟可能會觸發 [進階錯誤碼](/help/authentication/error-reporting.md) 如果是Apple SSO工作流程的特定專案 **下列其中一項true**：

   - ***VSA403***  — 應用程式拒絕使用者的TV提供者許可權。
   - ***VSA404***  — 應用程式未決定使用者的TV提供者許可權。
   - ***APPL*** - AccessEnabler iOS/tvOS SDK與視訊訂閱者帳戶架構之間的通訊發生錯誤。

   第二個步驟會嘗試以無訊息方式將Apple SSO設定檔交換為Adobe驗證Token，以防萬一 **以上皆為false** 和 **以下全部為true**：

   - 使用者的電視提供者許可權已授予應用程式。
   - 使用者已在裝置系統層級登入其電視提供者帳戶。
   - AccessEnabler iOS/tvOS SDK從視訊訂閱者帳戶架構收到使用者的電視提供者識別碼。
   - 使用者的電視提供者與應用程式的整合可透過Adobe Primetime TVE Dashboard啟用。
   - 使用者透過應用程式的電視提供者單一登入功能可透過Adobe Primetime TVE控制面板啟用。
   - 使用者的電視提供者不會透過Adobe Primetime TVE Dashboard降級。
   - AccessEnabler iOS/tvOS SDK從視訊訂閱者帳戶架構收到使用者的電視提供者SAML回應。

   **<u>專業秘訣：</u>** 此第二個步驟不會觸發任何其他回呼，除了 [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) 回呼，因為應用程式並未明確起始驗證。

1. 應用程式必須 [檢查驗證狀態](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **重要：** 此第三個步驟可能會觸發 [進階錯誤碼](/help/authentication/error-reporting.md) 如果是Apple SSO工作流程的特定專案 **下列其中一項true**：

   - ***VSA403**  — 使用者已在裝置系統層級登入其TV提供者帳戶，但使用者的TV提供者許可權已遭拒。
   - ***VSA404**  — 使用者已在裝置系統層級登入其TV提供者帳戶，但使用者的TV提供者許可權未決定於應用程式。
   - ***APPL\_ERROR**  — 使用者已在裝置系統層級登入其電視提供者帳戶，但AccessEnabler iOS/tvOS SDK與視訊訂閱者帳戶架構之間的通訊發生錯誤。

   **重要：** 此第三個步驟將觸發 [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) 回呼方法 *狀態* 等於0，如果是 **下列其中一項true**：

   - 使用者並未在裝置系統層級登入其TV提供者帳戶，或是透過一般驗證流程登入。
   - 使用者已在裝置系統層級或透過一般驗證流程登入其電視提供者帳戶，但使用者的電視提供者驗證權杖TTL已通過。
   - 使用者已在裝置系統層級或透過一般驗證流程登入其電視提供者帳戶，但使用者與應用程式的電視提供者整合已透過Adobe Primetime TVE Dashboard停用。
   - 使用者已在裝置系統層級登入其電視提供者帳戶，但使用者使用應用程式的電視提供者單一登入已透過Adobe Primetime TVE Dashboard停用。
   - 使用者已在裝置系統層級登入其TV提供者帳戶，但使用者的TV提供者許可權已遭拒。
   - 使用者已在裝置系統層級登入其TV提供者帳戶，但使用者的TV提供者許可權未決定於應用程式。
   - 使用者已在裝置系統層級登入其電視提供者帳戶，但AccessEnabler iOS/tvOS SDK與視訊訂閱者帳戶架構之間的通訊發生錯誤。

   **重要：** 此第三個步驟將觸發 [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) 回呼方法 *狀態* 等於1，如果是 **以上皆為false。**


1. 應用程式必須 [初始化驗證](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) 如果先前的驗證狀態檢查觸發 [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) 回呼方法 *狀態* 等於0。

   **<u>專業秘訣：</u>** 實作下列AccessEnabler iOS/tvOS SDK API之一 [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) 或 [getAuthentication：filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **重要：** 第四個步驟可能會觸發 [進階錯誤碼](/help/authentication/error-reporting.md) 如果是Apple SSO工作流程的特定專案 **下列其中一項true**：

   - ***VSA403***  — 應用程式拒絕使用者的TV提供者許可權。
   - ***VSA404***  — 應用程式未決定使用者的TV提供者許可權。
   - ***VSA503*** - AccessEnabler iOS/tvOS SDK與視訊訂閱者帳戶架構之間的通訊發生錯誤。
   - ***N003***  — 使用者從Apple MVPD選擇器選取「其他電視提供者」選項。
   - ***N004***  — 使用者從Apple MVPD選擇器選取目前要求者不支援的電視提供者（整合或停用單一登入）。
   - ***N005***  — 使用者決定取消一般MVPD選擇器或Apple MVPD選擇器。

   **重要：** 這第四個步驟將回溯至一般驗證流程，觸發 [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) 回呼和 **一** 以上的 [進階錯誤碼](/help/authentication/error-reporting.md)，以防萬一 **以上其中一個為true**.

   **重要：** 這第四個步驟將回溯至一般驗證流程，觸發 [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) 或 [navigateToUrl：useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) 回呼和 **無** 以上的 [進階錯誤碼](/help/authentication/error-reporting.md)，以防止使用者已選取不支援Apple SSO、但存在於Apple MVPD選擇器中的電視提供者。

   **<u>專業秘訣：</u>** AccessEnabler iOS/tvOS SDK會無訊息呼叫 [setSelectedProvder](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) API，以防止使用者已選取不支援Apple SSO、但存在於Apple MVPD選擇器中的電視提供者。

   **重要：** 第四個步驟會嘗試以無訊息方式將Apple SSO設定檔交換為Adobe驗證Token，以防萬一 **以上皆為false** 和 **以下全部為true**：

   - 使用者的電視提供者許可權已授予應用程式。
   - 使用者已登入/目前正在裝置系統層級登入其電視提供者帳戶。
   - AccessEnabler iOS/tvOS SDK從視訊訂閱者帳戶架構收到使用者的電視提供者識別碼。
   - 使用者的電視提供者與應用程式的整合可透過Adobe Primetime TVE Dashboard啟用。
   - 使用者透過應用程式的電視提供者單一登入功能可透過Adobe Primetime TVE控制面板啟用。
   - 使用者的電視提供者不會透過Adobe Primetime TVE Dashboard降級。
   - AccessEnabler iOS/tvOS SDK從視訊訂閱者帳戶架構收到使用者的電視提供者SAML回應。



>**<u>專業秘訣：</u>** 第四個步驟將觸發 [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus) 回呼，不考慮 *狀態* 結果，因為驗證是由應用程式明確啟動。


</br>

### 中繼資料 {#Metadata}

應用程式可選擇使用「*tokenSource&quot;* [使用者中繼資料](/help/authentication/iostvos-sdk-api-reference.md#getMeta) 來自AccessEnabler iOS/tvOS SDK的API。

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

</br>

### 登出 {#Logout}

此 [視訊訂閱者帳戶](https://developer.apple.com/documentation/videosubscriberaccount) 架構未提供API以程式設計方式將已在裝置系統層級登入其電視提供者帳戶的人登出。 因此，登出若要完全生效，使用者必須明確從登出 *`Settings -> TV Provider`* 在iOS/iPadOS上或 *`Settings -> Accounts -> TV Provider`* 在tvOS上。 使用者可以使用的另一個選項，是從特定應用程式設定區段（TV提供者許可權存取）撤銷存取使用者訂閱資訊的許可權。

>[!TIP]
>
> **<u>秘訣：</u>** 透過AccessEnabler iOS/tvOS SDK媒體實作此專案 [登出](/help/authentication/iostvos-sdk-api-reference.md#logout) API。


>[!TIP]
>
> **<u>專業秘訣：</u>** 請依照下列步驟進行tvOS實作。

- 應用程式必須 [啟動登出](/help/authentication/iostvos-sdk-api-reference.md#logout) 從AccessEnabler iOS/tvOS SDK。 這不會促進MVPD端的工作階段清理。
- 應用程式必須指示/提示使用者明確從登出 *`Settings -> Accounts -> TV Provider`* 僅限在tvOS上 [*VSA203* 狀態代碼已觸發](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>專業秘訣：</u>** 請依照下列步驟實作iOS/iPadOS。

- 應用程式必須 [啟動登出](/help/authentication/iostvos-sdk-api-reference.md#logout) 從AccessEnabler iOS/tvOS SDK。 這有助於MVPD端的工作階段清理。
- 應用程式必須指示/提示使用者明確從登出 *`Settings -> TV Provider`* 僅適用於iOS/iPadOS上 [*VSA203* 狀態代碼已觸發](/help/authentication/error-reporting.md).


<!--
## Resources {#Resources}

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [AccessEnabler iOS/tvOS SDK Overview](/help/authentication/iostvos-sdk-overview.md)
- [AccessEnabler iOS/tvOS SDK Cookbook](/help/authentication/iostvos-sdk-cookbook.md)
- [AccessEnabler iOS/tvOS SDK API Reference](/help/authentication/iostvos-sdk-api-reference.md)
- [Error Reporting](/help/authentication/error-reporting.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
