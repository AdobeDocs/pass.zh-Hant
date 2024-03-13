---
title: 擷取預先授權的資源清單
description: 擷取預先授權的資源清單
exl-id: 3821378c-bab5-4dc9-abd7-328df4b60cc3
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 0%

---

# 擷取預先授權的資源清單 {#retrieve-list-of-preauthorized-resources}

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

要求Adobe Pass驗證以取得預先授權資源清單。

有兩組API：一組用於串流應用程式或程式設計人員服務，另一組用於第二熒幕網頁應用程式。 本頁面說明串流應用程式或程式設計人員服務的API。


| 端點 | 已呼叫  </br>作者： | 輸入   </br>引數 | HTTP  </br>方法 | 回應 | HTTP  </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/preauthorize | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.請求者（必要）</br>2.  deviceId （必要）</br>3.  資源清單（必要）</br>4.  device_info/X-Device-Info （必要）</br>5.  _deviceType_</br> 6.  _deviceuser_ （已棄用）</br>7.  _appId_ （已棄用） | GET | 包含個別預先授權決定或錯誤詳細資料的XML或JSON。 請參閱下列範例。 | 200 — 成功</br></br>400 — 錯誤請求</br></br>401 — 未獲授權</br></br>405 — 不允許的方法  </br></br>412 — 先決條件失敗</br></br>500 — 內部伺服器錯誤 |


| 輸入引數 | 說明 |
| --- | --- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| 資源清單 | 字串，包含以逗號分隔的resourceId清單，用於識別使用者可能可以存取且可由MVPD授權端點辨識的內容。 |
| device_info/</br></br>X-Device-Info | 串流裝置資訊。</br></br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數潛在的大小以及GETURL長度的限制，應該在http標頭中作為X-Device-Info傳遞。 </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br>若此引數設定正確，ESM提供的量度會 [依裝置型別劃分](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) 使用無使用者端時，以便執行不同型別的分析，例如Roku、AppleTV和Xbox。</br></br>請參閱， [在pass量度中使用無使用者端裝置型別引數的好處&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**：此 `device_info` 會取代此引數。 |
| _deviceuser_ | 裝置使用者識別碼。 |
| _appId_ | 應用程式id/名稱。 </br></br>**注意**：device_info會取代此引數。 |



### 範例回應 {#sample-response}



**XML：**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
  <resource>
    <id>TestStream1</id>
    <authorized>true</authorized>
  </resource>
  <resource>
    <id>TestStream2</id>
    <authorized>false</authorized>
    <error>
      <status>403</status>
      <code>authorization_denied_by_mvpd</code>
      <message>User not authorized</message>
      <details>Your subscription package does not include the "TestStream3" channel.</details>
      <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
      <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
      <action>none</action>
    </error>
  </resource>
</resources>
```

</br>

**JSON：**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
