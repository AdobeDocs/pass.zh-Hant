---
title: 取得短媒體Token
description: 取得短媒體權杖
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# 取得短媒體Token {#obtain-short-media-token}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!NOTE]
>
> REST API實作受限於 [節流機制](/help/authentication/throttling-mechanism.md)

## REST API端點 {#clientless-endpoints}

&lt;reggie_fqdn>：

* 生產 —  [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* 分段 —  [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>：

* 生產 —  [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* 分段 —  [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

</br>

## 說明 {#description}

取得短媒體Token。

| 端點 | 已呼叫  </br>作者： | 輸入   </br>引數 | HTTP  </br>方法 | 回應 | HTTP  </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/mediatoken</br></br>  或</br></br>&lt;sp_fqdn>/api/v1/tokens/media</br></br>例如：</br></br>&lt;sp_fqdn>/api/v1/tokens/media | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.請求者（必要）</br>2.  deviceId （必要）</br>3.  resource （必要）</br>4.  device_info/X-Device-Info （必要）</br>5.  _deviceType_</br> 6.  _deviceuser_ （已棄用）</br>7.  _appId_ （已棄用） | GET | 包含Base64編碼媒體權杖的XML或JSON，或如果失敗則包含錯誤詳細資料。 | 200 — 成功  </br>403 — 無成功 |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| 輸入引數 | 說明 |
| --- | --- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| resource | 包含resourceId （或MRSS片段）的字串，可識別使用者要求的內容並由MVPD授權端點識別。 |
| device_info/</br></br>X-Device-Info | 串流裝置資訊。</br></br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數潛在的大小以及GETURL長度的限制，應該在http標頭中作為X-Device-Info傳遞。 </br></br>如需詳細資訊，請參閱 [傳遞裝置和連線資訊](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br>若此引數設定正確，ESM提供的量度會 [依裝置型別劃分]/(help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type)使用無使用者端時，因此可針對執行不同型別的分析。 例如，Roku、AppleTV和Xbox。</br></br>另請參閱 [使用無使用者端裝置型別引數的好處&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**：device_info會取代此引數。 |
| _deviceuser_ | 裝置使用者識別碼。</br></br>**注意**：若使用，deviceUser的值應與中的相同 [建立註冊代碼](/help/authentication/registration-code-request.md) 要求。 |
| _appId_ | 應用程式id/名稱。 </br></br>**注意**：device_info會取代此引數。 若已使用， `appId` 的值應該與 [建立註冊代碼](/help/authentication/registration-code-request.md) 要求。 |

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

欄位 `serializedToken` 從「取得短媒體權杖」呼叫是Base64編碼權杖，可根據Adobe Medium驗證程式庫進行驗證。
