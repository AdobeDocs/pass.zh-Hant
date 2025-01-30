---
title: 決定
description: 決定
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# 決定 {#decisions}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

決策是由Adobe Pass驗證[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)根據使用者的MVPD授權或預先授權查詢而產生，以判斷對[受保護內容](#protected-resources)的存取權是否被授與或拒絕。

根據叫用的API，提供兩種型別的決策：

* [預先授權決定](#preauthorization-decisions)是資訊性決定。
* [授權決定](#authorization-decisions)是授權決定。

## 預先授權決定 {#preauthorization-decisions}

預先授權決定是資訊性的決定，可讓使用者端應用程式被告知MVPD是否允許或拒絕使用者存取[受保護的資源](#protected-resources)。

預先授權（預檢授權）的目的是讓應用程式能夠顯示使用者可能符合檢視資格之內容的精確資訊。 這是透過使用指示器（例如已鎖定或未鎖定的圖示）增強使用者介面以反映存取狀態來達成。

>[!IMPORTANT]
>
> 不得以授權方式使用預先授權決定來播放資源，因為這是[授權決定](#authorization-decisions)的目的。

預先授權API的使用並非強制性，如果使用者端應用程式想要在不進行任何篩選的情況下呈現資源目錄，則可略過此步驟。

如果使用者端應用程式打算使用此功能，請務必注意，每個API請求只能針對有限數量的資源取得預先授權決定，通常最多5個。

>[!IMPORTANT]
> 
> 只有在與MVPD和Adobe Pass驗證代表達成協定後，才能增加資源的最大數量。 同意後，您組織中的管理員或代表您行事的Adobe Pass驗證代表可以透過Adobe Pass TVE儀表板實作變更。
> 
> 如需詳細資訊，請參閱[TVE儀表板整合使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties)檔案。

MVPD可透過各種機制支援預先授權，每種機制對效能以及單一API請求中可處理的最大資源數量都有不同的影響。

如需有關支援預先授權的現有機制的詳細資訊，請參閱[MVPD預檢授權](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md)檔案。

>[!IMPORTANT]
>
> 對於沒有完整預檢授權支援的MVPD，預先授權的使用必須事先與MVPD和Adobe Pass驗證代表商定，因為這可能會導致效能問題和較慢的回應時間。

## 授權決定 {#authorization-decisions}

授權決定是授權決定，可讓使用者端應用程式符合MVPD的決定，允許或拒絕使用者存取[受保護的資源](#protected-resources)。

授權的目的是讓應用程式在使用MVPD進行許可權驗證並從Adobe Pass驗證收到媒體權杖後，播放使用者要求的資源。

>[!IMPORTANT]
> 
> Adobe Pass驗證建議程式設計師使用媒體權杖驗證器程式庫來驗證授權決定中包含的媒體權杖，在開始視訊資料流之前確保安全存取。
> 
> 如需詳細資訊，請參閱[媒體代號](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)檔案。

授權API的使用是強制性的，如果客戶端應用程式想要播放使用者請求的資源，則無法跳過此階段，因為它需要在釋放資料流之前向MVPD驗證使用者是否有權使用。

請務必注意，每個API請求只能針對有限數量的資源取得授權決策，通常為1。

>[!IMPORTANT]
>
> 只有在與MVPD和Adobe Pass驗證代表達成協定後，才能增加資源的最大數量。

## 受保護的資源 {#protected-resources}

受保護的資源是指可流程化的內容，由MVPD與參與程式設計人員之間的協定所定義的唯一值所識別。

受保護的資源遵循階層式樹狀結構，每個層級提供更精細的內容授權粒度：

* 網路
   * 頻道
      * 顯示
         * 集數
            * 資產

>[!IMPORTANT]
>
> 預先授權（預檢授權）著重於識別碼具有簡單字串或MRSS格式的通道層級資源。
> 
> 我們不建議使用包含`CDATA`區段之識別碼的資源（若是預先授權），因為它們主要用於MRSS定義的資產層級資源。

### 資源識別碼 {#resource-identifier}

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

唯一識別碼對Adobe Pass驗證而言主要是不透明的，但可根據MVPD的功能和需求套用轉換器。 如果MVPD無法辨識或剖析資源識別碼，則會傳回錯誤給Adobe Pass驗證，接著會使用[增強錯誤碼](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)將錯誤轉送給使用者端應用程式。

## REST API V2 {#rest-api-v2}

可以使用以下API擷取預先授權決定：

* [使用特定mvpd擷取預先授權決定](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

可以使用以下API來擷取授權決定：

* [使用特定mvpd擷取授權決策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

請參閱上述API的&#x200B;**回應**&#x200B;和&#x200B;**範例**&#x200B;區段，瞭解預先授權和授權決定的結構。

如需如何及何時整合上述API的詳細資訊，請參閱下列檔案：

* [主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
