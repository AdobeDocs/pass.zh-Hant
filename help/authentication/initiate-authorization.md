---
title: 啟動授權
description: 啟動授權
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# 啟動授權 {#initiate-authorization}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!NOTE]
>
> REST API實作受限於 [節流機制](/help/authentication/throttling-mechanism.md)

## REST API端點 {#clientless-endpoints}

&lt;reggie_fqdn>：

* 生產 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 分段 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>：

* 生產 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 分段 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 說明 {#description}

取得授權回應。

| 端點 | 已呼叫  </br>作者： | 輸入   </br>引數 | HTTP  </br>方法 | 回應 | HTTP  </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authorize | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.請求者（必要）</br>2.  deviceId （必要）</br>3.  resource （必要）</br>4.  device_info/X-Device-Info （必要）</br>5.  _deviceType_</br> 6.  _deviceuser_ （已棄用）</br>7.  _appId_ （已棄用）</br>8.  額外引數（選擇性） | GET | XML或JSON包含授權詳細資訊，或如果失敗則包含錯誤詳細資訊。 請參閱下列範例。 | 200 — 成功  </br>403 — 無成功 |

{style="table-layout:auto"}

</br>


| 輸入引數 | 說明 |
| --- | --- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| resource | 包含resourceId （或MRSS片段）的字串，可識別使用者要求的內容並由MVPD授權端點識別。 |
| device_info/</br></br>X-Device-Info | 串流裝置資訊。</br></br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數潛在的大小以及GETURL長度的限制，應該在http標頭中作為X-Device-Info傳遞。 </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br>若此引數設定正確，ESM提供的量度會 [依裝置型別劃分](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) 使用無使用者端時，因此可針對Roku、AppleTV、Xbox等執行不同型別的分析。</br></br>另請參閱 [通過量度中無使用者端裝置型別引數的好處&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**：device_info會取代此引數。 |
| _deviceuser_ | 裝置使用者識別碼。 |
| _appId_ | 應用程式id/名稱。 </br></br>**注意**：device_info會取代此引數。 |
| 額外的引數 | 呼叫也可能包含可啟用其他功能的選用引數，例如：</br></br>* generic_data — 啟用 [促銷暫時傳遞](/help/authentication/promotional-temp-pass.md)</br></br>範例： `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**串流裝置IP位址**</br>
>對於使用者端對伺服器實作，串流裝置IP位址會與此呼叫一併隱含傳送。  針對伺服器對伺服器實作，其中 **regcode** 呼叫是由程式設計人員服務而不是串流裝置所進行，以下標題是傳遞串流裝置IP位址所必需：</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>位置 `<streaming\_device\_ip>` 是串流裝置的公用IP位址。</br></br>
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

    ```XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/resource>
    &lt;/authorization>
    ```



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
>當回應來自Proxy MVPD時，可能包含名為的其他元素 `proxyMvpd`.



* **案例2：拒絕授權**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
