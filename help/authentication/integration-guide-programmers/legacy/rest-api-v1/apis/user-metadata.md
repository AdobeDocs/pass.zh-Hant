---
title: 使用者中繼資料
description: 使用者中繼資料
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# （舊版）使用者中繼資料 {#user-metadata}

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

`<REGGIE_FQDN>`：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 說明 {#description}

擷取MVPD所分享有關已驗證使用者的中繼資料。


| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.要求者</br>2。  deviceId （必要）</br>3。  device_info/X-Device-Info （必要）</br>4。  deviceType</br>5。  deviceUser （已棄用）</br>6。  appId （已棄用） | GET | 包含使用者中繼資料的XML或JSON，或如果失敗則包含錯誤詳細資料。 | 200 — 成功<p>404 — 找不到中繼資料<p>412 — 無效的AuthN權杖（例如，過期的權杖） |


| 輸入引數 | 說明 |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| device_info/<p>X-Device-Info | 串流裝置資訊。</br></br> **注意：**&#x200B;這可以作為URL引數傳遞device_info，但由於此引數可能的大小以及GETURL的長度限制，應該在http標頭中作為X-Device-Info傳遞。 </br></br>檢視[傳遞裝置和連線資訊](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)中的完整詳細資料。 |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br>若此引數設定正確，ESM提供的量度在使用Clienless時可依每個裝置型別[&#128279;](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#progr-filter-metrics)進行劃分，因此可針對Roku、AppleTV、Xbox等執行不同型別的分析。</br></br>檢視[在Pass量度中使用無使用者端裝置型別引數的好處](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **注意：** `device_info`會取代此引數。 |
| _deviceUser_ | 裝置使用者識別碼。</br></br> **注意：**&#x200B;若使用，`deviceUser`應該與[建立註冊代碼](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求中的值相同。 |
| _appId_ | 應用程式id/名稱。</br></br> **注意：** `device_info`會取代此引數。 若已使用，`appId`應具有與[建立註冊代碼](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)要求中相同的值。 |

>[!NOTE]
> 
>使用者中繼資料資訊應在驗證流程完成後提供，但可在授權流程上根據MVPD和中繼資料型別進行更新。




## 範例回應 {#sample-response}

成功呼叫後，伺服器會以XML （預設）或JSON物件回應，其結構類似於以下所示：


```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
              zip: ["12345", "34567"],
              maxRating: { 
                  "MPAA": "PG-13",
                  "VCHIP": "TV-Y", 
                  "URL": "http://exam.pl/e/manage/ratings"
                         },
              householdID: "3456",
              userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
              channelID: ["channel-1", "channel-2"]
              }
    }
```

在物件的根目錄下將有三個節點：

* *已更新*：指定代表上次更新中繼資料時間的UNIX時間戳記。 在驗證階段產生中繼資料時，伺服器一開始會設定此屬性。 後續呼叫（更新中繼資料後）會導致時間戳記增加。
* *資料*：包含實際的中繼資料值。
* *encrypted*：列出加密屬性的陣列。 若要解密特定的中繼資料值，程式設計師必須對中繼資料執行Base64解碼，然後使用自己的私密金鑰，對產生的值套用RSA解密(Adobe使用程式設計師的公開憑證加密伺服器上的中繼資料)。

發生錯誤時，伺服器會傳回XML或JSON物件，指定詳細的錯誤訊息。

如需詳細資訊，請參閱[使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)。

[返回REST API參考](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
