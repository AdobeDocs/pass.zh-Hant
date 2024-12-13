---
title: 擷取Platform SSO設定檔要求
description: 擷取Platform SSO設定檔要求
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 1%

---

# （舊版）擷取平台SSO設定檔請求 {#retrieve-platform-sso-profile-request}

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

此資源會產生要求者ID和MVPD Tuple的設定檔要求。


| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.要求者（路徑引數）</br>2。 mvpd （路徑引數）</br>3。 deviceType （必要） | GET | 回應Content-Type將會是應用程式/八位元資料流，因為使用者端應用程式的實際裝載是不透明的。</br></br>應用程式應將回應轉送至Platform</br></br>SSO引擎以取得設定檔SSO。 | 200 — 成功   </br>400 — 錯誤請求 |


| 輸入引數 | 說明 |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| mvpd | 此操作有效的MVPD ID。 |
| deviceType | 我們正嘗試為其取得設定檔請求的Apple平台。  **iOS**&#x200B;或&#x200B;**tvOS**。 |
