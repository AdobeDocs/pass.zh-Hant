---
title: 擷取驗證Token
description: 擷取驗證Token
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# 擷取驗證Token {#retrieve-authentication-token}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## REST API端點 {#clientless-endpoints}

&lt;reggie_fqdn>：

* 生產 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 分段 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>：

* 生產 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 分段 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 說明 {#description}

擷取驗證(AuthN) Token。

| 端點 | 已呼叫  </br>作者： | 輸入   </br>引數 | HTTP  </br>方法 | 回應 | HTTP  </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/tokens/authn</br></br>例如：</br></br>&lt;sp_fqdn>/api/v1/tokens/authn | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.請求者（必要）</br>2.  deviceId （必要）</br>3.  device_info/X-Device-Info （必要）</br>4.  _deviceType_ （已棄用）</br>5.  _deviceuser_ （已棄用）</br>6.  _appId_ （已棄用） | GET | XML或JSON，其中包含驗證資訊或失敗時的錯誤詳細資料。 | 200 — 成功。  </br>404 — 找不到Token  </br>410 - Token已過期 |

{style="table-layout:auto"}


| 輸入引數 | 說明 |
| --- | --- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| device_info/</br></br>X-Device-Info | 串流裝置資訊。</br></br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數潛在的大小以及GETURL長度的限制，應該在http標頭中作為X-Device-Info傳遞。 </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br>**注意**：device_info會取代此引數。 |
| _deviceuser_ | 裝置使用者識別碼。</br></br>**注意**：若使用，deviceUser的值應與中的相同 [建立註冊代碼](/help/authentication/registration-code-request.md) 要求。 |
| _appId_ | 應用程式id/名稱。 </br></br>**注意**：device_info會取代此引數。 若已使用， `appId` 的值應該與 [建立註冊代碼](/help/authentication/registration-code-request.md) 要求。 |

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
