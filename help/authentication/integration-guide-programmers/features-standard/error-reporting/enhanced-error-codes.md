---
title: 增強的錯誤碼
description: 增強的錯誤碼
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2606'
ht-degree: 2%

---

# 增強的錯誤碼 {#enhanced-error-codes}

>[!IMPORTANT]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

增強的「錯誤碼」代表Adobe Pass驗證功能，可向整合以下專案的使用者端應用程式提供其他錯誤資訊：

* Adobe Pass驗證REST API：
   * [REST API v1](../../legacy/rest-api-v1/apis/rest-api-overview.md)
   * [REST API v2](../../rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
* Adobe Pass驗證SDK預先授權API：
   * [JavaScript SDK （預先授權API）](../../legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
   * [iOS/tvOS SDK （預先授權API）](../../legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
   * [Android SDK （預先授權API）](../../legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)

  _(*) Preauthorize API是唯一支援增強型錯誤碼的Adobe Pass Authentication SDK API。_

>[!IMPORTANT]
>
> 整合Adobe Pass驗證REST API v2的應用程式依預設將受益於增強錯誤碼，而無需額外設定。
>
> <br/>
>
> 整合Adobe Pass驗證REST API v1或SDK預先授權API的應用程式只有在明確啟用此功能時，才能受益於增強型錯誤代碼。
>
> <br/>
>
> 若要明確啟用此功能，請透過我們的[Zendesk](https://adobeprimetime.zendesk.com)建立票證，並向您的技術客戶經理(TAM)尋求協助。

## 表示 {#enhanced-error-codes-representation}

根據整合的Adobe Pass驗證API和使用的「接受」標頭值（亦即`application/json`或`application/xml`），增強型錯誤代碼可以以`JSON`或`XML`格式表示：

| Adobe Pass驗證API | JSON | XML |
|-------------------------------|---------|---------|
| REST API v1 | &amp;amp；檢查； | &amp;amp；檢查； |
| REST API v2 | &amp;amp；檢查； |         |
| SDK預先授權API | &amp;amp；檢查； |         |

>[!IMPORTANT]
>
> Adobe Pass驗證可以透過兩種表單將增強型錯誤代碼傳達給使用者端應用程式：
>
> <br/>
>
> * **最上層錯誤資訊**：在此情況下，***&quot;error&quot;***&#x200B;物件位於最上層，因此回應本文只能包含&#x200B;***&quot;error&quot;***&#x200B;物件。
> * **專案層級錯誤資訊**：在此情況下，***&quot;error&quot;***&#x200B;物件位於專案層級，因此回應內文可能會針對在服務時發生錯誤的所有專案包含&#x200B;***&quot;error&quot;***&#x200B;物件。
>
> <br/>
>
> 請檢視每個整合式Adobe Pass驗證API的公開檔案，以決定增強型錯誤代碼表示細節。

請參閱下列HTTP回應，其中包含以`JSON`或`XML`表示的增強型錯誤碼範例。

>[!BEGINTABS]

>[!TAB REST API v1 — 最上層錯誤資訊(JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "8bcb17f9-b172-47d2-86d9-3eb146eba85e"
}
```

>[!TAB REST API v1 — 最上層錯誤資訊(XML)]

```XML
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<error>
  <action>none</action>
  <status>400</status>
  <code>invalid_requestor</code>
  <message>The requestor parameter is missing or invalid.</message>
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!TAB REST API v1 — 專案層級錯誤資訊(JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resources": [
    {
      "id": "TestStream1",
      "authorized": true
    },
    {
      "id": "TestStream2",
      "authorized": false,
      "error": {
        "action": "retry",
        "status": 403,
        "code": "network_connection_failure",
        "message": "Unable to contact your TV provider services",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v2 — 最上層錯誤資訊(JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "action": "none",
  "status": 400,
  "code": "invalid_parameter_service_provider",
  "message": "The service provider parameter value is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!TAB REST API v2 — 專案層級錯誤資訊(JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "decisions": [
    {
      "resource": "REF30",
      "serviceProvider": "REF30",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": true,
      "token": {
        "issuedAt": 1697094207324,
        "notBefore": 1697094207324,
        "notAfter": 1697094802367,
        "serializedToken": "PHNpZ25hdHVyZUluZm8..."
      }
    },
    {
      "resource": "REF40",
      "serviceProvider": "REF40",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": false,
      "error" : {
        "action": "retry",
        "status": 403,
        "code": "network_connection_failure",
        "message": "Unable to contact your TV provider services",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!ENDTABS]

增強型錯誤碼包含下列`JSON`欄位或`XML`屬性：

| 名稱 | 型別 | 範例 | 受限制 | 說明 |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *動作* | *字串* | *重試* | &amp;amp；檢查； | Adobe Pass驗證建議可修正此檔案中定義的狀況的動作。 <br/><br/>如需詳細資訊，請參閱[動作](#enhanced-error-codes-action)區段。 |
| *狀態* | *整數* | *403* | &amp;amp；檢查； | 在[RFC 7231](https://tools.ietf.org/html/rfc7231#section-6)檔案中定義的HTTP回應狀態碼。 <br/><br/>如需詳細資訊，請參閱[狀態](#enhanced-error-codes-status)區段。 |
| *代碼* | *字串* | *network_connection_failure* | &amp;amp；檢查； | 與本檔案定義之錯誤相關聯的Adobe Pass驗證唯一識別碼代碼。 <br/><br/>如需詳細資訊，請參閱[代碼](#enhanced-error-codes-code)區段。 |
| *訊息* | *字串* | *無法連絡您的電視提供者服務* |            | 在某些情況下可顯示給一般使用者的人類可讀訊息。 <br/><br/>如需詳細資訊，請參閱[回應處理](#enhanced-error-codes-response-handling)區段。 |
| *詳細資料* | *字串* | *您的訂閱套件不包含「即時」頻道* |            | 服務合作夥伴在某些情況下可能提供的詳細訊息，<br/><br/>若服務合作夥伴未提供任何自訂訊息，此欄位可能不存在。 |
| *helpUrl* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html* |            | Adobe Pass驗證公開檔案URL可連結至此錯誤發生原因及可能解決方案的相關資訊。 <br/><br/>此欄位包含絕對URL，不應從錯誤碼推斷，視錯誤內容而定，可以提供不同的URL。 |
| *追蹤* | *字串* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | 回應的唯一識別碼，可在聯絡Adobe Pass驗證支援以疑難排解特定問題時使用。 |

>[!IMPORTANT]
>
> **Restricted**&#x200B;欄指出個別欄位是否保留有限集的值，而無限制的欄位可包含任何資料。
>
> <br/>
>
> 此檔案的未來更新可能會將值新增至有限集，但不會移除或變更現有值。

### 動作 {#enhanced-error-codes-representation-action}

增強的「錯誤碼」包含「action」欄位，此欄位提供可修正此情況的建議動作。

「動作」欄位可能的值包括：

| 動作 | 說明 | 類別 |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| 無 | 沒有預先定義的動作可修正此問題，但在某些情況下，這可能表示API的呼叫不正確。 | 修正請求內容。 |
| 設定 | 使用者端應用程式需要變更設定，大部分時間都會透過Adobe Pass TVE Dashboard進行。 | 修正整合設定內容。 |
| application-register | 使用者端應用程式需要再次登入本身。 | 修正使用者端應用程式內容。 |
| authentication | 使用者端應用程式需要驗證或重新驗證使用者。 | 修正使用者端應用程式內容。 |
| authorization | 使用者端應用程式需要取得指定資源的授權。 | 修正使用者端應用程式內容。 |
| 重試 | 使用者端應用程式需要重試要求。 | 修正請求內容。 |

_(*)對於某些錯誤，多個動作可能是可能的解決方案，但「action」欄位表示修正錯誤的可能性最高的動作。_

### 狀態 {#enhanced-error-codes-representation-status}

增強型錯誤碼包含一個「狀態」欄位，可指示與錯誤相關聯的HTTP狀態代碼。

「狀態」欄位可能的值包括：

| 程式碼 | 原因短語 |
|------|-----------------------|
| 400 | 錯誤請求 |
| 401 | 未獲授權 |
| 403 | 已禁止 |
| 404 | 找不到 |
| 405 | 不允許的方法 |
| 410 | 已過期 |
| 412 | 先決條件失敗 |
| 500 | 內部伺服器錯誤 |

當使用者端產生錯誤，而且大部分時間表示使用者端需要額外的工作來修復錯誤時，通常會出現具有4xx「狀態」的增強型錯誤代碼。

當伺服器產生錯誤且大部分時間表示伺服器需要額外的工作來修復錯誤時，通常會出現具有5xx「狀態」的增強錯誤碼。

>[!IMPORTANT]
>
> 有時HTTP回應狀態代碼與增強型錯誤代碼「狀態」欄位不同，尤其是與將增強型錯誤代碼當作專案層級錯誤資訊傳達的Adobe Pass驗證API互動時。

### 程式碼 {#enhanced-error-codes-representation-code}

增強的「錯誤代碼」包含「代碼」欄位，此欄位提供與錯誤相關的Adobe Pass驗證唯一識別碼。

根據整合的Adobe Pass驗證API，「代碼」欄位可能的值在兩個清單中彙總在[以下](#enhanced-error-codes-list)。

## 清單 {#enhanced-error-codes-lists}

### REST API v1 {#enhanced-error-codes-lists-rest-api-v1}

下表列出使用者端應用程式在與Adobe Pass驗證REST API v1整合時可能遇到的增強型錯誤代碼。

| 動作 | 程式碼 | 狀態 | 訊息 |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **無** | *invalid_requestor* | 400 | 要求者引數遺失或無效。 |
|                    | *invalid_device_info* | 400 | 裝置資訊遺失或無效。 |
|                    | *invalid_device_id* | 400 | 裝置識別碼遺失或無效。 |
|                    | *missing_resource* | 400， 412 | 缺少資源引數。 |
|                    | *格式錯誤的authz_request* | 400， 412 | 授權要求為空值或無效。 |
|                    | *preauthorization_denied_by_mvpd* | 403 | MVPD在要求指定資源的預先授權時傳回「拒絕」決定。 |
|                    | *authorization_denied_by_mvpd* | 403 | MVPD在要求指定資源的授權時傳回「拒絕」決定。 |
|                    | *authorization_denied_by_parental_controls* | 403 | MVPD因為指定資源的家長監護設定，而傳回「拒絕」決定。 |
|                    | *內部錯誤* | 400， 405， 500 | 由於內部伺服器錯誤，請求失敗。 |
| **組態** | *未知的整合* | 400， 412 | 指定的程式設計師和身分提供者之間的整合不存在。 使用TVE Dashboard建立所需的整合。 |
|                    | *太多資源* | 403 | 授權或預先授權要求失敗，因為查詢的資源太多。 請聯絡支援團隊，以正確設定授權和預先授許可權制。 |
| **驗證** | *authentication_session_issuer_mismatch* | 400 | 授權要求失敗，因為授權流程所指示的MVPD與發出驗證工作階段的MVPD不同。 使用者必須使用所需的MVPD重新驗證才能繼續。 |
|                    | *authorization_denied_by_hba_policies* | 403 | MVPD因為家用驗證原則而傳回「拒絕」決定。 目前的驗證是使用家用驗證流程(HBA)取得的，但裝置在請求指定資源的授權時已不在家中。 使用者必須使用支援的MVPD重新驗證才能繼續。 |
|                    | *authorization_denied_by_session_invalidated* | 403 | 身分提供者已使驗證工作階段失效。 使用者必須使用支援的MVPD重新驗證才能繼續。 |
|                    | *identity_not_recovered_by_mvpd* | 403 | 授權要求失敗，因為MVPD無法辨識使用者身分。 |
|                    | *authentication_session_invalidated* | 403 | 身分提供者已使驗證工作階段失效。 使用者必須使用支援的MVPD重新驗證才能繼續。 |
|                    | *authentication_session_missing* | 403， 412 | 無法擷取與此要求關聯的驗證工作階段。 使用者必須使用支援的MVPD重新驗證才能繼續。 |
|                    | *authentication_session_expired* | 403， 412 | 目前的驗證工作階段已過期。 使用者必須使用支援的MVPD重新驗證才能繼續。 |
|                    | *preauthorization_authentication_session_missing* | 412 | 無法擷取與此要求關聯的驗證工作階段。 使用者必須使用支援的MVPD重新驗證才能繼續。 |
|                    | *preauthorization_authentication_session_expired* | 412 | 目前的驗證工作階段已過期。 使用者必須使用支援的MVPD重新驗證才能繼續。 |
| **授權** | *authorization_not_found* | 403， 404 | 找不到指定資源的授權。 使用者必須取得新授權才能繼續。 |
|                    | *authorization_expired* | 410 | 指定資源的先前授權已過期。 使用者必須取得新授權才能繼續。 |
| **重試** | *network_received_error* | 403 | 從關聯的合作夥伴服務擷取回應時發生讀取錯誤。 重試請求可能會解決問題。 |
|                    | *network_connection_timeout* | 403 | 與關聯的合作夥伴服務發生連線逾時。 重試請求可能會解決問題。 |
|                    | *excepted_execution_time_exceeded* | 403 | 請求未在允許的最長時間內完成。 重試請求可能會解決問題。 |

### SDK預先授權API {#enhanced-error-codes-lists-sdks-preauthorize-api}

請參閱前[節](#enhanced-error-codes-list-rest-api-v1)，瞭解使用者端應用程式在與Adobe Pass Authentication SDK整合時，預先授權API可能會遇到的可能增強型錯誤碼。

### REST API v2 {#enhanced-error-codes-lists-rest-api-v2}

下表列出使用者端應用程式在與Adobe Pass Authentication REST API v2整合時可能遇到的增強型錯誤代碼。

| 動作 | 程式碼 | 狀態 | 訊息 |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **無** | *invalid_parameter_service_provider* | 400 | 服務提供者引數值遺失或無效。 |
|                              | *invalid_parameter_mvpd* | 400 | mvpd引數值遺失或無效。 |
|                              | *invalid_parameter_code* | 400 | 程式碼引數值遺失或無效。 |
|                              | *invalid_parameter_resources* | 400 | 資源引數值遺失或無效。 |
|                              | *invalid_parameter_redirect_url* | 400 | 遺失或無效的重新導向URL引數值。 |
|                              | *invalid_parameter_partner* | 400 | 夥伴引數值遺失或無效。 |
|                              | *invalid_parameter_saml_response* | 400 | SAML回應引數值遺失或無效。 |
|                              | *invalid_header_device_info* | 400 | 裝置資訊標頭值遺失或無效。 |
|                              | *invalid_header_device_identifier* | 400 | 裝置識別碼標頭值遺失或無效。 |
|                              | *invalid_header_identity_for_temporary_access* | 400 | 暫時存取標頭值的身分識別遺失或無效。 |
|                              | *invalid_header_pfs_permission_access_not_present* | 400 | 來自合作夥伴框架狀態標頭的許可權存取狀態值不存在。 |
|                              | *invalid_header_pfs_permission_access_not_determined* | 400 | 來自合作夥伴框架狀態標頭的許可權存取狀態值未決定。 |
|                              | *invalid_header_pfs_permission_access_not_granted* | 400 | 未授予來自合作夥伴框架狀態標頭的許可權存取狀態值。 |
|                              | *invalid_header_pfs_provider_id_not_determined* | 400 | 合作夥伴架構狀態標頭中的提供者ID值與已知的mvpd沒有關聯。 |
|                              | *invalid_header_pfs_provider_id_mismatch* | 400 | 合作夥伴架構狀態標頭中的提供者ID值與以引數形式傳送的mvpd不相符。 |
|                              | *invalid_header_pfs_provider_info_expired* | 400 | 夥伴架構狀態標頭中的提供者資訊已過期。 |
|                              | *無效整合* | 400 | 指定的服務提供者與mvpd之間的整合不存在或已停用。 |
|                              | *invalid_authentication_session* | 400 | 與此請求關聯的驗證工作階段遺失或無效。 |
|                              | *preauthorization_denied_by_mvpd* | 403 | MVPD在要求指定資源的預先授權時傳回「拒絕」決定。 |
|                              | *authorization_denied_by_mvpd* | 403 | MVPD在要求指定資源的授權時傳回「拒絕」決定。 |
|                              | *authorization_denied_by_parental_controls* | 403 | MVPD因為指定資源的家長監護設定，而傳回「拒絕」決定。 |
|                              | *authorization_denied_by_degradation_rule* | 403 | 指定的服務提供者與mvpd之間的整合已套用降級規則，該規則會拒絕對所要求資源的授權。 |
|                              | *內部伺服器錯誤* | 500 | 由於內部伺服器錯誤，請求失敗。 |
| **組態** | *太多資源* | 403 | 授權或預先授權要求失敗，因為查詢的資源太多。 請聯絡支援團隊，以正確設定授權和預先授許可權制。 |
|                              | *invalid_configuration_user_metadata_certificate* | 500 | 使用者中繼資料憑證設定遺失或無效。 |
|                              | *invalid_configuration_temporary_access* | 500 | 暫時存取設定無效。 |
|                              | *invalid_configuration_platform* | 500 | 整合缺少平台設定或平台設定無效。 |
|                              | *invalid_configuration_platform_id* | 500 | 平台ID設定遺失或無效。 |
|                              | *invalid_configuration_platform_trait* | 500 | 平台特徵設定遺失或無效。 |
|                              | *invalid_configuration_platform_category_trait* | 500 | 平台類別特徵設定遺失或無效。 |
|                              | *invalid_configuration_platform_services* | 500 | 整合的平台服務設定遺失或無效。 |
|                              | *invalid_configuration_mvpd_platform* | 500 | mvpd和平台的mvpd平台設定遺失或無效。 |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | mvpd和平台的mvpd平台登入狀態設定遺失或無效。 |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | mvpd和平台的mvpd平台設定檔交換設定遺失或無效。 |
| **應用程式註冊** | *invalid_access_token_service_provider* | 401 | 由於無效的服務提供者，存取權杖無效。 |
|                              | *invalid_access_token_client_application* | 401 | 由於無效的使用者端應用程式，存取權杖無效。 |
| **驗證** | *authenticated_profile_missing* | 403 | 缺少與此請求關聯的已驗證設定檔。 |
|                              | *authenticated_profile_expires* | 403 | 與此要求關聯的已驗證設定檔已過期。 |
|                              | *authenticated_profile_invalid* | 403 | 與此請求相關聯的已驗證設定檔已失效。 |
|                              | *temporary_access_duration_limit_exceeded* | 403 | 已超過暫時存取持續時間限制。 |
|                              | *temporary_access_resources_limit_exceeded* | 403 | 已超過暫時存取資源限制。 |
|                              | *authorization_denied_by_hba_policies* | 403 | MVPD因為家用驗證原則而傳回「拒絕」決定。 目前的驗證是透過家用驗證流程取得，但裝置在請求指定資源的授權時已不在家中。 使用者必須使用支援的MVPD重新驗證才能繼續。 |
|                              | *authorization_denied_by_session_invalidated* | 403 | 身分提供者已使驗證工作階段失效。 使用者必須使用支援的MVPD重新驗證才能繼續。 |
|                              | *identity_not_recovered_by_mvpd* | 403 | 授權要求失敗，因為MVPD無法辨識使用者身分。 |
| **重試** | *network_received_error* | 403 | 從關聯的合作夥伴服務擷取回應時發生讀取錯誤。 重試請求可能會解決問題。 |
|                              | *network_connection_timeout* | 403 | 與關聯的合作夥伴服務發生連線逾時。 重試請求可能會解決問題。 |
|                              | *excepted_execution_time_exceeded* | 403 | 請求未在允許的最長時間內完成。 重試請求可能會解決問題。 |

## 回應處理 {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> 使用者端應用程式程式碼可自動處理增強型錯誤碼，例如網路逾時時時重試授權要求，或要求使用者在工作階段過期時重新驗證，但其他型別可能需要變更設定，或Adobe Pass驗證客戶服務團隊互動。
>
> <br/>
>
> 因此，透過我們的[Zendesk](https://adobeprimetime.zendesk.com)建立票證時，務必收集並提供完整的錯誤資訊，以確保在啟動新應用程式或新功能之前進行必要的變更。

簡而言之，處理包含增強型錯誤碼的回應時，您應考量下列事項：

1. **檢查兩個狀態值**：一律檢查HTTP回應狀態碼和增強錯誤碼「狀態」欄位。 兩者可能有所不同，而且都提供有價值的資訊。

1. **無法辨識最上層與專案層級的錯誤資訊**：處理最上層與專案層級的錯誤資訊，無法辨識其傳達方式，請確定您可以處理兩種傳輸增強型錯誤碼的形式。

1. **重試邏輯**：對於需要重試的錯誤，請確定重試是以指數回溯完成，以避免讓伺服器不知所措。 此外，如果是Adobe Pass驗證API同時處理多個專案（例如，預先授權API），您應在重複請求中僅包含標籤為「重試」的專案，而非整個清單。

1. **組態變更**：對於需要組態變更的錯誤，請確保在啟動新應用程式或新功能之前進行必要的變更。

1. **驗證和授權**：對於與驗證和授權相關的錯誤，您必須視需要提示使用者重新驗證或取得新的授權。

1. **使用者意見反應**：選擇性，使用人類可讀的「訊息」和（可能的）「詳細資料」欄位，通知使用者此問題。 「詳細資料」文字訊息可能會從MVPD預先授權或授權端點傳遞，或在套用降級規則時從程式設計師傳遞。
