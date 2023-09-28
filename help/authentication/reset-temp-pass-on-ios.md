---
title: 在iOS上重設暫時傳遞
description: 在iOS上重設暫時傳遞
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# 在iOS上重設暫時傳遞 {#reset-temp-pass-on-ios}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>

iOS示範應用程式包含重設Temp Pass TTL的專用畫面。 重設作業需要下列資訊：

- **環境：** 指定將接收重設暫時通過網路呼叫的Adobe付費電視通過伺服器端點。 可能的值： **先決條件** (*mgmt-prequal.auth-staging.adobe.com*)， **版本** (*mgmt.auth.adobe.com*)或 **自訂** (保留給Adobe內部測試)。
- **OAuth2持有人權杖：** 必須有OAuth2權杖，才能授權程式設計師進行Adobe付費電視驗證。 此類權杖可以從專用的付費電視驗證OAuth2端點獲取(例如 *curl -u &quot;\&lt;consumer _key=&quot;&quot;>：\&lt;consumer _secret=&quot;&quot; _key=&quot;&quot;>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken？grant\_type=client\_credentials&quot;*)。
- **要求者ID：** 目前程式設計師的唯一ID。 系統會從示範應用程式的主畫面（要求者欄位）讀取此值。
- **暫存傳遞ID：** 暫時傳遞MVPD的唯一ID。
- **裝置ID：** 由示範應用程式運算出的雜湊裝置ID。
- **一般金鑰：** 有些Temp Pass MVPD （亦即下一個可擴充的Temp Pass功能）支援用於重設Temp Pass的通用金鑰（與裝置ID一併提供）。

上述所有引數(除了 *一般金鑰*)為必填欄位。 以下是示範應用程式將執行的引數和相關網路呼叫範例（範例形式為*curl *命令）：

- **環境：** 發行版本(*mgmt.auth.adobe.com*)
- **OAuth2持有人權杖：** H4j7cF3GtJX81BrsgDa10GwSizVz
- **程式設計師ID：** 參照
- **暫存傳遞ID：** TempPassREF
- **裝置ID：** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **一般金鑰：** null （未提供值）

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

將會向發出DELETEHTTP要求 **/reset** 端點，傳遞 *OAuth2持有人權杖* 在Authorization標頭和 *裝置ID*， *要求者ID* 和 *暫存傳遞ID (MVPD ID)* 作為引數。

如果程式設計師為 *一般金鑰*，則會執行另一個HTTP呼叫(這次是 **/reset/generic** 端點)，傳遞 *一般金鑰* 內部 *key* 要求引數。

例如，設定 *一般金鑰* 至電子郵件地址雜湊（適用於支援這類功能的暫時傳遞MVPD）將產生以下HTTP呼叫(電子郵件為 `user@domain.com` 其SHA-256雜湊為 `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`)：

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

請務必強調，在示範應用程式中重設Temp Pass對同一部裝置上的程式設計師應用程式可能沒有相同的效果。 這是因為裝置ID （由示範應用程式和AccessEnabler所計算）可能並非對裝置上的所有應用程式都相同：

- iOS 6及以下版本：裝置ID是使用MAC位址計算（每個應用程式都是唯一的），因此在示範應用程式中重設Temp Pass會在裝置上的所有其他程式設計人員應用程式中重設。

- iOS 7和以上版本：裝置ID是根據IDFV （廠商識別碼）值計算而得，此值在所有應用程式中具有相同的套件ID首碼（亦即所有元件中只有最後一個）時都是唯一的。 由於示範應用程式和程式設計人員應用程式具有不同的套件ID，在示範應用程式中重設Temp Pass對程式設計人員應用程式沒有影響。

最後一個使用案例(iOS 7及更新版本)最常見，因此讓我們瞭解在這種情況下，程式設計師如何為他們的應用程式重設Temp Pass。 有幾個選項：

1. 將程式碼從示範應用程式移植到程式設計人員應用程式。 此 *TempPassResetViewController* 和 *DeviceIdDemoApp* 類別包含重設Temp Pass的核心邏輯，而且可輕鬆修改並包含在「程式設計人員」應用程式中。

1. 執行HTTP要求以重設Temp Pass *curl*. device\_Id引數可透過計算程式設計人員應用程式的IDFV並在其上套用SHA-256雜湊來取得(中的範常式式碼 *DeviceIdDemoApp* 類別)。

1. 只要將程式設計師應用程式的雜湊IDFV指定為 *一般金鑰*. 這會產生兩個網路呼叫：一個用於重設示範應用程式的暫時傳遞（與程式設計師無關），另一個用於重設程式設計師應用程式的暫時傳遞。

上述所有選項都類似，由程式設計師根據實作的容易程度來選擇一個選項。
