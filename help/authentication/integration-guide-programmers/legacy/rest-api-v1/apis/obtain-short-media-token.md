---
title: 取得短媒體Token
description: 取得短媒體權杖
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 0%

---

# （舊版）取得短媒體Token {#obtain-short-media-token}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

>[!NOTE]
>
> REST API實作已由[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)限制

## REST API端點 {#clientless-endpoints}

&lt;REGGIE_FQDN>：

* 生產 — [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* 正在暫存 — [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>：

* 生產 — [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* 正在暫存 — [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

</br>

## 說明 {#description}

取得短媒體Token。

| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/mediatoken</br></br>或</br></br>&lt;SP_FQDN>/api/v1/tokens/media</br></br>例如：</br></br>&lt;SP_FQDN>/api/v1/tokens/media | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.要求者（必要）</br>2。  deviceId （必要）</br>3。  資源（必要）</br>4。  device_info/X-Device-Info （必要）</br>5。  _deviceType_</br> 6。  _deviceUser_ （已棄用）</br>7。  _appId_ （已棄用） | GET | 包含Base64編碼媒體權杖的XML或JSON，或如果失敗則包含錯誤詳細資料。 | 200 — 成功</br>403 — 無成功 |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| 輸入引數 | 說明 |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| resource | 包含resourceId （或MRSS片段）的字串，可識別使用者請求的內容並由MVPD授權端點識別。 |
| device_info/</br></br>X-Device-Info | 串流裝置資訊。</br></br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數的潛在大小以及GETURL長度的限制，它應該作為X-Device-Info傳遞到http標頭。 </br></br>檢視[傳遞裝置和連線資訊](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)中的完整詳細資料。 |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br>若此引數設定正確，ESM提供的量度在使用Clienless時可依裝置型別&rbrack;/(help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type)進行&lbrack;劃分，因此可針對執行不同型別的分析。 例如，Roku、AppleTV和Xbox。</br></br>檢視[使用無使用者端devicetype引數的好處&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**： device_info將會取代此引數。 |
| _deviceUser_ | 裝置使用者識別碼。</br></br>**注意**：若使用，deviceUser的值應該與[建立註冊代碼](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求中的值相同。 |
| _appId_ | 應用程式id/名稱。 </br></br>**注意**： device_info會取代此引數。 若已使用，`appId`應具有與[建立註冊代碼](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求中相同的值。 |

{style="table-layout:auto"}

### 範例回應 {#response}

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <play>
        <expires>1348649569000</expires>
        <mvpdId>sampleMvpdId</mvpdId>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <serializedToken>PHNpZ25hdH[...]</serializedToken>
        <userId>sampleScrambledUserId</userId>
    </play>
```



**JSON：**

```JSON
    {
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348649614000",
        "serializedToken": "PHNpZ25hdH[...]",
        "userId": "sampleScrambledUserId",
        "mvpdId": "sampleMvpdId"
    }
```



### 媒體驗證程式庫相容性

「取得短媒體Token」呼叫的欄位`serializedToken`是Base64編碼的Token，可根據Adobe Medium驗證程式庫進行驗證。
