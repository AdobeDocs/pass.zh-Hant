---
title: 提供MVPD清單
description: 提供MVPD清單
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 2%

---

# （舊版）提供MVPD清單 {#provide-mvpd-list}

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

傳回要求者的已設定MVPD清單。

| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br>例如：</br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestorId | Adobe Pass 驗證 | 1.要求者</br>    （路徑元件）</br>_2。  deviceType （已棄用）_ | GET | 包含MVPD清單的XML或JSON。 | 200 |

{style="table-layout:auto"}


| 輸入引數 | 說明 |
| --------------- | ------------------------------------------------------------- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| *deviceType* | 裝置型別。 |

{style="table-layout:auto"}

### 範例回應 {#sample-response}

與/config servlet的現有MVPD XML回應相同

注意：所有設定為使用Platform SSO的MVPD在其對應節點(JSON/XML)中都具有下列額外屬性：

* **enablePlatformServices （布林值）：**&#x200B;旗標，指出是否透過Platform SSO整合此MVPD
* **boardingStatus （字串）：**&#x200B;旗標，指出MVPD是否完全支援Platform SSO （支援）或MVPD是否只出現在平台選擇器（選擇器）中
* **displayInPlatformPicker （布林值）：**&#x200B;此MVPD是否出現在平台選擇器中
* **platformMappingId （字串）：**&#x200B;此MVPD的識別碼（由平台所知）
* **requiredMetadataFields （字串陣列）：**&#x200B;使用者中繼資料欄位在成功登入時應該可以使用
