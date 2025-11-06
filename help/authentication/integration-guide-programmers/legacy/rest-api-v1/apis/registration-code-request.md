---
title: 註冊頁面
description: 註冊頁面
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 0%

---

# （舊版）註冊頁面 {#registration-page}

## REST API端點 {#clientless-endpoints}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

>[!NOTE]
>
> REST API實作已由[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)限制

&lt;REGGIE_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

<br>

## 說明 {#create-reg-code-svc}

傳回隨機產生的註冊代碼和登入頁面URI。

| 端點 | 呼叫<br>者 | 輸入   <br>引數 | HTTP <br>方法 | 回應 | HTTP <br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode<br>例如：<br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | 串流應用程式<br>或<br>程式設計師服務 | 1.要求者<br>    （路徑元件）<br>2。  deviceId （雜湊）   <br>    （必要）<br>3。  device_info/X-Device-Info （必要）<br>4。  mvpd （選用）<br>5。  ttl （選擇性）<br> | POST | 包含註冊代碼的XML或JSON，以及失敗時的資訊或錯誤詳細資料。 請參閱下列範例。 | 201 |

{style="table-layout:auto"}

| 輸入引數 | 型別 | 說明 |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Authorization | 標頭<br>值：持有人&lt;access_token> | DCR存取Token |
| Accept | 標頭<br>值： application/json | 指出使用者端應該能夠瞭解的內容型別 |
| 要求者 | 查詢引數 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 查詢引數 | 裝置識別碼位元組。 |
| device_info/<br>X-Device-Info | device_info：內文<br> X-Device-Info：標頭 | 串流裝置資訊。<br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數的潛在大小以及GET URL長度的限制，它應該作為X-Device-Info傳遞到http標頭。 <br>檢視[傳遞裝置和連線資訊](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)中的完整詳細資料。 |
| mvpd | 查詢引數 | 此操作有效的MVPD ID。 |
| ttl | 查詢引數 | 此regcode應在秒記憶體留多久。<br>**注意**： ttl允許的最大值為36000秒（10小時）。 較高的值會導致400 HTTP回應（錯誤請求）。 如果`ttl`留空，Adobe Pass驗證會設定30分鐘的預設值。 |
| _deviceType_ | 查詢引數 | 已棄用，不應使用。 |
| _deviceUser_ | 查詢引數 | 已棄用，不應使用。 |
| _appId_ | 查詢引數 | 已棄用，不應使用。 |

{style="table-layout:auto"}

>[!CAUTION]
>
>**串流裝置IP位址**
><br>
>對於使用者端對伺服器實作，串流裝置IP位址會與此呼叫一併隱含傳送。  對於伺服器對伺服器實作，其中發出&#x200B;**regcode**&#x200B;呼叫是程式設計人員服務，而不是串流裝置，以下標頭是傳遞串流裝置IP位址的必要專案：
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>其中`<streaming\_device\_ip>`是串流裝置的公用IP位址。
><br><br>
>範例：<br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
<br>

### 回應JSON


#### 註冊代碼JSON範例

```JSON
{
  "id": "ef5a79e8-7c8a-41d6-a45a-e378c6c7c8b5",
  "code": "IYQD5JQ",
  "requestor": "sampleRequestorId",
  "mvpd": "sampleMvpdId",
  "generated": 1704963921144,
  "expires": 1704965721144,
  "info": {
    "deviceId": "c28tZGV2aWQtMDAz",
    "deviceInfo": "eyJ0eXBlIjoiU2V0VG9wQm94IiwibW9kZWwiOiJBRlRNTSIsInZlcnNpb24iOnsibWFqb3IiOjAsIm1pbm9yIjowLCJwYXRjaCI6MCwicHJvZmlsZSI6IiJ9LCJoYXJkd2FyZSI6eyJuYW1lIjoiQUZUTU0iLCJ2ZW5kb3IiOiJVbmtub3duIiwidmVyc2lvbiI6eyJtYWpvciI6MCwibWlub3IiOjAsInBhdGNoIjowLCJwcm9maWxlIjoiIn0sIm1hbnVmYWN0dXJlciI6IlJva3UifSwib3BlcmF0aW5nU3lzdGVtIjp7Im5hbWUiOiJBbmRyb2lkIiwiZmFtaWx5IjoiQW5kcm9pZCIsInZlbmRvciI6IkFtYXpvbiIsInZlcnNpb24iOnsibWFqb3IiOjcsIm1pbm9yIjoxLCJwYXRjaCI6MiwicHJvZmlsZSI6IiJ9fSwiYnJvd3NlciI6eyJuYW1lIjoiQ2hyb21lIiwidmVuZG9yIjoiR29vZ2xlIiwidmVyc2lvbiI6eyJtYWpvciI6MTEyLCJtaW5vciI6MCwicGF0Y2giOjU2MTUsInByb2ZpbGUiOiIifSwidXNlckFnZW50IjoiTW96aWxsYS81LjAgKExpbnV4OyBBbmRyb2lkIDcuMS4yOyBBRlRNTSBCdWlsZC9OUzYyOTc7IHd2KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBWZXJzaW9uLzQuMCBDaHJvbWUvMTEyLjAuNTYxNS4xOTcgTW9iaWxlIFNhZmFyaS81MzcuMzYgQWRvYmVQYXNzTmF0aXZlRmlyZVRWLzMuMC44Iiwib3JpZ2luYWxVc2VyQWdlbnQiOiJNb3ppbGxhLzUuMCAoTGludXg7IEFuZHJvaWQgNy4xLjI7IEFGVE1NIEJ1aWxkL05TNjI5Nzsgd3YpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIFZlcnNpb24vNC4wIENocm9tZS8xMTIuMC41NjE1LjE5NyBNb2JpbGUgU2FmYXJpLzUzNy4zNiBBZG9iZVBhc3NOYXRpdmVGaXJlVFYvMy4wLjgifSwiZGlzcGxheSI6eyJ3aWR0aCI6MCwiaGVpZ2h0IjowLCJwcGkiOjAsIm5hbWUiOiJESVNQTEFZIiwidmVuZG9yIjpudWxsLCJ2ZXJzaW9uIjpudWxsLCJkaWFnb25hbFNpemUiOm51bGx9LCJhcHBsaWNhdGlvbklkIjpudWxsLCJjb25uZWN0aW9uIjp7ImlwQWRkcmVzcyI6IjE5My4xMDUuMTQwLjEzMSIsInBvcnQiOiI5OTM0Iiwic2VjdXJlIjpmYWxzZSwidHlwZSI6bnVsbH19",
    "userAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "originalUserAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "authorizationType": "OAUTH2",
    "sourceApplicationInformation": {
      "id": "14138364-application-id",
      "name": "application name",
      "version": "1.0.0"
    }
  }
}
```

<br>

| 元素名稱 | 說明 |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| id | 註冊代碼服務產生的UUID |
| 程式碼 | 註冊代碼服務產生的註冊代碼 |
| 要求者 | 要求者ID |
| mvpd | Mvpd ID |
| 已產生 | 註冊代碼建立時間戳記（自1970年1月1日GMT起以毫秒為單位） |
| 過期 | 註冊代碼過期的時間戳記（自1970年1月1日以來以毫秒為單位GMT） |
| deviceId | Base64唯一裝置識別碼 |
| 資訊:deviceId | Base64裝置型別 |
| 資訊:deviceInfo | Base64標準化裝置資訊建置在從使用者代理程式、X-Device-Info或device_info收到的資訊上 |
| 資訊:userAgent | 應用程式傳送的使用者代理 |
| 資訊:originalUserAgent | 應用程式傳送的使用者代理 |
| 資訊:authorizationType | 使用DCR的呼叫的OAUTH2 |
| 資訊:sourceApplicationInformation | DCR中設定的應用程式資訊 |

{style="table-layout:auto"}


<br>

### 錯誤訊息JSON回應範例(#error-sample-response)

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

