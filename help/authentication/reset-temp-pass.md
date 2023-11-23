---
title: 重設暫時通過
description: 重設暫時通過
source-git-commit: 4ae0b17eff2dfcf0aaa5d11129dfd60743f6b467
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 重設暫時通過 {#reset-temp-pass}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。
>
>若要使用Reset Temp Pass API，您需要：
>- 請要求支援小組為您註冊的應用程式提供軟體宣告
>- 取得存取權杖依據 [動態使用者端註冊](dynamic-client-registration.md)
> 

為了 **重設特定的暫時傳遞**， Adobe Pass驗證為程式設計師提供 *公共* 網頁API：

- **環境：** 指定將接收重設暫時通過網路呼叫的Adobe付費電視通過伺服器端點。 可能的值： **先決條件** (*mgmt-prequal.auth.adobe.com*)， **版本** (*mgmt.auth.adobe.com*)或 **自訂** (保留給Adobe內部測試)。
- **OAuth2存取Token：** 必須有OAuth2權杖，才能授權程式設計師進行Adobe付費電視驗證。 此代號可以從 [動態使用者端註冊](dynamic-client-registration.md).
- **暫存傳遞ID：** 要重設的暫時傳遞MVPD的唯一ID。（程式設計師可以使用多個Temp Pass MVPD，並且想要重設特定的MVPD）
- **一般金鑰：** 部分Temp Pass MVPD (即 [促銷臨時傳遞](promotional-temp-pass.md))。

上述所有引數(除了 *一般金鑰*)為必填欄位。 以下是引數和關聯網路呼叫的範例（範例採用*curl *命令的形式）：

- **環境：** 發行版本(*mgmt.auth.adobe.com*)
- **OAuth2存取Token：** &lt;access_token> 從 [動態使用者端註冊](dynamic-client-registration.md)
- **程式設計師ID：** 參照
- **暫存傳遞ID：** TempPassREF
- **一般金鑰：** null （未提供值）

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

將會向發出DELETEHTTP要求 **/reset** 端點，傳遞 *OAuth2存取Token* 在Authorization標頭和 *裝置ID*， *要求者ID* 和 *暫存傳遞ID (MVPD ID)* 作為引數。

如果程式設計師為 *一般金鑰*，則會執行另一個HTTP呼叫(這次是 **/reset/generic** 端點)，傳遞 *一般金鑰* 內部 *key* 要求引數。

例如，設定 *一般金鑰* 至電子郵件地址雜湊（適用於支援這類功能的暫時傳遞MVPD）將產生以下HTTP呼叫(電子郵件為 `user@domain.com` 其SHA-256雜湊為 `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`)：

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


為了 **重設所有裝置的特定Temp Pass**， Adobe Pass驗證為程式設計師提供 *公共* 網頁API：

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>上述URL會取代先前的重設API。 舊的重設API (v1)不再受支援。

- **通訊協定：** HTTPS
- **主機：**
   - 發行版本 — mgmt.auth.adobe.com
   - 前述 — mgmt-prequal.auth.adobe.com
- **路徑：** /reset-tempass/v3/reset
- **查詢引數：** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
- **標頭：** 授權：持有人 &lt;access_token_here>
- **回應：**
   - 成功 — HTTP 204
   - 失敗：
      - HTTP 400的不正確請求
      - HTTP 401 （如果存取遭拒）。 使用者端必須要求新的access_token。
      - HTTP 403 （如果不再允許使用者端ID執行請求）。 應產生新的使用者端認證。


例如：

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
