---
title: 在iOS上重設暫時傳遞
description: 在iOS上重設暫時傳遞
exl-id: 53a22fae-192c-4b4c-9d63-fd9a2d960923
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---

# 在iOS上重設暫時傳遞 {#reset-temp-pass-on-ios}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>

iOS示範應用程式包含重設Temp Pass TTL的專用畫面。 重設作業需要下列資訊：

- **環境：**&#x200B;指定將接收重設暫時通過網路呼叫的Adobe付費電視傳遞伺服器端點。 可能的值： **Prequal** (*mgmt-prequal.auth-staging.adobe.com*)、**Release** (*mgmt.auth.adobe.com*)或&#x200B;**Custom** (保留給Adobe內部測試)。
- **OAuth2持有人權杖：** OAuth2權杖是授權Adobe付費電視驗證程式設計師的必要專案。 此類權杖可以從專用的付費電視驗證OAuth2端點取得(例如&#x200B;*curl -u &quot;\&lt;consumer\_key\>：\&lt;consumer\_secret\_key\>*&quot; *&quot;https://mgmt.auth.adobe.com/oauth2/permanent\_accesstoken？grant\_type=client\_credentials&quot;*)。
- **要求者識別碼：**&#x200B;目前程式設計師的唯一識別碼。 系統會從示範應用程式的主畫面（要求者欄位）讀取此值。
- **暫存傳遞ID：**&#x200B;暫存傳遞MVPD的唯一識別碼。
- **裝置識別碼：**&#x200B;示範應用程式所計算的雜湊裝置識別碼。
- **一般金鑰：**&#x200B;某些Temp Pass MVPD （亦即下一個可擴充的Temp Pass功能）支援用於重設Temp Pass的一般金鑰（連同裝置ID）。

上述所有引數（除了&#x200B;*泛型索引鍵*）都是必要引數。 以下是示範應用程式將執行的引數和相關網路呼叫範例（範例形式為*curl *命令）：

- **環境：**&#x200B;版本(*mgmt.auth.adobe.com*)
- **OAuth2持有人權杖：** H4j7cF3GtJX81BrsgDa10GwSizVz
- **程式設計師識別碼：**&#x200B;參考
- **暫存傳遞ID：** TempPassREF
- **裝置識別碼：** f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991
- **泛型索引鍵：** null （未提供值）

```curl
curl -X DELETE -H "Authorization:Bearer* *H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

將向&#x200B;**/reset**&#x200B;端點發出DELETE的HTTP要求，在授權標頭中傳遞&#x200B;*OAuth2持有人權杖*，並將&#x200B;*裝置ID*、*要求者ID*&#x200B;和&#x200B;*暫時傳遞ID (MVPD ID)*&#x200B;作為引數。

如果程式設計師提供&#x200B;*泛型金鑰*&#x200B;的值，將會執行另一個HTTP呼叫（這次是至&#x200B;**/reset/泛型**&#x200B;端點），在&#x200B;*key*&#x200B;要求引數中傳遞&#x200B;*泛型金鑰*。

例如，將&#x200B;*一般金鑰*設定為電子郵件地址雜湊(針對
暫時傳遞MVPD支援此類功能)將產生
接聽HTTP呼叫(電子郵件是`user@domain.com`其SHA-256
雜湊為`f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`)：

```curl
curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz"
"https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```

請務必強調，在示範應用程式中重設Temp Pass對同一部裝置上的程式設計師應用程式可能沒有相同的效果。 這是因為裝置ID （由示範應用程式和AccessEnabler所計算）可能並非對裝置上的所有應用程式都相同：

- iOS 6及以下版本：裝置ID是使用MAC位址計算（每個應用程式都是唯一的），因此在示範應用程式中重設Temp Pass會在裝置上的所有其他程式設計人員應用程式中重設。

- iOS 7和以上版本：裝置ID是根據IDFV （廠商識別碼）值計算而得，此值在所有應用程式中具有相同的套件ID首碼（亦即所有元件中只有最後一個）時都是唯一的。 由於示範應用程式和程式設計人員應用程式具有不同的套件ID，在示範應用程式中重設Temp Pass對程式設計人員應用程式沒有影響。

最後一個使用案例(iOS 7及更新版本)最常見，因此讓我們瞭解在這種情況下，程式設計師如何為他們的應用程式重設Temp Pass。 有幾個選項：

1. 將程式碼從示範應用程式移植到程式設計人員應用程式。 *TempPassResetViewController*&#x200B;和&#x200B;*DeviceIdDemoApp*&#x200B;類別包含重設Temp Pass的核心邏輯，而且可以輕鬆修改這些類別，並將其包含在Programmer App中。

1. 執行HTTP要求以使用&#x200B;*curl*&#x200B;重設Temp Pass。 計算程式設計人員應用程式的IDFV並在其上套用SHA-256雜湊（*DeviceIdDemoApp*&#x200B;類別中的範常式式碼），即可取得device\_Id引數。

1. 只要將程式設計師應用程式的雜湊IDFV指定為&#x200B;*一般金鑰*，即可從示範應用程式執行重設。 這會產生兩個網路呼叫：一個用於重設示範應用程式的暫時傳遞（與程式設計師無關），另一個用於重設程式設計師應用程式的暫時傳遞。

上述所有選項都類似，由程式設計師根據實作的容易程度來選擇一個選項。
