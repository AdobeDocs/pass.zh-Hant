---
title: 臨時通票和促銷臨時通票的免費預覽
description: 臨時通票和促銷臨時通票的免費預覽
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# （舊版）臨時通關和促銷臨時通關的免費預覽 {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

允許為Temp Pass和Promotional Temp Pass建立驗證權杖，而不需要第二個畫面。


| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
|-------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| &lt;SP_FQDN>/api/v1/authenticate/freepreview | 串流應用程式</br></br>或</br></br>程式設計師服務 | &#x200B;1. requestor_id （必要）</br>    </br>2。  deviceId （必要）</br>    </br>3。  mso_id （必要）</br>    </br>4。  domain_name （必要）</br>    </br>5。  device_info/X-Device-Info （必要）</br>6。  deviceType</br>    </br>7。  deviceUser （已棄用）</br>    </br>8。  appId （已棄用）</br>    </br>9。  generic_data （選用） | POST | 成功的回應將是「204無內容」，這表示已成功建立權杖，並已準備好用於授權流程。 | 204 — 無內容   </br>400 — 錯誤請求 |

<div>


| 輸入引數 | 說明 |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| requestor_id | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| mso_id | 此操作有效的MVPD ID。 |
| domain_name | 要授與權杖的網域名稱。 在授予授權權杖時，會與服務提供者的網域進行比較。 |
| device_info/</br></br>X-Device-Info | 串流裝置資訊。</br></br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數的潛在大小以及GET URL長度的限制，它應該作為X-Device-Info傳遞到http標頭。 </br></br>檢視[傳遞裝置和連線資訊](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)中的完整詳細資料。 |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br>若此引數設定正確，ESM提供的量度在使用Clienless時可依裝置型別[進行](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type)劃分，因此可針對Roku、AppleTV、Xbox等執行不同型別的分析。</br></br>檢視[使用無使用者端裝置型別引數的好處&#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**注意**： device_info將會取代此引數。 |
| _deviceUser_ | 裝置使用者識別碼。</br></br>**注意**：若使用，deviceUser的值應該與[建立註冊代碼](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求中的值相同。 |
| _appId_ | 應用程式id/名稱。 </br></br>**注意**： device_info會取代此引數。 若已使用，`appId`應具有與[建立註冊代碼](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求中相同的值。 |
| generic_data | 用於限制促銷暫時傳遞的權杖範圍。 |


### [返回REST API參考](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
