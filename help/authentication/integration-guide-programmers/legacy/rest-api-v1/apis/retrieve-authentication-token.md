---
title: 擷取驗證Token
description: 擷取驗證Token
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# （舊版）擷取驗證Token {#retrieve-authentication-token}

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

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 說明 {#description}

擷取驗證(AuthN) Token。

| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn</br></br>例如：</br></br>&lt;SP_FQDN>/api/v1/tokens/authn | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.要求者（必要）</br>2。  deviceId （必要）</br>3。  device_info/X-Device-Info （必要）</br>4。  _deviceType_ （已棄用）</br>5。  _deviceUser_ （已棄用）</br>6。  _appId_ （已棄用） | GET | XML或JSON，其中包含驗證資訊或失敗時的錯誤詳細資料。 | 200 — 成功。  </br>404 — 找不到Token </br>410 - Token已過期 |

{style="table-layout:auto"}


| 輸入引數 | 說明 |
| --- | --- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| device_info/</br></br>X-Device-Info | 串流裝置資訊。</br></br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數的潛在大小以及GET URL長度的限制，它應該作為X-Device-Info傳遞到http標頭。 </br></br>檢視[傳遞裝置和連線資訊](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)中的完整詳細資料。 |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br>**注意**： device_info會取代此引數。 |
| _deviceUser_ | 裝置使用者識別碼。</br></br>**注意**：若使用，deviceUser的值應該與[建立註冊代碼](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求中的值相同。 |
| _appId_ | 應用程式id/名稱。 </br></br>**注意**： device_info會取代此引數。 若已使用，`appId`應具有與[建立註冊代碼](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求中相同的值。 |

{style="table-layout:auto"}

</br>

### 範例回應 {#response}



#### 成功

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authentication>
         <expires>1601114932000</expires>
         <userId>sampleUserId</userId>
         <mvpd>sampleMvpdId</mvpd>
         <requestor>sampleRequestor</requestor>
    </authentication>
```


**JSON：**

```JSON
    {
         "requestor": "sampleRequestor",
         "mvpd": "sampleMvpdId",
         "userId": "sampleUserId",
         "expires": "1601114932000"
    }
```





#### 找不到驗證Token：

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
        "message": "Not Found"
    }
```
