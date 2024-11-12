---
title: REST API V2逐步指南（伺服器對伺服器）
description: REST API V2逐步指南（伺服器對伺服器）
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: 563e0b17de3be9290a661242355b4835b8c386e1
workflow-type: tm+mt
source-wordcount: '1578'
ht-degree: 0%

---

# REST API V2逐步指南（伺服器對伺服器） {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> REST API V2實作受到[節流機制](/help/authentication/throttling-mechanism.md)檔案的限制。

本逐步指南檔案的目的，在於詳細說明在伺服器對伺服器架構中使用REST API V2實作Adobe Pass驗證的最佳實務。 它提供基本需求、逐步流程實作，以及生產環境和作業的一般考量事項。

## 元件 {#components}

在運作中的伺服器對伺服器解決方案中，下列元件會參與其中：

| 型別 | 元件 | 說明 |
|---------------------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 串流裝置 | 串流應用程式 | 位在使用者串流裝置上並播放已驗證視訊的程式設計師應用程式。 |
|                           | \[Optional\]驗證模組 | 如果串流裝置具有使用者代理程式（亦即Web瀏覽器），則AuthN模組將負責在MVPD IdP上驗證使用者。 |
| \[Optional\] AuthN裝置 | AuthN應用程式 | 如果串流裝置沒有使用者代理程式（亦即Web瀏覽器），則AuthN應用程式為程式設計人員網頁應用程式，可使用網頁瀏覽器從個別使用者的裝置進行存取。 |
| 程式設計師基礎結構 | 程式設計師服務 | 此服務會將串流裝置與Adobe Pass服務連結在一起，以取得驗證和授權決策。 |
| Adobe基礎結構 | Adobe Pass服務 | 與MVPD IdP和AuthZ服務整合，並提供驗證和授權決定的服務。 |
| MVPD基礎結構 | MVPD IdP | MVPD端點，提供認證型驗證服務來驗證其使用者的身分。 |
|                           | MVPD AuthZ服務 | MVPD端點會根據使用者的訂閱、家長監護等提供授權決策。 |

流程中使用的其他辭彙已在[字彙表](/help/authentication/glossary.md)中定義。

下圖說明了整個流程：

![REST API V2逐步指南（伺服器對伺服器）](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

### 在伺服器對伺服器架構中實作REST API V2的步驟 {#steps-to-implement-the-rest-api-v2-in-server-to-server-infrastructure}

若要實作Adobe Pass REST API V2，您必須依照下列步驟分組至階段。

## 0.必要條件 {#prerequisites}

在伺服器對伺服器實作中，串流應用程式和程式設計師服務需要為程式設計師服務建立通訊協定，才能：

* 唯一識別裝置上的串流應用程式。
* 代表串流應用程式採取行動，並與Adobe Pass服務通訊。
* 收集和儲存串流應用程式和裝置（例如IP位址、來源連線埠、裝置資訊）的相關資訊，以便將其傳遞至Adobe Pass。
* 將決定和指示傳回串流應用程式。

裝置ID、使用者端ID、使用者端密碼（定義如下）等引數可儲存在串流應用程式或程式設計人員服務中。

## A.註冊階段 {#registration-phase}

### 步驟1：註冊您的應用程式 {#step-1-register-your-application}

應用程式若要能夠呼叫Adobe Pass REST API V2，它需要API安全性層所需的存取權杖。

對於伺服器對伺服器實作，「程式設計人員服務」可以代表應用程式執行個體進行註冊，但是每個串流裝置都需要取得使用者端認證（使用者端ID和使用者端密碼）值。

若要取得存取Token，程式設計師服務可以代表串流應用程式執行，而且必須遵循[動態使用者端註冊](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)檔案中所述的步驟。

## B.驗證階段 {#authentication-phase}

### 步驟2：檢查現有的已驗證設定檔 {#step-2-check-for-existing-authenticated-profiles}

程式設計師服務代表串流應用程式檢查現有的已驗證設定檔： `/api/v2/{serviceProvider}/profiles` （[擷取已驗證的設定檔](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)）。

* 如果找不到設定檔，且串流應用程式實作TempPass流程
   * 請遵循如何實作[暫時存取流程](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)的檔案
* 如果找不到設定檔，串流應用程式會實作驗證流程
   * <b>步驟2.a：</b>程式設計師服務擷取可用於serviceProvider的MVPD清單： <b>/api/v2/{serviceProvider}/configuration</b><br>
（[擷取可用的MVPD清單](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)）
   * 程式設計人員服務可能會對MVPD清單實作篩選，並只顯示想要隱藏的MVPD （TempPass、測試MVPD、開發中的MVPD等）
   * 程式設計師服務應該傳回串流應用程式的已篩選MVPD清單，以顯示選擇器，使用者會選取MVPD
   * 從串流應用程式中選取MVPD後，程式設計師服務會建立工作階段： <b>/api/v2/{serviceProvider}/sessions</b><br>
（[建立驗證工作階段](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)）<br>
      * 系統會傳回驗證所用的程式碼和URL
      * 如果找到設定檔，程式設計師服務可能會繼續到<a href="#preauthorization-phase">C。預先授權階段</a>
   * 程式設計師服務應該將程式碼和URL傳回串流應用程式

### 步驟3：驗證使用者 {#step-3-authenticate-the-user}

使用瀏覽器或第二個畫面網頁式應用程式：

* 選項1。 串流應用程式可以開啟瀏覽器或Webview、載入URL以進行驗證，且使用者會登陸需要提交認證的MVPD登入頁面
   * 使用者輸入登入/密碼，最終重新導向顯示成功頁面
* 選項2。 串流應用程式無法開啟瀏覽器而只顯示程式碼。 <b>需要開發個別的Web應用程式AuthN_APP</b>，要求使用者輸入CODE、建置並開啟URL： <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * 使用者輸入登入/密碼，最終重新導向顯示成功頁面

### 步驟4：檢查已驗證的設定檔 {#step-4-check-for-authenticated-profiles}

程式設計師服務會檢查是否使用MVPD進行驗證，以在瀏覽器或第二個畫面中完成

* 建議在<b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>上每15秒輪詢一次
（[擷取特定MVPD的已驗證設定檔](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)）
   * 如果未在串流應用程式中選取MVPD，因為MVPD選擇器出現在第二個畫面應用程式中，則應該使用程式碼<b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>進行輪詢
（[擷取特定程式碼的已驗證設定檔](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)）
* 如果輪詢時間達到30分鐘，且串流應用程式仍在作用中，則需要起始新工作階段，並傳回新的程式碼和URL，則輪詢不應超過30分鐘
* 驗證完成後，傳回值為200且包含已驗證的設定檔
* 程式設計師服務可能會繼續到<a href="#preauthorization-phase">C。預先授權階段</a>

## C.預先授權階段 {#preauthorization-phase}

### 步驟5：檢查預先授權的資源 {#step-5-check-for-preauthorized-resources}

有了有效的使用者驗證設定檔，程式設計師服務就可以檢查對可用視訊的存取權，並將清單傳遞至串流應用程式以顯示。

* 如果應用程式想要篩選掉已驗證的使用者套件中無法提供的資源，則步驟為選擇性並執行
* 呼叫<b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br>
（[使用特定MVPD擷取預先授權決定](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)）

## D.授權階段 {#authorization-phase}

### 步驟6：檢查授權的資源 {#step-6-check-for-authorized-resources}

串流應用程式已準備好播放使用者選取的視訊/資產/資源。

* 每個播放開始都需要執行步驟
* 串流應用程式會將此資訊傳遞給程式設計人員服務
* 程式設計師服務代表串流應用程式，請呼叫<b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
（[使用特定MVPD擷取授權決定](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)）
   * 決定= &#39;允許&#39;，程式設計師服務會指示串流應用程式開始串流
   * decision = &#39;Deny&#39;，程式設計師服務會指示串流應用程式通知使用者其無法存取該視訊
   * 在此過程中，程式設計師服務可能會評估其他商業規則，並將適當的決定傳回串流應用程式

## E.登出階段 {#logout-phase}

### 步驟7：登出 {#step-7-logout}

串流應用程式：使用者想要從MVPD登出

* 串流應用程式會通知程式設計人員服務，它必須從MVPD登出此特定應用程式。
* 程式設計人員服務可能會清除其儲存的已驗證使用者相關資訊
* 程式設計師服務呼叫<b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
（[啟動特定MVPD的登出](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)）
* 如果response actionType=&#39;interactive&#39;且url出現，程式設計師服務就會將url傳回串流應用程式
* 根據現有的功能，串流應用程式可能會在瀏覽器中開啟url （通常與驗證所用的相同）
* 如果串流應用程式沒有瀏覽器，或它與驗證時的執行個體不同，則流量可以停止，因為MVPD工作階段不會持續存在於瀏覽器快取中。

## 環境和功能需求{#environments}

程式設計師應建立至少兩個環境：一個用於生產，一個或更多用於測試。

### 生產

生產環境應具備高可用性，且規模要適合大型或未預期的尖峰（例如，即時運動、突發新聞）。

Adobe Pass服務可在分散於美國各地的多個資料中心執行。 為了讓Adobe Pass服務達到最佳的回應時間（亦即最低的延遲），程式設計師也應該建立類似的地理位置分散的服務基礎架構。

如果Adobe需要重新路由流量，程式設計師服務應該將DNS快取限製為最多30秒。 如果資料中心無法使用，就可能會發生這種情況。

程式設計師應提供生產環境的公用IP範圍。 這些會輸入到Adobe Pass基礎結構中的IP允許清單中，以供存取，並由Adobe的Fair API使用原則進行管理。

### 分段

中繼環境可以是最小的，但應包括所有系統元件和業務邏輯。 其功能應與生產環境類似，並允許在生產環境之外測試版本。 理想情況下，預備環境可連線至Adobe Pass測試環境以供程式設計師使用，並在需要時透過Adobe使用，因此我們可協助進行測試和疑難排解。

### 功能需求

程式設計人員服務必須傳遞其執行流程之裝置的正確裝置識別資訊。 此外，程式設計師服務必須傳遞他們執行流程的裝置的IP （在x-forwarded-for標頭中）以及連線來源連線埠（在裝置資訊欄位中）：

程式設計師服務應該傳送個別MVPD或整合應用程式所需的資料和格式（例如，裝置IP、來源連線埠、裝置資訊、MRSS、選擇性資料，例如ECID）。<!--Please see the documentation for [Passing Device and Connection Information Cookbook](http://tve.helpdocsonline.com/passing-device-information-cookbook)-->。

程式設計師服務必須在快取時遵守驗證設定檔和決定有效性，並在收到通知時使驗證或決定失效。

程式設計師服務必須維護與Adobe共用的憑證（針對加密的使用者中繼資料）。

## 相關資訊 {#related}

* [REST API V2參考](/help/authentication/rest-api-v2/rest-api-v2-overview.md)
