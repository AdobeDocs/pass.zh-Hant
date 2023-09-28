---
title: 擷取Platform SSO設定檔要求
description: 擷取Platform SSO設定檔要求
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# 擷取Platform SSO設定檔要求 {#retrieve-platform-sso-profile-request}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## REST API端點 {#clientless-endpoints}

&lt;reggie_fqdn>：

* 生產 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 分段 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>：

* 生產 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 分段 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 說明 {#description}

此資源會產生要求者ID和MVPD Tuple的設定檔要求。


| 端點 | 已呼叫  </br>作者： | 輸入   </br>引數 | HTTP  </br>方法 | 回應 | HTTP  </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/{requestor}/profile-requests/{mvpd} | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.要求者（路徑引數）</br>2. mvpd （路徑引數）</br>3. deviceType （必要） | GET | 回應Content-Type將會是應用程式/八位元資料流，因為使用者端應用程式的實際裝載是不透明的。</br></br>應用程式應將回應轉送至平台</br></br>用於取得設定檔SSO的SSO引擎。 | 200 — 成功   </br>400 — 錯誤請求 |


| 輸入引數 | 說明 |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| mvpd | 此作業適用的MVPD ID。 |
| deviceType | 我們正嘗試為其取得設定檔請求的Apple平台。  兩者之一 **iOS** 或 **tvOS**. |
