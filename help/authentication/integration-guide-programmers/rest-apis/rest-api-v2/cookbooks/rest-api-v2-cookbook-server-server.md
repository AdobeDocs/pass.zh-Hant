---
title: REST API V2逐步指南（伺服器對伺服器）
description: REST API V2逐步指南（伺服器對伺服器）
exl-id: 3160c03c-849d-4d39-95e5-9a9cbb46174d
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '2497'
ht-degree: 0%

---

# REST API V2逐步指南（伺服器對伺服器） {#rest-api-v2-cookbook-server-to-server}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> REST API V2實作受到[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)檔案的限制。

本檔案適用於將[Adobe Pass Authentication REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)整合至具有伺服器對伺服器(S2S)架構之串流應用程式的開發人員。

## 先決條件 {#prerequisites}

如需辭彙與定義，請參閱[REST API V2字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)檔案。

如需強制需求和建議的作法，請參閱[REST API V2檢查清單](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md)檔案。

如需常見問題的相關資訊，請參閱[REST API V2常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)檔案。

### 元件 {#components}

開始之前，請確定您瞭解工作流程中使用的下列元件和術語：

| 型別 | 元件 | 說明 |
|---------------------------|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adobe基礎結構 | Adobe Pass服務 | 與MVPD IdP和AuthZ服務整合，以提供驗證和授權決策。 |
| 程式設計師基礎結構 | 程式設計師服務 | 將串流裝置連線至Adobe Pass服務，以取得已驗證的設定檔和授權決策。 |
| MVPD基礎結構 | MVPD IdP服務 | 負責認證式驗證的MVPD端點，驗證使用者的身分。 |
|                           | MVPD AuthZ服務 | MVPD端點，可根據使用者訂閱、家長監護及其他權益規則來決定授權決策。 |
| 串流裝置 | 串流應用程式 | 在使用者的串流裝置上執行並播放已驗證視訊內容的程式設計師應用程式。 |
|                           | （選用） AuthN模組 | 如果串流裝置具有使用者代理（例如瀏覽器），AuthN模組會處理MVPD IdP上的使用者驗證。 |
| （選用） AuthN裝置 | AuthN應用程式 | 如果串流裝置缺少使用者代理程式（例如瀏覽器），則AuthN應用程式是從個別裝置透過網頁瀏覽器存取的程式設計人員網頁應用程式。 |

### 需求 {#requirements}

在伺服器對伺服器(S2S)實作中，串流應用程式和程式設計師服務必須建立通訊協定，讓程式設計師服務能夠：

* 代表串流應用程式與Adobe Pass服務通訊。

* 收集並傳遞[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)標頭要求的串流裝置的唯一裝置識別碼。

* 收集並傳遞精確的串流裝置資訊，包括來源連線埠和裝置特定的詳細資料，如[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)標頭所要求。

* 收集並傳遞X-Forwarded-For標頭要求的串流裝置IP位址。

* 將裝置ID、使用者端ID和使用者端密碼等引數安全地儲存在串流應用程式或程式設計人員服務中。

* 依照MVPD和整合式應用程式來格式化及傳送資料，包括裝置IP、來源連線埠、裝置特定資訊、MRSS和選用的識別碼（例如ECID）。

* 維護並安全地管理與Adobe共用的憑證，以傳輸加密的使用者中繼資料。

* 在快取時遵循驗證設定檔和授權決策有效性，確保在通知時驗證和授權狀態失效。

* 將授權決定和相關指示傳回串流應用程式。

### 環境 {#environments}

進入工作流程之前，請確定您至少維持兩個環境：生產和預備。

**生產**

生產環境必須具備高可用性，且規模適當以處理大型或意外的流量尖峰，例如體育賽事直播或突發新聞導致的流量尖峰。

* Adobe Pass服務在美國各地多個分散的資料中心運作，以最佳化效能並將延遲降至最低。

   * 程式設計師服務應採用類似的基礎架構策略，確保Adobe Pass提供低延遲回應時間。

* 程式設計師必須提供其生產環境的公用IP範圍。

   * 這些IP將新增至Adobe Pass基礎結構中的允許清單。

* 程式設計師服務必須將DNS快取限制在最多30秒，以允許動態重新路由，以防Adobe由於資料中心無法使用而需要重新導向流量。

**暫存**

中繼環境可能最小，但應包含所有關鍵系統元件和業務邏輯來映象生產。

* 在部署到生產環境之前，它必須允許測試版本。

* 它在操作上應保持與生產相似，以實現現實的測試。

* 理想情況下，預備環境應連結至Adobe Pass測試環境，以：

   * 允許程式設計師針對Adobe的基礎建設進行測試。

   * 啟用Adobe ，以便在必要時協助進行測試和疑難排解。

## 工作流程 {#workflow}

執行以下步驟，如下圖所示。

![REST API V2逐步指南（伺服器對伺服器）](/help/authentication/assets/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-to-server-diagram.png)

*REST API V2逐步指南（伺服器對伺服器）*

## A.註冊階段 {#registration-phase}

註冊階段的目的是透過動態使用者端註冊(DCR)程式，針對Adobe Pass驗證註冊串流應用程式。

動態使用者端註冊(DCR)程式需要串流應用程式取得一對使用者端憑證，並擷取存取權杖作為註冊階段的終端目標。

註冊階段是強制性的，但如果串流應用程式具有快取的使用者端憑證對和仍然有效的存取權杖，則可跳過此階段。

+++相關文章

API：

* [擷取使用者端認證](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [擷取存取權杖](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

流量：

* [動態使用者端註冊流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

常見問答：

* [註冊階段常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### 步驟1：註冊您的應用程式 {#step-1-register-your-application}

* 擷取使用者端認證：程式設計師服務會呼叫&#x200B;[**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)&#x200B;端點來擷取使用者端認證。

   * 程式設計人員服務或程式設計人員應用程式必須儲存使用者端憑證，並在需要擷取存取權杖時無限期使用。


* 擷取存取Token：程式設計師服務會呼叫&#x200B;[**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)&#x200B;端點來擷取存取Token。

   * 程式設計師服務或程式設計師應用程式必須儲存和使用存取權杖，直到它過期為止，然後捨棄它並取得新的存取權杖。

## B.驗證階段 {#authentication-phase}

驗證階段的目的是為串流應用程式提供驗證使用者身分和取得使用者中繼資料資訊的功能。

當串流應用程式需要播放內容時，「驗證階段」是「預先授權階段」或「授權階段」的先決條件步驟。

+++相關文章

API

* [建立驗證工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [繼續驗證工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [擷取驗證工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [在使用者代理程式中執行驗證](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [擷取設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [擷取特定mvpd的設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [擷取特定程式碼的設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

流程

* [主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

常見問答

* [驗證階段常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### 步驟2：檢查現有的已驗證設定檔 {#step-2-check-for-existing-authenticated-profiles}

* **擷取設定檔：**&#x200B;程式設計師服務會呼叫&#x200B;[**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)&#x200B;端點，代表串流應用程式檢查現有的設定檔。


* **案例1：**&#x200B;存在現有的設定檔，程式設計師服務可能會繼續進行[預先授權階段](#preauthorization-phase)或[授權階段](#authorization-phase)。


* **案例2：**&#x200B;沒有現有的設定檔，程式設計師服務可能會繼續執行下一個步驟，以[驗證使用者](#step-3-authenticate-the-user)。


* **案例3：**&#x200B;沒有現有的設定檔，程式設計師服務可能會繼續透過[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)功能為使用者提供暫存存取權。

   * 此情境超出本檔案的範圍，如需詳細資訊，請參閱[暫時存取流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)檔案。

### 步驟3：驗證使用者 {#step-3-authenticate-the-user}

* **擷取組態：**&#x200B;程式設計師服務會呼叫&#x200B;[**/api/v2/{serviceProvider}/組態**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)&#x200B;端點以擷取可用MVPD的清單。

   * 程式設計師服務可以實作自訂篩選機制，以精簡設定回應中的MVPD清單，讓串流應用程式只顯示預期的提供者，同時隱藏其他提供者（例如正在開發的MVPD、測試MVPD、TempPass）。 這可確保在選擇電視提供者時，會向使用者呈現已組織的選取專案。


* **建立驗證工作階段：**&#x200B;程式設計師服務會呼叫&#x200B;[**/api/v2/{serviceProvider}/sessions**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)&#x200B;端點來起始驗證工作階段。

   * 程式設計師服務必須將`code`和`url`傳回串流應用程式。


* **案例1：**&#x200B;串流應用程式可以開啟瀏覽器或webview，因此必須載入驗證`url`。

   * 使用者會在MVPD登入頁面中提交其使用者名稱和密碼。 成功驗證後，最終重新導向會顯示成功頁面。


* **案例2：**&#x200B;串流應用程式無法開啟瀏覽器，因此必須顯示驗證`code`。 需要個別的網頁應用程式來提示使用者輸入`code`、建構驗證`url`並開啟： [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)。

   * 使用者會在MVPD登入頁面中提交其使用者名稱和密碼。 成功驗證後，最終重新導向會顯示成功頁面。

### 步驟4：檢查已驗證的設定檔 {#step-4-check-for-authenticated-profiles}

* **擷取特定程式碼的設定檔：**&#x200B;程式設計師服務必須使用`code`實作輪詢機制，以透過呼叫&#x200B;[**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)端點來檢查設定檔是否成功產生及儲存。

   * 程式設計師服務必須在下列條件下&#x200B;**啟動輪詢**&#x200B;機制：

      * **在主要（熒幕）應用程式內執行的驗證：**&#x200B;當瀏覽器元件載入`redirectUrl`工作階段[端點要求中為](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)引數指定的URL後，程式設計師服務應在使用者到達最終目的地頁面時開始輪詢。

      * **在次要（熒幕）應用程式內執行的驗證：**&#x200B;程式設計師服務應用程式應在使用者起始驗證程式後立即開始輪詢 — 在收到[工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)端點回應並向使用者顯示驗證代碼之後。

   * 程式設計師服務必須在下列條件下&#x200B;**停止輪詢**&#x200B;機制：

      * **成功驗證：**&#x200B;已成功擷取使用者的設定檔資訊，確認其驗證狀態。 此時，不再需要輪詢。

      * **驗證工作階段和程式碼到期日：**&#x200B;驗證工作階段和程式碼會到期，如`notAfter`工作階段[端點回應中的](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)時間戳記（例如30分鐘）所指示。 如果發生這種狀況，使用者必須重新啟動驗證程式，而且使用先前驗證代碼的輪詢應該立即停止。

      * **產生的新驗證碼：**&#x200B;如果使用者要求主要（熒幕）裝置上的新驗證碼，則現有工作階段不再有效，使用先前驗證碼的輪詢應立即停止。

   * 程式設計師服務必須在下列條件下&#x200B;**設定輪詢**&#x200B;機制頻率：

      * **在主要（熒幕）應用程式內執行的驗證：**&#x200B;程式設計師服務應該每3-5秒或更長時間輪詢一次。

      * **在次要（熒幕）應用程式內執行的驗證：**&#x200B;程式設計師服務應該每3-5秒或更長時間輪詢一次。

   * 程式設計人員服務應將部分使用者設定檔資訊快取到永久性儲存體中，以避免不必要的請求並改善使用者體驗。

## C. （選擇性）預先授權階段 {#preauthorization-phase}

預先授權階段的目的是讓串流應用程式能夠呈現其目錄中使用者有權存取的資源子集。

預先授權階段可在使用者首次開啟串流應用程式或導覽至新區段時增強使用者體驗。

預先授權階段並非強制性，如果串流應用程式想要呈現資源目錄而不先根據使用者的許可權進行篩選，則可以跳過此階段。

+++相關文章

API

* [擷取預先授權決定](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

流程

* [主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

常見問答

* [預先授權階段常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### 步驟5：檢查預先授權的資源 {#step-5-check-for-preauthorized-resources}

* **擷取預先授權決定：**&#x200B;程式設計師服務會呼叫&#x200B;[**/api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)端點，以擷取資源清單的預先授權決定。

   * 程式設計人員服務必須將允許清單和拒絕預先授權決定傳遞給串流應用程式。

   * 將預先授權決定儲存在永久儲存體時，不需要Programmer Service。 但是，建議將允許決策快取到記憶體中以改善使用者體驗。 這有助於避免對已預先授權的資源發出不必要的呼叫，減少延遲並改善效能。

   * 程式設計師服務可透過檢查Decisions Preauthorize端點的回應中包含的[錯誤碼和訊息](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)，來判斷拒絕預先授權決定的原因。 這些詳細資料可為insight提供預先授權請求被拒絕的特定原因，有助於告知使用者體驗或觸發應用程式中的任何必要處理。 如果預先授權決定遭拒，請確定任何針對擷取預先授權決定所實作的重試機制，都不會導致無休止的回圈。 請考慮將重試限制在合理數字，並透過向使用者呈現清楚的意見反應來適當地處理拒絕。

   * 由於MVPD所強加的條件，程式設計師服務可以在單一API要求中，針對有限數量的資源取得預先授權決定，通常最多5個。 您的組織管理員或代表您行事的Adobe Pass驗證代表透過Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)同意MVPD後，可以檢視和變更此最大資源數量。

## D.授權階段 {#authorization-phase}

授權階段的用途是，在MVPD中驗證使用者的許可權後，為串流應用程式提供播放使用者要求的資源的功能。

授權階段是強制性的，如果串流應用程式想要播放使用者請求的資源，則無法跳過此階段，因為它需要在發佈串流之前向MVPD驗證使用者是否有權使用。

+++相關文章

API

* [擷取授權決定](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

流程

* [主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

常見問答

* [授權階段常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### 步驟6：檢查授權的資源 {#step-6-check-for-authorized-resources}

* **擷取授權決定：**&#x200B;程式設計師服務會呼叫&#x200B;[**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)端點，擷取串流應用程式所傳遞之特定資源的授權決定。

   * 將授權決定儲存在永久儲存體時，不需要Programmer Service。

   * 程式設計師服務可透過檢查包含在來自Decisions Authorize端點的回應中的[錯誤碼和訊息](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)，來判斷拒絕授權決定的原因。 這些詳細資料可為insight提供授權請求遭拒絕的特定原因，有助於告知使用者體驗或串流應用程式中觸發任何必要的處理。 如果授權決定遭拒，請確定任何針對擷取授權決定所實作的重試機制都不會導致無限回圈。 請考慮將重試限制在合理數字，並透過向使用者呈現清楚的意見反應來適當地處理拒絕。

   * 程式設計師服務可能會評估其他商業規則，並將適當的授權決定傳回串流應用程式。

   * 當串流正在播放時，不需要程式設計師服務來重新整理過期的媒體Token。 如果媒體權杖在播放期間到期，則應該允許資料流繼續而不中斷。 不過，使用者端在下次嘗試播放資源時，必須要求新的授權決定，並取得新的媒體代號。

   * 由於MVPD施加的條件，程式設計師服務可以在單一API要求中取得有限資源數量的授權決定，通常最多1個。

## E.登出階段 {#logout-phase}

登出階段的用途是讓串流應用程式能在使用者要求時，在Adobe Pass驗證中終止使用者的已驗證設定檔。

登出階段是強制性的，串流應用程式必須提供使用者登出的功能。

+++相關文章

API

* [啟動特定mvpd的登出](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

流程

* [主要應用程式內執行的基本登出流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

常見問答

* [登出階段常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### 步驟7：登出 {#step-7-logout}

* 起始Adobe Pass登出：程式設計人員服務會呼叫[/api/v2/{serviceProvider}/logout/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)端點，依串流應用程式要求啟動登出流程。

   * 程式設計師服務可能會清除其儲存的關於已驗證使用者的任何資訊。

   * 程式設計師服務必須遵循登出端點回應的`actionName`和`actionType`屬性中提供的指示，以確保登出程式正確完成。

      * 如果回應中的`actionType`屬性設為「互動式」，程式設計師服務必須將`url`屬性值傳回串流應用程式。

         * **案例1：**&#x200B;串流應用程式可以開啟瀏覽器或webview，因此必須載入登出`url`。

         * **情節2：**&#x200B;串流應用程式無法開啟瀏覽器，因此登出程式可以停止，因為MVPD工作階段並未保留在串流裝置瀏覽器快取中。
