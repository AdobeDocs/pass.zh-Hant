---
title: 使用者中繼資料
description: 使用者中繼資料
exl-id: 3d7b6429-972f-4ccb-80fd-a99870a02f65
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 0%

---

# 使用者中繼資料 {#user-metadata}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!NOTE]
>
> REST API實作受限於 [節流機制](/help/authentication/throttling-mechanism.md)

## REST API端點 {#clientless-endpoints}

`<REGGIE_FQDN>`：

* 生產 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 分段 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`：

* 生產 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 分段 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 說明 {#description}

擷取MVPD所共用的有關已驗證使用者的中繼資料。


| 端點 | 已呼叫  </br>作者： | 輸入   </br>引數 | HTTP  </br>方法 | 回應 | HTTP  </br>回應 |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>`/api/v1/tokens/usermetadata | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.請求者</br>2.  deviceId （必要）</br>3.  device_info/X-Device-Info （必要）</br>4.  deviceType</br>5.  deviceUser （已棄用）</br>6.  appId （已棄用） | GET | 包含使用者中繼資料的XML或JSON，或如果失敗則包含錯誤詳細資料。 | 200 — 成功<p>404 — 找不到中繼資料<p>412 — 無效的AuthN權杖（例如，過期的權杖） |


| 輸入引數 | 說明 |
| --- | --- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| device_info/<p>X-Device-Info | 串流裝置資訊。</br></br> **注意：** 這可以作為URL引數傳遞device_info，但由於此引數潛在的大小以及GETURL長度的限制，它應該在http標頭中作為X-Device-Info傳遞。 </br></br> 如需詳細資訊，請參閱 [傳遞裝置和連線資訊](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br></br> 若此引數設定正確，ESM提供的量度會 [依裝置型別劃分](/help/authentication/entitlement-service-monitoring-overview.md#progr-filter-metrics) 使用無使用者端時，因此可針對Roku、AppleTV、Xbox等執行不同型別的分析。</br></br> 另請參閱 [在Pass量度中使用無使用者端裝置型別引數的好處](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md) </br></br> **注意：** 此 `device_info` 會取代此引數。 |
| _deviceuser_ | 裝置使用者識別碼。</br></br> **注意：** 若已使用， `deviceUser` 的值應該與 [建立註冊代碼](/help/authentication/registration-code-request.md) 要求。 |
| _appId_ | 應用程式id/名稱。 </br></br> **注意：** 此 `device_info` 會取代此引數。 若已使用， `appId` 的值應該與 [建立註冊代碼](/help/authentication/registration-code-request.md) 要求。 |

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

如需詳細資訊，請參閱 [使用者中繼資料](/help/authentication/user-metadata-feature.md).

[返回REST API參考](/help/authentication/rest-api-reference.md)
