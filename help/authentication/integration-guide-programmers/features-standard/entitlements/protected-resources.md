---
title: 受保護的資源
description: 受保護的資源
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# 受保護的資源 {#protected-resources}

>[!IMPORTANT]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

受保護的資源代表使用者可以串流的內容，並由MVPD與參與程式設計人員之間協定所建立的唯一值識別。

受保護的資源遵循階層式樹狀結構，每個層級提供更精細的內容授權粒度：

* 網路
   * 頻道
      * 顯示
         * 集數
            * 資產

## 受保護的資源識別碼 {#identifiers}

資源唯一識別碼可以有兩種格式：

* 簡單的字串格式，例如管道（品牌）的唯一識別碼。
* 包含其他資訊的媒體RSS (MRSS)格式，例如標題、分級和家長監護中繼資料。

在簡單資源識別碼，例如「REF30」（假設代表管道）的情況下，可以將其轉譯為RSS資源識別碼，如下所示：

```RSS
    <rss version="2.0"> 
        <channel>
            <title>REF30</title>
        </channel>
    </rss>
```

在較複雜的資源識別碼的情況下，RSS資源識別碼可以包含其他評等資訊，如下所示：

```RSS
    <rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
        <channel>
            <title>REF30</title>
            <media:rating scheme="urn:mpaa">pg</media:rating>
        </channel>
    </rss>
```

## REST API V2 {#rest-api-v2}

擷取預先授權和授權決策的Adobe Pass驗證API要求使用者端應用程式將受保護資源的識別碼納入為引數。

預先授權和授權決定可使用以下API來擷取：

* [使用特定mvpd擷取預先授權決定](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [使用特定mvpd擷取授權決策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

請參閱上述API的&#x200B;**Request**&#x200B;和&#x200B;**Samples**&#x200B;區段，瞭解提供受保護資源的唯一識別碼所需的格式。

唯一識別碼對Adobe Pass驗證而言是不透明的，因為它們會直接傳遞至MVPD。 如果MVPD無法辨識或剖析資源識別碼，則會傳回錯誤給Adobe Pass驗證，然後使用[增強型錯誤碼](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)將錯誤轉送回使用者端應用程式。

如需如何及何時整合上述API的詳細資訊，請參閱下列檔案：

* [主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
