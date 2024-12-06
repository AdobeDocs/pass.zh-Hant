---
title: 啟動登出
description: 啟動登出
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# 啟動登出 {#initiate-logout}

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

從儲存空間中移除AuthN和AuthZ權杖。


| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/logout | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.要求者</br>2。  deviceId （必要）</br>3。  device_info/X-Device-Info （必要）</br>4。  _deviceType_</br> 5。  _deviceUser_ （已棄用）</br>6。  _appId_ （已棄用） | DELETE | 無 | 204 |


| 輸入引數 | 說明 |
|-------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| device_info/</br></br>X-Device-Info | 串流裝置資訊。</br></br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數的潛在大小以及GETURL長度的限制，它應該作為X-Device-Info傳遞到http標頭。 </br></br>檢視[傳遞裝置和連線資訊](/help/authentication/integration-guide-programmers/passing-client-information-device-connection-and-application.md)中的完整詳細資料。 |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br>若此引數設定正確，ESM提供的量度在使用Clienless時可依裝置型別](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)進行[劃分，因此可針對Roku、AppleTV、Xbox等執行不同型別的分析。</br></br>檢視[在傳遞量度中使用無使用者端裝置型別引數的好處&#x200B;](/help/authentication/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**： device_info將會取代此引數。 |
| _deviceUser_ | 裝置使用者識別碼。</br></br>**注意**：若使用，deviceUser的值應該與[建立註冊代碼](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求中的值相同。 |
| _appId_ | 應用程式id/名稱。 </br></br>**注意**： device_info會取代此引數。 若已使用，`appId`應具有與[建立註冊代碼](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求中相同的值。 |

>[!IMPORTANT]
> 
>登出呼叫目前具有下列限制：它會從儲存體(亦即程式設計人員/ Adobe Pass驗證端)清除AuthN和AuthZ權杖，但&#x200B;**不會**&#x200B;呼叫MVPD登出端點。
