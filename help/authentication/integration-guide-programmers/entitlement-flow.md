---
title: 程式設計師權益流程
description: 程式設計師權益流程
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '1823'
ht-degree: 0%

---

# 程式設計師權益流程 {#prog-entitlement-flow}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

本檔案從程式設計師的角度說明基本權益流程。  如需在此說明的基本TVE整合以外的功能與使用案例相關資訊，請參閱[程式設計師使用案例](/help/authentication/integration-guide-programmers/programmer-use-cases.md)。

Adobe Pass驗證透過為雙方提供安全、一致的介面，調解程式設計師和MVPD之間的權益流程。  在程式設計師方面，Adobe Pass驗證提供兩種一般型別的權益介面：

1. AccessEnabler — 一種使用者端元件，為可轉譯網頁的裝置（例如網頁應用程式、智慧型手機/平板電腦應用程式）上的應用程式提供API程式庫。
2. 無使用者端API — 適用於無法轉譯網頁的裝置（例如機上盒、遊戲機、智慧型電視）的RESTful網路服務。 轉譯網頁的需求來自MVPD要求使用者在MVPD網站上驗證的需求。

除了這裡提供的平台中立概覽外，這裡還有無使用者端API專屬概觀：無使用者端API檔案。 AccessEnabler會在受支援的平台上以原生方式執行(在Web上執行AS / JS、在iOS上執行Objective-C，以及在Android上執行Java)。 AccessEnabler API在支援的平台上保持一致。 所有不支援AccessEnabler的平台都使用相同的無使用者端API。

對於這兩種型別的介面，Adobe Pass驗證會安全地協調程式設計師的應用程式和使用者的MVPD之間的權益流程：

![](../assets/prog-entitlement-flow.png)


*圖： Adobe Pass驗證生態系統*

>[!IMPORTANT]
>
>在上圖中，請注意權利流程有一部分不會透過Adobe Pass驗證伺服器： MVPD登入。 使用者必須登入其MVPD的登入頁面。 基於此要求，在無法轉譯網頁的裝置上，程式設計師的應用程式必須指示使用者切換到可支援網頁的裝置以使用其MVPD登入，之後他們才會回到原始裝置以進行剩餘的權益流程。

## 權益流程 {#entitlement-flow}

基本權益由四個不同的子流程組成
流量：

1. [啟動流程](/help/authentication/integration-guide-programmers/entitlement-flow.md#startup)
1. [驗證流程](/help/authentication/integration-guide-programmers/entitlement-flow.md#authentication)
1. [授權流程](/help/authentication/integration-guide-programmers/entitlement-flow.md#authorization)
1. [登出檔案](/help/authentication/integration-guide-programmers/entitlement-flow.md#logout)

使用者初次造訪程式設計師網站時，軟體權利檔案流程將依照上述順序進行。 不過，在後續的造訪中，根據驗證和授權權杖是否已過期，或根據檢視原則，使用者可能只會通過一兩個子流程。

### 啟動流程 {#startup}

建立程式設計師與裝置的身分識別，執行初始化工作。 這是所有後續軟體權利檔案呼叫的先決條件。

**AccessEnabler**

* **`setRequestor()`** — 使用AccessEnalber建立您的識別碼，並透過延伸功能建立Adobe Pass驗證伺服器。 此呼叫是權益流程其餘部分的先導。 例如，在JavaScript中：

  ```JavaScript
    /* Define the requestor ID (Programmer/aggregator ID). */
      var requestorID = "sample_requestor_Id";
      ...
      // Callback indicating that the AccessEnabler swf has initialized
      function swfLoaded() {
          // AccessEnabler is loaded so we can use the API function it provides
          accessEnablerObject.setRequestor(requestorID); 
      ...
      }
  ```

**無使用者端API**

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`** — 視平台而定，在您的應用程式呼叫regcode之前，可能會有需要完成的先決條件工作。 如需詳細資訊，請參閱&#x200B;**無使用者端API檔案**。 例如，Xbox平台會要求您在呼叫regcode之前完成規定的安全性步驟。

### 驗證流程 {#authentication}

成功的驗證會產生繫結至裝置和請求者的AuthN權杖。 成功的驗證是取得授權的先決條件。

**AccessEnabler**

* `checkAuthentication()` — 檢查本機權杖快取中是否存在有效的快取驗證權杖，而不會實際觸發完整的驗證流程。 這會觸發`setAuthenticationStatus()`回呼函式。
* `getAuthentication()` — 啟動完整的驗證流程。 如果成功，Adobe Pass驗證會產生AuthN權杖，並在使用者端上快取該權杖。 使用者登入其選取的MVPD網站，在iFrame、快顯視窗或Webview中顯示（視平台而定）。 這會觸發displayProviderDialog()。

**無使用者端API**

* `<FQDN>/.../checkauthn` — 上述`checkAuthentication()`的Web服務版本。
* `<FQDN>/.../config` — 將MVPD清單傳回至第二熒幕應用程式。
* `<FQDN>/.../authenticate` — 從第二熒幕應用程式起始驗證流程，將使用者重新導向至其選取的MVPD登入。 如果成功，Adobe Pass驗證會產生AuthN權杖並將其儲存在伺服器上，然後使用者會返回其原始裝置以完成權利流程。

如果下列兩點為True，則AuthN代號視為有效：

* AuthN權杖未過期
* 與AuthN權杖相關聯的MVPD位於目前請求者ID的允許MVPD清單中

#### 通用AccessEnabler初始驗證工作流程 {#generic-ae-initial-authn-flow}

1. 您的應用程式會呼叫`getAuthentication()`來啟動驗證工作流程，以檢查有效的快取驗證Token。 這個方法有選擇性的`redirectURL`引數；如果您沒有提供`redirectURL`的值，在成功驗證之後，使用者會回到初始化驗證的URL。
1. AccessEnabler決定目前的驗證狀態。 如果使用者目前已驗證，AccessEnabler會呼叫您的`setAuthenticationStatus()`回呼函式，傳遞驗證狀態以表示成功。
1. 如果使用者未驗證，AccessEnabler會判斷使用者的上次驗證嘗試在指定的MVPD中是否成功，以繼續驗證流程。 如果已快取MVPD ID且`canAuthenticate`旗標為true或使用`setSelectedProvider()`選取了MVPD，則不會使用MVPD選取對話方塊提示使用者。 驗證流程會繼續使用MVPD的快取值(即上次成功驗證期間使用的MVPD)。 系統會向後端伺服器發出網路呼叫，並將使用者重新導向至MVPD登入頁面。

1. 如果未快取任何MVPD ID，而且未使用`setSelectedProvider()`選取任何MVPD，或`canAuthenticate`標幟設定為false，則會呼叫`displayProviderDialog()`回呼。 此回呼會指示您的應用程式建立UI，向使用者顯示可從中進行選擇的MVPD清單。 提供了一系列MVPD物件，其中包含您建置MVPD選擇器所需的資訊。 每個MVPD物件都說明MVPD實體，並包含MVPD ID和可以找到MVPD標誌的URL等資訊。

1. 選取MVPD後，您的應用程式必須通知AccessEnabler使用者的選擇。 對於非Flash使用者端，一旦使用者選取想要的MVPD，您就會透過呼叫`setSelectedProvider()`方法通知AccessEnabler使用者選取範圍。 Flash使用者端改為分派型別&quot;`mvpdSelection`&quot;的共用`MVPDEvent`，傳遞選取的提供者。

1. 當通知AccessEnabler使用者的MVPD選擇時，會向後端伺服器發出網路呼叫，並將使用者重新導向至MVPD登入頁面。

1. 在驗證工作流程中，AccessEnabler會與Adobe Pass Authentication和選取的MVPD通訊，以索取使用者的憑證（使用者ID和密碼）並驗證其身分。 雖然有些MVPD會重新導向至自己的登入網站，有些則需要您在iFrame中顯示其登入頁面。 您的頁面必須包含建立iFrame的回呼，以防客戶選擇其中一個MVPD。<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)-->。

1. 使用者成功登入後，AccessEnabler會擷取驗證Token，並通知您的應用程式驗證流程已完成。 AccessEnabler以狀態碼1呼叫`setAuthenticationStatus()`回呼，表示成功。 如果在執行這些步驟期間發生錯誤，`setAuthenticationStatus()`回呼會以狀態碼0觸發，表示驗證失敗以及對應的錯誤碼。

>[!IMPORTANT]
>Comcast是目前唯一未提供標誌靜態URL的MVPD。 程式設計師應從[XFINITY開發人員入口網站](https://developers.xfinity.com/products/tv-everywhere)提取最新的標誌。
>

### 授權流程 {#authorization}

授權是檢視受保護內容的先決條件。 成功的授權會產生AuthZ權杖，以及提供給程式設計師應用程式以用於安全目的的短期媒體權杖。 請注意，若要支援授權工作流程，您必須先執行必要的要求者設定，並整合[媒體權杖驗證器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)。 完成這些步驟後，您就可以啟動授權。

當使用者請求存取受保護的資源時，您的應用程式會起始授權。 您可以傳遞指定請求資源的資源ID （例如，頻道、集數等）。 您的應用程式會先檢查是否有已儲存的驗證Token。 如果找不到，您就會啟動驗證程式。

**AccessEnabler**

* `checkAuthorization()` — 檢查授權而不起始完整授權流程。 這通常用於更新程式設計師應用程式UI中顯示的狀態資訊。

* `getAuthorization()` — 起始完整授權流程。

您提供下列回呼函式來處理結果
授權呼叫：

* `setToken()` — 如果先前驗證成功且授權成功，則AccessEnabler會呼叫您的`setToken()`回呼函式，傳遞短期的媒體權杖，表示成功完成Adobe Pass驗證權利流程。 (在允許使用者檢視受保護的內容之前，程式設計師的應用程式會使用媒體權杖驗證器來檢查媒體權杖的有效性。

* `tokenRequestFailed()` — 如果使用者未獲授權使用要求的資源（或若查詢因任何其他原因而失敗），AccessEnabler會呼叫此回呼函式（加上您自己的錯誤報告函式），並傳遞有關失敗的詳細資料。

**無使用者端API**

* `\<FQDN\>/.../authorize` — 起始完整授權流程。

#### 通用AccessEnabler授權工作流程 {#generic-ae-authr-wf}

1. 提供回呼函式，使用`setReqestor()`將指派的程式設計員GUID註冊到Access Enabler。 當AccessEnabler已經執行時，會呼叫此回呼函式
已成功下載。

1. 當使用者請求存取受保護的資源時，呼叫`getAuthorization()`。 使用`getAuthorization()`，傳遞指定請求資源的資源ID （例如，頻道、集數等）。 AccessEnabler會尋找快取的驗證Token，以隨授權要求傳遞。 如果找不到，則會起始驗證流程。
1. 提供回呼函式以處理授權結果：

   * `setToken()` — 如果授權成功，或如果使用者先前已獲得授權，存取啟用程式會透過呼叫您的`setToken()`回呼函式，傳遞短期授權權杖來繼續授權程式。

   * `tokenRequestFailed()` — 如果使用者未獲授權使用要求的資源（或查詢因任何其他原因而失敗），AccessEnabler會呼叫您已登入的任何錯誤報告函式，加上`tokenRequestFailed()`回呼，以傳遞有關失敗的詳細資料。

### 登出流程 {#logout}

清除與目前使用者軟體權利檔案流程關聯的權杖和其他資料。

**AccessEnabler**

* `logout()`

**無使用者端API**

* `\<FQDN\>/.../logout`

## 瞭解AccessEnabler行為 {#ae-behavior}

所有AccessEnabler API呼叫為非同步（但有一個例外，記錄在API參考資料中）。 您可以任意次數呼叫API，但無法保證動作會觸發
，會以呼叫的相同順序完成。 (目前的Flash Player執行階段是一個例外；若不是多執行緒，則會確保呼叫*do*按順序完成
這些區段稱為。)

為了區分回應並且能夠配對回應與呼叫，所有回呼都會回呼其輸入引數。 這包括`setToken()`和`tokenRequestFailed()`，它們最終是由`checkAuthorization()`觸發。 （對於`checkAuthorization()`回呼，會回呼所使用的資源。） 利用此功能，您可以區分哪個回應對應到哪個呼叫。 若要使用此功能，您可以編寫類似以下內容的代碼：

```JavaScript
    for each (resource in ["TNT", "CNN", "TBS", "AdultSwim"] ) {
         ae.checkAuthorization(resource);
    }
    
    // Success callback
    function setToken(resource, token) {
         // Use "resource" to figure 
         // out which checkAuthorization
         // call triggered this response
    }
    
    // Old error callback
    function tokenRequestFailed(resource, error, details) {
         // use "resource" to figure
         // out in response to which
         // checkAuthorization call
         // this was triggered
    }
    
    // Error callback using new error api
    ae.bind("errorEvent',"errorHandler");
    
    function errorHandler(error) {
         if(error.resource) {        
              // Use error.resource to figure
              // which checkAuthorization call
              // triggered this response
         }
    }
```

### AEM行為常見問題集 {#ae-beh-faq}

**問題。 如果在第一個呼叫完成之前進行第二個AccessEnabler呼叫，會發生什麼情況？**

當第二個呼叫執行時，第一個呼叫會繼續執行（非同步通訊）。

**問題。 AccessEnabler可支援的同步呼叫數目是否達到上限？**

AccessEnabler程式碼未明確設定任何限制，因此您僅受可用系統資源及MVPD容量的限制。

<!--

>[!MORELIKETHIS]
>
>*   [Programmer use cases](/help/authentication/programmer-use-cases.md)
>*   Error Reporting
>**Platform Cookbooks:**
>*   Clientless integration cookbook
>*   iOS Integration Cookbook
>*   Android Integration Cookbook
>*   JavaScript Integration Cookbook
>*   ActionScript Integration Cookbook
>*   Windows 8 Integration Cookbook
>*   AIR Native Extension Overview
-->
