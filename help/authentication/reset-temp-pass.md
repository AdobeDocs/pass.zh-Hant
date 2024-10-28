---
title: 重設暫時通過
description: 重設暫時通過
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 32060798501abe2bf7314474d55cf844efb3f7b1
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---


# 重設暫時通過 {#reset-temp-pass}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 在使用Reset Temp Pass API之前，請確保符合以下先決條件：
>
> * 依照[擷取使用者端認證](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API檔案中的說明取得使用者端認證。
> * 依照[擷取存取權杖](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API檔案中的說明取得存取權杖。
>
> 請參閱[Dynamic Client Registration Overview](./dcr-api/dynamic-client-registration-overview.md)檔案，以取得有關如何建立註冊的應用程式及下載軟體陳述式的詳細資訊。

為了&#x200B;**重設裝置或所有裝置的特定Temp Pass**，Adobe Pass驗證為程式設計師提供&#x200B;*公用*&#x200B;網頁API：

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

* **通訊協定：** HTTPS
* **主機：**
   * 發行版本 — mgmt.auth.adobe.com
   * 前述 — mgmt-prequal.auth.adobe.com
* **路徑：** /reset-tempass/v3/reset
* **查詢引數：** device_id （未提供值時，預設為全部）、requestor_id （必要）、mvpd_id （必要）、appId、deviceUser、environment
* **標頭：**&#x200B;授權：持有人&lt;access_token_here>
* **回應：**
   * 成功 — HTTP 204
   * 失敗：xw
      * HTTP 400的不正確請求
      * HTTP 401 （如果存取遭拒）。 使用者端必須要求新的access_token。
      * HTTP 403 （如果不再允許使用者端ID執行要求）。 應產生新的使用者端認證。


例如：

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

為了&#x200B;**重設一般金鑰或所有金鑰的彈性Temp Pass**，Adobe Pass驗證為程式設計師提供&#x200B;*公用*&#x200B;網頁API：

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic
```

* **通訊協定：** HTTPS
* **主機：**
   * 發行版本 — mgmt.auth.adobe.com
   * 前述 — mgmt-prequal.auth.adobe.com
* **路徑：** /reset-tempass/v3/reset/generic
* **查詢引數：**&#x200B;索引鍵（未提供值，預設為全部）、requestor_id （必要）、mvpd_id （必要）、環境
* **標頭：**&#x200B;授權：持有人&lt;access_token_here>
* **回應：**
   * 成功 — HTTP 204
   * 失敗：
      * HTTP 400的不正確請求
      * HTTP 401 （如果存取遭拒）。 使用者端必須要求新的access_token。
      * HTTP 403 （如果不再允許使用者端ID執行要求）。 應產生新的使用者端認證。


例如，將&#x200B;*一般金鑰*設定為電子郵件地址雜湊(針對
暫時傳遞MVPD支援此類功能)將產生
接聽HTTP呼叫(電子郵件是`user@domain.com`其SHA-256
雜湊為`f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`)：

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```
