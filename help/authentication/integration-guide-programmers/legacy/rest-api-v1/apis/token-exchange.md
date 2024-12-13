---
title: 交換Platform SSO權杖以取得Adobe權杖
description: 交換Platform SSO權杖以取得Adobe權杖
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# （舊版）交換平台SSO權杖以取得Adobe權杖 {#exchange-a-platform-sso-token-for-an-adobe-token}

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

允許將平台SSO設定檔「交換」為Adobe權杖。

| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.要求者（必要）</br>    </br>2。  deviceId （必要）</br>    </br>3。  mvpd （必要）</br>    </br>4。  deviceType （必要）</br>    </br>5。  SAMLResponse （必要）</br>    </br>6。  deviceUser （已棄用）</br>    </br>7。  appId （已棄用） | POST | 成功的回應將是「204無內容」，這表示已成功建立權杖，並已準備好用於授權流程。 | 204 — 無內容   </br>400 — 錯誤請求 |


| 輸入引數 | 說明 |
| --- | --- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| mvpd | 此操作有效的MVPD ID。 |
| deviceType | 我們正嘗試為其取得設定檔請求的Apple平台。  **iOS**&#x200B;或&#x200B;**tvOS**。 |
| SAMLResponse | Platform SSO傳回的實際設定檔。 |
| _deviceUser_ | 裝置使用者識別碼。 |
| _appId_ | 應用程式id/名稱。 |
