---
title: REST API V2逐步指南（伺服器對伺服器）
description: REST API V2逐步指南（伺服器對伺服器）
source-git-commit: 87d4d95a3bf4ace68bc71ca700b09da14ee316f4
workflow-type: tm+mt
source-wordcount: '1566'
ht-degree: 0%

---

# REST API V2逐步指南（伺服器對伺服器） {#rest-api-v2-cookbook-server-to-server}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。


## 概觀 {#overview}

本逐步指南檔案的目的，在於詳細說明在伺服器對伺服器架構中使用REST API V2實作Adobe Pass驗證的最佳實務。  它提供基本需求、逐步流程實作，以及生產環境和作業的一般考量事項。

>[!IMPORTANT]
>
> REST API V2實作受到[節流機制](/help/authentication/throttling-mechanism.md)檔案的限制。


## 元件 {#components}

在運作中的伺服器對伺服器解決方案中，涉及下列元件：


| 型別 | 元件 | 說明 |
| --- | --- | --- |
| 串流裝置 | 串流應用程式 | 位在使用者串流裝置上並播放已驗證視訊的程式設計師應用程式。 |
| | \[Optional\]驗證模組 | 如果串流裝置具有使用者代理（亦即Web瀏覽器），則AuthN模組須負責在MVPD IdP上驗證使用者。 |
| \[Optional\] AuthN裝置 | AuthN應用程式 | 如果串流裝置沒有使用者代理程式（亦即Web瀏覽器），則AuthN應用程式為程式設計人員網頁應用程式，可使用網頁瀏覽器從個別使用者的裝置進行存取。 |
| 程式設計師基礎結構 | 程式設計師服務 | 此服務會將串流裝置與Adobe Pass服務連結在一起，以取得驗證和授權決策。 |
| Adobe Infrastructure | Adobe Pass Service | 與MVPD IdP和AuthZ服務整合，並提供驗證和授權決定的服務。 |
| MVPD基礎結構 | MVPD IdP | MVPD端點，提供認證型驗證服務來驗證其使用者的身分。 |
| | MVPD AuthZ服務 | MVPD端點會根據使用者的訂閱、家長監護等提供授權決策。 |


[](/help/authentication/glossary.md)

下圖說明了整個流程：

![](/help/authentication/assets/rest-api-v2/cookbooks/apass-servertoserver-cookbook.png)

### 在伺服器對伺服器架構中實作REST API V2的步驟 {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

若要實作Adobe Pass REST API V2，您必須依照下列步驟分組至階段。

## 0.必要條件 {#prerequisites}

In server-to-server implementations, the Streaming App and Programmer Service needs to establish a protocol, for Programmer Service to be able to :
* uniquely identify the Streaming App on the device
* act on behalf of the Streaming App and communicate with Adobe Pass Service
* collect and store information about the Streaming App and the device like IP address, source port, device information to pass it to Adobe Pass
* return decisions and instructions to the Streaming App

Parameters like device ID, client id, client secret (defined below) can be stored on either Streaming App or Programmer Service.

## A.註冊階段 {#registration-phase}

### 步驟1：註冊您的應用程式 {#step-1-register-your-application}

應用程式若要能夠呼叫Adobe Pass REST API V2，它需要API安全性層所需的存取權杖。 對於伺服器對伺服器實作，Programmer Service可以代表應用程式執行個體註冊。
需要為每個串流裝置取得使用者端ID和使用者端密碼值。

若要取得存取Token，程式設計師服務可以代表串流應用程式執行，而且需要遵循如下的步驟： [動態使用者端註冊](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)

## B.驗證階段 {#authentication-phase}

### 步驟2：檢查現有的已驗證設定檔 {#step-2-check-for-existing-authenticated-profiles}

程式設計師服務代表串流應用程式檢查現有的已驗證設定檔： `/api/v2/{serviceProvider}/profiles` （[擷取已驗證的設定檔](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)）

* 如果找不到設定檔，且串流應用程式實作TempPass流程
   * 請遵循如何實作[暫時存取流程](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)的檔案
* 如果找不到設定檔，串流應用程式會實作驗證流程
   * <b></b><b></b><br>[](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
   * the Programmer Service may implement filtering on the list of MVPDs and display only MVPDs intended while hiding others (TempPass, test MVPDs, MVPDs under development, etc.)
   * the Programmer Service should return a filtered MVPD list for the Streaming App to display picker, User selects the MVPD
   * <b></b><br>[](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)<br>
      * a CODE and URL to use for authentication is returned
      * 如果找到設定檔，程式設計師服務可能會繼續到<a href="#preauthorization-phase">C。預先授權階段</a>
   * 程式設計師服務應該會將程式碼和URL傳回串流應用程式

### 步驟3：驗證使用者 {#step-3-authenticate-the-user}

使用瀏覽器或第二熒幕Web應用程式：

* 選項1。 串流應用程式可以開啟瀏覽器或Web檢視、載入URL以進行驗證，且使用者會登陸需要提交認證的MVPD登入頁面
   * 使用者輸入登入/密碼，最終重新導向顯示成功頁面
* 選項2。 串流應用程式無法開啟瀏覽器而只顯示程式碼。 <b>需要開發個別的Web應用程式AuthN_APP</b>，以要求使用者輸入CODE、建置並開啟URL： <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * 使用者輸入登入/密碼，最終重新導向顯示成功頁面

### 步驟4：檢查已驗證的設定檔 {#step-4-check-for-authenticated-profiles}

程式設計師服務會檢查是否使用MVPD進行驗證，以在瀏覽器或第二個畫面中完成

* 建議在<b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>上每15秒輪詢一次
（[擷取特定MVPD的已驗證設定檔](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)）
   * 如果未在串流應用程式中選取MVPD，因為MVPD選擇器出現在第二個畫面應用程式中，則應該使用程式碼<b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>進行輪詢
（[擷取特定程式碼的已驗證設定檔](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)）
* 輪詢不應超過30分鐘，如果達到30分鐘且串流應用程式仍處於作用中狀態，則需要起始新的工作階段，並將傳回新的程式碼和URL
* 驗證完成後，傳回值為200且已驗證設定檔
* 程式設計師服務可能會繼續到<a href="#preauthorization-phase">C。預先授權階段</a>

## C.預先授權階段 {#preauthorization-phase}

### 步驟5：檢查預先授權的資源 {#step-5-check-for-preauthorized-resources}

有了有效的使用者驗證設定檔，程式設計師服務就可以檢查
存取可用的影片，並將清單傳遞至串流應用程式以顯示。

* 如果應用程式想要篩選驗證的使用者套件中無法提供的資源，步驟為選擇性並執行
* 呼叫<b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br>
（[使用特定MVPD擷取預先授權決定](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)）

## D.授權階段 {#authorization-phase}

### 步驟6：檢查授權的資源 {#step-6-check-for-authorized-resources}

串流應用程式已準備好播放使用者選取的視訊/資產/資源。

* Step is necessary for every play start
* Streaming App pass this information to the Programmer Service
* <b></b><br>[](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
   * 決定= &#39;允許&#39; ，程式設計師服務指示串流應用程式開始串流
   * 決定= &#39;拒絕&#39;，程式設計師服務指示串流應用程式通知使用者其無法存取該視訊
   * 在此過程中，程式設計師服務可能會評估其他商業規則，並將適當的決定傳回串流應用程式

## E.登出階段 {#logout-phase}

### 步驟7：登出 {#step-7-logout}

串流應用程式：使用者想要從MVPD登出

* 串流應用程式會通知程式設計人員服務，它需要從MVPD登出此特定應用程式。
* 程式設計人員服務可能會清除其儲存的已驗證使用者相關資訊
* 程式設計師服務呼叫<b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
（[啟動特定MVPD的登出](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)）
* 如果response actionType=&#39;interactive&#39;且url出現，程式設計師服務就會將url傳回串流應用程式
* 根據現有的功能，串流應用程式可能會在瀏覽器中開啟url （通常與驗證所用的相同）
* If the Streaming App does not have a browser or it is a different instance than the one at authentication, the flow can be stopped as the MVPD session was not persisted in the browser cache.

## 環境和功能需求{#environments}

程式設計師應建立至少兩個環境：一個用於生產，一個或更多用於測試。


### Production

The production environment should be highly available and scaled appropriately for large or unexpected spikes (e.g. live sports, breaking
news).



The Adobe Pass service runs on multiple data centers geographically dispersed throughout the US.  To achieve the best response time (i.e. lowest latency) from the Adobe Pass service, the Programmer should also create a similar geographically dispersed service
infrastructure.


如果Adobe需要重新路由流量，程式設計師服務應該將DNS快取限製為最多30秒。 如果資料中心無法使用，就可能會發生這種情況。


程式設計師應提供生產環境的公用IP範圍。 這些會輸入到Adobe Pass基礎結構中的IP允許清單中，以供存取，並由Adobe的Fair API使用原則進行管理。

### 分段

中繼環境可以是最小的，但應包括所有系統元件和業務邏輯。 其功能應與生產環境類似，並允許在生產環境之外測試版本。 理想情況下，預備環境可連線至Adobe Pass測試環境以供程式設計師使用，並在需要時透過Adobe使用，以便我們可協助測試和疑難排解。

### 功能需求

程式設計人員服務必須傳遞其執行流程之裝置的正確裝置識別資訊。 此外，程式設計師服務必須傳遞他們執行流程的裝置的IP （在x-forwarded-for標頭中）以及連線來源連線埠（在裝置資訊欄位中）：

程式設計師服務應該傳送個別MVPD或整合應用程式所需的資料和格式（例如裝置IP、來源連線埠、裝置資訊、MRSS、選擇性資料，例如ECID）。<!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->。

程式設計師服務必須在快取時遵守驗證設定檔和決定有效性，並在收到通知時使驗證或決定失效。

The Programmer Service must maintain certificates shared with Adobe (for encrypted user metadata).

## Related Information {#related}

* [REST API V2 Reference](/help/authentication/rest-api-v2/rest-api-v2-flows-overview.md)
