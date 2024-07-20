---
title: 在Adobe Pass驗證中使用Experience CloudID
description: 在Adobe Pass驗證中使用Experience CloudID
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# 在Adobe Pass驗證中使用Experience CloudID

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 什麼是Experience CloudID以及如何取得該ID？ {#what-exp-cloud-id-obtain}

Experience CloudID （簡稱ECID）是Adobe Experience Cloud為您的應用程式/網站中的每位使用者產生的唯一ID。 ECID多用於所有用來連結多個應用程式/網站上特定使用者資訊的Experience Cloud報表。

如果您已有提供訪客ID的系統，您應針對此檔案的範圍使用相同ID。

取得ECID的一種方式是使用Experience CloudID服務。 您可以根據TDM、JS程式庫、伺服器端、直接整合或行動平台的原生程式庫，使用您偏好的實作型別。 如需可用服務、程式庫、SDK和實作指南的完整檢視，請參閱： <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html>

## 在Adobe Pass驗證中使用Experience CloudID有何優點？ {#benefit-ex-cloud-id}

如果您設定我們的SDK和無使用者端REST API來使用您的ECID，您稍後將能夠將Adobe Pass驗證收集的資料連結至您現有的Experience Cloud解決方案。 這可讓您更瞭解Adobe提供之所有解決方案的客戶歷程和體驗。

## 如何在Adobe Pass驗證中使用Experience CloudID？ {#how-to-ex-cloud-id-authn}

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

iOS/tvOS SDK有一個專用方法，稱為setOptions。

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

透過Adobe Pass的REST API使用時，應在所有API **上**&#x200B;將&#x200B;**ECID**&#x200B;值當作名為&#x200B;**&#39;ap_vi&#39;**&#x200B;的引數傳送。

**使用範例：**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`
