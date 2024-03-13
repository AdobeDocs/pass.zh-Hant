---
title: 提供MVPD清單
description: 提供MVPD清單
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 2%

---

# 提供MVPD清單 {#provide-mvpd-list}

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

傳回要求者的已設定MVPD清單。

| 端點 | 已呼叫  </br>作者： | 輸入   </br>引數 | HTTP  </br>方法 | 回應 | HTTP  </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/config/{requestorId}</br></br>例如：</br></br>&lt;sp_fqdn>/api/v1/config/sampleRequestorId | Adobe Pass 驗證 | 1.請求者</br>    （路徑元件）</br>_2.  deviceType （已棄用）_ | GET | 包含MVPD清單的XML或JSON。 | 200 |

{style="table-layout:auto"}


| 輸入引數 | 說明 |
| --------------- | ------------------------------------------------------------- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| *deviceType* | 裝置型別。 |

{style="table-layout:auto"}

### 範例回應 {#sample-response}

與/config servlet的現有MVPD XML回應相同

注意：所有設定為使用Platform SSO的MVPD在其對應節點(JSON/XML)中都具有下列額外屬性：

* **enablePlatformServices （布林值）：** 表示此MVPD是否透過Platform SSO整合的旗標
* **boardingStatus （字串）：** 此旗標可指出MVPD是否完全支援Platform SSO （支援），或MVPD是否只出現在平台選擇器（選擇器）中
* **displayInPlatformPicker （布林值）：** 此MVPD是否出現在平台選擇器中
* **platformMappingId （字串）：** 平台所知的此MVPD的識別碼
* **requiredMetadataFields （字串陣列）：** 使用者中繼資料欄位應在成功登入後提供
