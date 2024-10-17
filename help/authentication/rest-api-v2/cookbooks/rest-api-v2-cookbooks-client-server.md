---
title: REST API V2逐步指南（使用者端對伺服器）
description: REST API V2逐步指南（使用者端對伺服器）
source-git-commit: 0d6693d51887c9e794401e984f3a4075be091ee5
workflow-type: tm+mt
source-wordcount: '695'
ht-degree: 0%

---


# REST API V2逐步指南（使用者端對伺服器） {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> REST API V2實作受到[節流機制](/help/authentication/throttling-mechanism.md)檔案的限制。

## 在使用者端應用程式中實作REST API V2的步驟 {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

若要實作Adobe Pass REST API V2，您必須依照下列步驟（分為幾個階段）進行。

## A.註冊階段 {#registration-phase}

### 步驟1：註冊您的應用程式 {#step-1-register-your-application}

應用程式若要能夠呼叫Adobe Pass REST API V2，它需要API安全性層所需的存取權杖。

若要取得存取Token，應用程式必須依照說明的步驟進行： [動態使用者端註冊](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)

## B.驗證階段 {#authentication-phase}

### 步驟2：檢查現有的已驗證設定檔 {#step-2-check-for-existing-authenticated-profiles}

串流應用程式檢查現有的已驗證設定檔： <b>/api/v2/{serviceProvider}/設定檔</b><br>
（[擷取已驗證的設定檔](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)）

* 如果找不到設定檔，且串流應用程式實作TempPass流程
   * 請遵循如何實作[暫時存取流程](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)的檔案
* 如果找不到設定檔，且串流應用程式實作驗證流程
   * 串流應用程式會擷取適用於serviceProvider的MVPD清單： <b>/api/v2/{serviceProvider}/configuration</b><br>
（[擷取可用的MVPD清單](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)）
   * 串流應用程式可能會在MVPD清單上實作篩選，並只顯示想要用來隱藏其他MVPD （TempPass、測試MVPD、開發中的MVPD等）
   * 串流應用程式顯示選取器，使用者選取MVPD
   * 串流應用程式會建立工作階段： <b>/api/v2/{serviceProvider}/sessions</b><br>
（[建立驗證工作階段](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)）<br>
      * 會傳回用於驗證的程式碼和URL
      * 如果找到設定檔，串流應用程式可能會繼續到<a href="#preauthorization-phase">C。預先授權階段</a>或<a href="#authorization-phase">D。授權階段</a>

### 步驟3：驗證使用者 {#step-3-authenticate-the-user}

使用瀏覽器或第二熒幕Web應用程式：

* 選項1。 串流應用程式可以開啟瀏覽器或Web檢視、載入URL以進行驗證，且使用者會登陸需要提交認證的MVPD登入頁面
   * 使用者輸入登入/密碼，最終重新導向顯示成功頁面
* 選項2。 串流應用程式無法開啟瀏覽器而只顯示程式碼。 <b>需要開發個別的Web應用程式</b>，要求使用者輸入程式碼、建置並開啟URL： <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * 使用者輸入登入/密碼，最終重新導向顯示成功頁面

### 步驟4：檢查已驗證的設定檔 {#step-4-check-for-authenticated-profiles}

串流應用程式會檢查是否有MVPD驗證，以在瀏覽器或第二個畫面中完成

* 建議在<b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>上每15秒輪詢一次
（[擷取特定MVPD的已驗證設定檔](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)）
   * 如果未在串流應用程式中選取MVPD，因為MVPD選擇器出現在第二個畫面應用程式中，則應該使用程式碼<b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>進行輪詢
（[擷取特定程式碼的已驗證設定檔](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)）
* 輪詢不應超過30分鐘，如果達到30分鐘且串流應用程式仍處於作用中狀態，則需要起始新的工作階段，並將傳回新的程式碼和URL
* 驗證完成後，傳回值為200且包含已驗證的設定檔
* 串流應用程式可能會繼續到<a href="#preauthorization-phase">C。預先授權階段</a>或<a href="#authorization-phase">D。授權階段</a>

## C.預先授權階段 {#preauthorization-phase}

### 步驟5：檢查預先授權的資源 {#step-5-check-for-preauthorized-resources}

串流應用程式準備顯示已驗證身分的使用者可用的影片，且可檢查
存取這些資源。

* 如果應用程式想要篩選驗證的使用者套件中無法提供的資源，步驟為選擇性並執行
* 呼叫<b>/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}</b><br>
（[使用特定MVPD擷取預先授權決定](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)）

## D.授權階段 {#authorization-phase}

### 步驟6：檢查授權的資源 {#step-6-check-for-authorized-resources}

串流應用程式準備播放使用者選取的視訊/資產/資源。

* 每個播放開始都需要執行步驟
* 呼叫<b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
（[使用特定MVPD擷取授權決定](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)）
   * 決定= &#39;允許&#39; ，串流裝置開始串流
   * 決定= &#39;拒絕&#39;，串流裝置會通知使用者它無法存取該視訊

## E.登出階段 {#logout-phase}

### 步驟7：登出 {#step-7-logout}

串流裝置：使用者想要從MVPD登出

* 呼叫<b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
（[啟動特定MVPD的登出](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)）
* 如果出現response actionType=&#39;interactive&#39;和url，請在瀏覽器/第二個畫面中開啟url以完成以MVPD登出
