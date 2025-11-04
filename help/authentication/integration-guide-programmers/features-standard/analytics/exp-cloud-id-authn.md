---
title: 在Adobe Pass驗證中使用Experience Cloud ID
description: 在Adobe Pass驗證中使用Experience Cloud ID
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: e4d243ebf293f3ecc38e532d77116c065a22ebd2
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---

# 在Adobe Pass驗證中使用Experience Cloud ID

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 什麼是Experience Cloud ID以及如何取得該ID？ {#what-exp-cloud-id-obtain}

Experience Cloud ID （簡稱ECID）是Adobe Experience Cloud為您的應用程式/網站中的每個使用者產生的唯一ID。 ECID在多個Experience Cloud報表中大量使用，這些報表用來連結關於多個應用程式/網站中特定使用者的資訊。

如果您已有提供訪客ID的系統，您應針對此檔案的範圍使用相同ID。

取得ECID的一種方式是使用Experience Cloud ID服務。 您可以根據TDM、JS程式庫、伺服器端、直接整合或行動平台的原生程式庫，使用您偏好的實作型別。 如需可用服務、程式庫、SDK及實作指南的完整檢視，請參閱： <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html?lang=zh-Hant>

## 在Adobe Pass驗證中使用Experience Cloud ID有何優點？ {#benefit-ex-cloud-id}

如果您設定我們的SDK和無使用者端REST API來使用您的ECID，您稍後將能夠將Adobe Pass驗證所收集的資料連結至您現有的Experience Cloud解決方案。 這可讓您更瞭解Adobe提供之所有解決方案的客戶歷程和體驗。

## 如何在Adobe Pass驗證中使用Experience Cloud ID？ {#how-to-ex-cloud-id-authn}

取得ECID （如上所述）後，您需要將此資訊傳遞至我們的SDK和無使用者端REST API。 這些資訊稍後將會在SDK發出的每個網路呼叫上傳遞至我們的伺服器。 每個SDK的設定程式不同，如下所示：

### JS SDK {#js-sdk}

針對JavaScript，您必須在對應中將ECID作為第三個引數傳遞至setRequestor呼叫。

**使用範例：**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### iOS/tvOS SDK {#ios-sdk}

對於iOS/tvOS SDK，有一個專用方法稱為setOptions。

**使用範例：**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/fireTV SDK {#android-sdk}

Android/fireTV SDK的機制類似於iOS。 只有引數名稱不同。 此API已記錄在此處。

**使用範例：**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### 無使用者端API {#clientless-api}

透過Adobe Pass的REST API v1使用時，應在所有API **上**&#x200B;將&#x200B;**ECID**&#x200B;值當作名為&#x200B;**&#39;ap_vi&#39;**&#x200B;的引數傳送。

**使用範例：**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`

### REST API V2 {#rest-api-v2}

透過Adobe Pass的REST API v2使用時，應在所有API **上**&#x200B;以名稱為&#x200B;**&#39;AP-Visitor-Identifier&#39;**&#x200B;的標頭傳送&#x200B;**ECID**&#x200B;值。

**使用範例：**

`POST: https://api.auth.adobe.com/api/v2/${serviceProvider}/sessions/`\
標頭：\
`AP-Visitor-Identifier: THE_ECID_VALUE`

