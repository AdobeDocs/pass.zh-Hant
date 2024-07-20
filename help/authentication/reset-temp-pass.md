---
title: 重設暫時通過
description: 重設暫時通過
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 28d432891b7d7855e83830f775164973e81241fc
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---

# 重設暫時通過 {#reset-temp-pass}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。
>
>若要使用Reset Temp Pass API，您需要：
>- 請要求支援小組為您註冊的應用程式提供軟體宣告
>- 根據[動態使用者端註冊](dynamic-client-registration.md)取得存取權杖
> 

為了&#x200B;**重設特定的Temp Pass**，Adobe Pass驗證為程式設計師提供&#x200B;*公用*&#x200B;網頁API：

- **環境：**&#x200B;指定將接收重設暫時通過網路呼叫的Adobe付費電視傳遞伺服器端點。 可能的值： **Prequal** (*mgmt-prequal.auth.adobe.com*)、**Release** (*mgmt.auth.adobe.com*)或&#x200B;**Custom** (保留給Adobe內部測試)。
- **OAuth2存取權杖：**&#x200B;必須有OAuth2權杖才能授權程式設計師進行Adobe付費電視驗證。 可從[Dynamic Client Registration](dynamic-client-registration.md)取得此權杖。
- **暫時傳遞ID：**&#x200B;要重設的暫時傳遞MVPD的唯一識別碼。（程式設計師可以使用多個Temp Pass MVPD，並且想要重設特定的MVPD）
- **一般金鑰：**&#x200B;某些Temp Pass MVPD （亦即[促銷暫存MVPD](promotional-temp-pass.md)）。

上述所有引數（除了&#x200B;*泛型索引鍵*）都是必要引數。 以下是引數和關聯網路呼叫的範例（範例採用*curl *命令的形式）：

- **環境：**&#x200B;版本(*mgmt.auth.adobe.com*)
- **OAuth2存取權杖：** &lt;access_token>來自[動態使用者端註冊](dynamic-client-registration.md)
- **程式設計師識別碼：**&#x200B;參考
- **暫存傳遞ID：** TempPassREF
- **泛型索引鍵：** null （未提供值）

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

將向&#x200B;**/reset**&#x200B;端點發出DELETE的HTTP要求，在授權標頭中傳遞&#x200B;*OAuth2存取權杖*，並將&#x200B;*裝置ID*、*要求者ID*&#x200B;和&#x200B;*暫時傳遞ID (MVPD ID)*&#x200B;作為引數。

如果程式設計師提供&#x200B;*泛型金鑰*&#x200B;的值，將會執行另一個HTTP呼叫（這次是至&#x200B;**/reset/泛型**&#x200B;端點），在&#x200B;*key*&#x200B;要求引數中傳遞&#x200B;*泛型金鑰*。

例如，將&#x200B;*一般金鑰*設定為電子郵件地址雜湊(針對
暫時傳遞MVPD支援此類功能)將產生
接聽HTTP呼叫(電子郵件是`user@domain.com`其SHA-256
雜湊為`f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`)：

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


為了&#x200B;**重設所有裝置的特定Temp Pass**，Adobe Pass驗證為程式設計師提供&#x200B;*公用*&#x200B;網頁API：

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
- **標頭：**&#x200B;授權：持有人&lt;access_token_here>
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
