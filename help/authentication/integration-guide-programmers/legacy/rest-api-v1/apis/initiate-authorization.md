---
title: 啟動授權
description: 啟動授權
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# （舊版）啟動授權 {#initiate-authorization}

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

取得授權回應。

| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authorize | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.要求者（必要）</br>2。  deviceId （必要）</br>3。  資源（必要）</br>4。  device_info/X-Device-Info （必要）</br>5。  _deviceType_</br> 6。  _deviceUser_ （已棄用）</br>7。  _appId_ （已棄用）</br>8。  額外引數（選擇性） | GET | XML或JSON包含授權詳細資訊，或如果失敗則包含錯誤詳細資訊。 請參閱下列範例。 | 200 — 成功</br>403 — 無成功 |

{style="table-layout:auto"}

</br>


| 輸入引數 | 說明 |
| --- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| resource | 包含resourceId （或MRSS片段）的字串，可識別使用者請求的內容並由MVPD授權端點識別。 |
| device_info/</br></br>X-Device-Info | 串流裝置資訊。</br></br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數的潛在大小以及GET URL長度的限制，它應該作為X-Device-Info傳遞到http標頭。 </br></br>檢視[傳遞裝置和連線資訊](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)中的完整詳細資料。 |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br>若此引數設定正確，ESM提供的量度在使用Clienless時可依裝置型別[進行](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)劃分，因此可針對Roku、AppleTV、Xbox等執行不同型別的分析。</br></br>檢視傳遞量度中無使用者端裝置型別引數的[優點&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**： device_info將取代此引數。 |
| _deviceUser_ | 裝置使用者識別碼。 |
| _appId_ | 應用程式id/名稱。 </br></br>**注意**： device_info會取代此引數。 |
| 額外的引數 | 呼叫也可能包含啟用其他功能的選用引數，例如：</br></br>* generic_data — 可啟用[促銷臨時傳遞](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)</br></br>範例： `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**串流裝置IP位址**</br>
>對於使用者端對伺服器實作，串流裝置IP位址會與此呼叫一併隱含傳送。  對於伺服器對伺服器實作（其中由程式設計師服務而不是串流裝置進行&#x200B;**regcode**&#x200B;呼叫），需要以下標題才能傳遞串流裝置IP位址： </br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>其中`<streaming\_device\_ip>`是串流裝置公用IP位址。</br></br>
>範例：</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### 範例回應 {#sample-response}

* **案例1：成功**
</br>
  * **XML：**
  </br>

    ``XML
    &lt;？xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;？>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
    `&#39;



* **JSON：**

  ```JSON
  {
    "mvpd": "sampleMvpdId",
    "resource": "sampleResourceId",
    "requestor": "sampleRequestorId",
    "expires": "1348148289000"
  }
  ```

>[!IMPORTANT]
>
>當回應來自Proxy MVPD時，可能包含名為`proxyMvpd`的其他元素。



* **案例2：拒絕授權**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
