---
title: 依第二熒幕Web應用程式檢查驗證流程
description: 依第二熒幕Web應用程式檢查驗證流程
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# 依第二熒幕Web應用程式檢查驗證流程 {#check-authentication-flow-by-second-screen-web-app}

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

第二個熒幕登入Web應用程式應使用此API，以確認Adobe Pass驗證已確認從MVPD成功登入。 我們建議先呼叫此API，再向一般使用者顯示成功訊息，指示他/她繼續前往裝置主控台，以繼續執行工作流程。


| 端點 | 已呼叫  </br>作者： | 輸入   </br>引數 | HTTP  </br>方法 | 回應 | HTTP  </br>回應 |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{註冊代碼} | 登入網頁應用程式 | 1.註冊代碼  </br>    （路徑元件）</br>2.  要求者  </br>    （必要） | GET | XML或JSON包含失敗時的錯誤詳細資料。 | 200 — 成功   </br>403 — 禁止 |

</br>

| 輸入引數 | 說明 |
| ----------------- | --------------------------------------------------------------------------------------------- |
| 註冊代碼 | 使用者在驗證流程開始時提供的註冊代碼值。 |
| 要求者 | 此作業有效的程式設計師要求者ID。 |


### 範例回應（發生錯誤時） {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

### [返回REST API參考](/help/authentication/rest-api-reference.md)
