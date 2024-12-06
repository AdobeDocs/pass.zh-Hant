---
title: 檢查驗證Token
description: 檢查驗證Token
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# 檢查驗證Token {#check-authentication-token}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!NOTE]
>
> REST API實作已由[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)限制

## REST API端點 {#clientless-endpoints}

&lt;REGGIE_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 說明 {#description}

指出裝置是否有未過期的驗證Token。

| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/checkauthn | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.要求者（必要）</br>2。  deviceId （必要）</br>3。  device_info/X-Device-Info （必要）</br>4。  _deviceType_ </br>5。  _deviceUser_ （已棄用）</br>6。  _appId_ （已棄用） | GET | XML或JSON包含失敗時的錯誤詳細資料。 | 200 — 成功   </br>403 — 無成功 |

{style="table-layout:auto"}


| 輸入引數 | 說明 |
| --- | --- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| device_info/</br></br>X-Device-Info | 串流裝置資訊。</br></br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數可能的大小以及GETURL的長度限制，應該在http標頭中作為X-Device-Info傳遞。</br></br><!--See the full details in [Passing Device and Connection Information](/help/authentication/passing-client-information-device-connection-and-application.md)(/help/authentication/passing-client-information-device-connection-and-application.md)-->。 |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br>若此引數設定正確，ESM提供的量度在使用Clienless時可依裝置型別](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)進行[劃分，因此可針對Roku、AppleTV、Xbox等執行不同型別的分析。</br></br>如需詳細資訊，請參閱[在Adobe Pass驗證度量中使用無使用者端deviceType引數的好處&#x200B;](/help/authentication/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**注意**： device_info將取代此引數。 |
| _deviceUser_ | 裝置使用者識別碼。 |
| _appId_ | 應用程式id/名稱。</br>**注意**： device_info會取代此引數。 |

{style="table-layout:auto"}


## 回應（若不成功） {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [返回REST API參考](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
