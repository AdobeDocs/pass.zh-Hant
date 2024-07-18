---
title: 擷取授權Token
description: 擷取授權Token
exl-id: 0b010958-efa8-4dd9-b11b-5d10f51f5680
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# 擷取授權Token {#retrieve-authorization-token}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!NOTE]
>
> REST API實作已由[節流機制](/help/authentication/throttling-mechanism.md)限制

## REST API端點 {#clientless-endpoints}

&lt;REGGIE_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 說明 {#description}

擷取授權(AuthZ) Token。


| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authz</br></br>例如：</br></br>&lt;SP_FQDN>/api/v1/tokens/authz | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.要求者（必要）</br>2。  deviceId （必要）</br>3。  資源（必要）</br>4。  device_info/X-Device-Info （必要）</br>5。  _deviceType_</br> 6。  _deviceUser_ （已棄用）</br>7。  _appId_ （已棄用） | GET | 1.成功</br>2。  驗證Token </br>    找不到或已過期：   </br>    XML說明原因</br>    找不到authn權杖的</br>3。  授權權杖</br>    找不到： </br>    XML說明</br>4。  授權權杖</br>    已過期： </br>    XML說明 | 200 — 成功</br>412 — 無AuthN</br></br>404 — 無AuthZ</br></br>410 - AuthZ已過期 |

{style="table-layout:auto"}

</br>

| 輸入引數 | 說明 |
| --- | --- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| resource | 包含resourceId （或MRSS片段）的字串，可識別使用者要求的內容並由MVPD授權端點識別。 |
| device_info/</br></br>X-Device-Info | 串流裝置資訊。</br></br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數的潛在大小以及GETURL長度的限制，它應該作為X-Device-Info傳遞到http標頭。 </br></br>檢視[傳遞裝置和連線資訊](/help/authentication/passing-client-information-device-connection-and-application.md)中的完整詳細資料。 |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br>若此引數設定正確，ESM提供的量度在使用Clienless時，會針對每個裝置型別](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type)進行[劃分，因此可以執行不同型別的分析，例如Roku、AppleTV和Xbox。</br></br>檢視，[在傳遞量度中使用無使用者端裝置型別引數的好處&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**： device_info將會取代此引數。 |
| _deviceUser_ | 裝置使用者識別碼。 |
| _appId_ | 應用程式id/名稱。 </br></br>**注意**： device_info會取代此引數。 |

{style="table-layout:auto"}


### 範例回應 {#response}



#### 成功

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authorization>
        <expires>1348148289000</expires>
        <mvpd>sampleMvpdId</mvpd>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <proxyMvpd>sampleProxyMvpdId</proxyMvpd>
    </authorization>
```



**JSON：**

```JSON
    {
        "mvpd": "sampleMvpdId",
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348148289000",
        "proxyMvpd": "sampleProxyMvpdId"
    }
```

</br>


#### 找不到驗證權杖或已過期：

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>412</status>
        <message>User not authenticated</message>
    </error>
```



**JSON：**

```JSON
    {
        "status": 412,
        "message": "User not authenticated",
        "details": null
    }
```

</br>


#### 找不到授權Token：

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>404</status>
        <message>Not found</message>
    </error>
```



**JSON：**

```JSON
    {
        "status": 404,
        "message": "Not Found",
        "details": null
    }
```

</br>



#### 授權權杖已過期：

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>410</status>
        <message>Gone</message>
    </error>
```



**JSON：**

```JSON
    {
        "status": 410,
        "message": "Gone",
        "details": null
    }
```
