---
title: MVPD使用者中繼資料交換
description: MVPD使用者中繼資料交換
exl-id: 8bce6acc-cd33-476c-af5e-27eb2239cad1
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# MVPD使用者中繼資料交換

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 簡介 {#intro-user-metadata-exchange}

MVPD會維護其客戶的使用者特定中繼資料，這些中繼資料在某些情況下會與程式設計師共用。 Adobe Pass驗證的目的在於代理此「使用者中繼資料」的交換，但並非強制執行任何有關交換的規則。 交換規則是供MVPD與其程式設計人員合作夥伴合作使用。

目前可用於Exchange的使用者中繼資料型別包括下列專案：

* 郵遞區號
* 最高評等（VChip或MPAA）
* 使用者 ID
* 家庭ID
* 管道ID

使用此功能，MVPD和程式設計師可以實作特殊使用案例，例如家長監護。 例如，MVPD可以將家長評等資料傳送給程式設計師，程式設計師隨後會使用該資料來篩選使用者的可用檢視選擇。

使用者中繼資料關鍵點：

* 在驗證和授權流程期間，MVPD會將使用者中繼資料傳遞給程式設計師的應用程式
* Adobe Pass驗證會將中繼資料值儲存在AuthN和AuthZ權杖中
* Adobe Pass驗證可以標準化MVPD的值，這些值提供不同格式的使用者中繼資料
* 部分引數可使用程式設計人員的金鑰加密
* Adobe會透過設定變更提供特定值

>[!NOTE]
>
>使用者中繼資料是靜態中繼資料（驗證權杖TTL、授權權杖TTL和裝置ID）的延伸，先前可在Adobe Pass驗證中使用。

## 範例 {#example-mvpd-user-metadata-exch}

### 家長監護 {#example-parental-control}

此範例顯示下列專案的變更：

* [程式設計師到MVPD中繼資料交換](#progr-mvpd-metadata-exch)

* [MVPD與程式設計師中繼資料交換流程](#mvpd-progr-exchange-flow)

### 程式設計師到MVPD中繼資料交換 {#progr-mvpd-metadata-exch}

目前，程式設計師API、Adobe Pass驗證和MVPD授權者都僅支援通道層級的授權。 通道在程式設計師的getAuthorization() API呼叫中指定為純文字字串。 此字串會一直傳播至MVPD的授權後端：

使用者從程式設計師的應用程式或網站中，選擇支援XACML的MVPD （在此範例中為「TNT」）。 如需XACML的相關資訊，請參閱[可延伸存取控制標籤語言](https://en.wikipedia.org/wiki/XACML){target=_blank}。
程式設計師的應用程式會形成包含資源及其中繼資料的AuthZ請求。  此範例在頻道元素的媒體屬性中納入「pg」的MPAA評等：

```XML
var resource = '<rss version="2.0" xmlns:media="http://video.search.yahoo.com/mrss/">
                    <channel> 
                        <title>TNT</title> 
                        <media:rating scheme="urn:mpaa">pg</media:rating>
                    </channel>
                </rss>';
getAuthorization(resource);
```

當MVPD和程式設計師都支援時，Adobe Pass驗證實際上可支援更精細的授權，甚至資產層級。 資源及其中繼資料對Adobe而言是不透明的；其目的是建立標準格式，以標準化方式指定資源ID和中繼資料，以將資源ID傳送至不同的MVPD。

>[!NOTE]
>
>如果使用者選擇僅限管道使用的MVPD，則Adobe Pass驗證只會擷取管道標題（上述範例中為「TNT」），並只將標題傳給MVPD。

### MVPD與程式設計師中繼資料交換流程 {#mvpd-progr-exchange-flow}

Adobe Pass驗證會進行下列假設：

* MVPD會傳送最大評分作為SAML回應的一部分
* 此資訊會儲存為驗證權杖的一部分
* Adobe Pass驗證提供的API可讓程式設計師擷取此資訊
* 程式設計師在其網站或應用程式上實作此功能（例如，隱藏超過使用者最高評等的視訊）

```XML
<saml:Assertion ID="pfxec5f92e0-8589-3fc3-c708-f4fb8e2fad59"
                 IssueInstant="2010-07-20T10:05:41Z" Version="2.0"
                 xmlns:xs="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:AttributeStatement>
        <saml:Attribute
                Name="MaxTVRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">tv-ma</saml:AttributeValue>
        </saml:Attribute>
        <saml:Attribute
                Name="MaxMovieRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">nc-17</saml:AttributeValue>
        </saml:Attribute>
    </saml:AttributeStatement>
</saml:Assertion>
```

### 附註 {#notes-mvpd-progr-metadata-exch-flow}

**資源標準化及驗證。**&#x200B;資源ID可以純字串或MRSS字串形式傳遞。 程式設計師可以決定使用純字串格式或MRSS，但必須事先與MVPD達成協定，讓MVPD知道如何處理該資源。

**資源ID和中繼資料規格。** Adobe Pass驗證使用具有Media RSS副檔名的RSS標準來指定資源及其中繼資料。 Adobe Pass驗證結合Media RSS擴充功能可支援各種中繼資料，例如家長監護（透過`<media:rating>`）或地理位置(`<media:location>`)。

Adobe Pass驗證也可針對需要RSS的MVPD，支援從舊版頻道字串到對應RSS資源的透明轉換。 另一方面，Adobe Pass驗證支援將RSS+MRSS轉換為純頻道標題，適用於僅限頻道的MVPD。

**Adobe Pass驗證可確保與現有整合的完全回溯相容性。**&#x200B;也就是說，對於使用管道層級驗證的程式設計師而言，Adobe Pass驗證在將管道ID傳送至瞭解該格式的MVPD之前，會注意以必要的格式封裝它。 反之亦然：如果程式設計師以新格式指定其所有資源，而授權僅具有管道層級授權的Adobe Pass時，MVPD驗證會將新格式轉譯為簡單的管道字串。

## 使用者中繼資料使用案例 {#user-metadata-use-cases}

隨著更多MVPD進行法律安排並新增功能，使用案例會不斷變更和擴充。 以下是可用於的使用者中繼資料的範例。

* [MVPD使用者ID](#mvpd-user-id)
* [家庭ID](#household-user-id)
* [郵遞區號](#zip-code)
* [最大評分（家長監護）](#max-rating-parental-control)
* [頻道排列](#channel-line-up)

### MVPD使用者ID {#mvpd-user-id}

* 如MVPD所提供
* 並非使用者的實際登入資訊，因為此資訊會由MVPD進行雜湊處理
* 可用於指出特定使用者的或問題
* 已加密
* MVPD支援：所有MVPD

### 家庭使用者ID {#household-user-id}

* 可提供良好的量度資訊
* 已加密
* MVPD支援：部分MVPD

### 郵遞區號 {#zip-code}

* 使用者的帳單郵遞區號
* 主要用於強制實施體育賽事凍結期間規則
* 可隨附AuthZ回應以進行快速更新
* MVPD支援：部分MVPD

### 最大評分（家長監護） {#max-rating-parental-control}

* AuthN初始，加上AuthZ重新整理
* 從UI篩選內容
* MPAA或VChip評等
* MVPD支援：部分MVPD

### 頻道排列 {#channel-line-up}

* MVPD可提供使用者有權檢視的管道清單
* 允許快速UI繪圖
* OLCA規格允許將此作為AuthN回應中的AttributeStatement
* 支援MVPD：部分MVPD

<!--
>[!RELATEDINFORMATION]
>
>* [Proxy MVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Content Metadata Exhange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [OLCA AuthN / AuthZ Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank}
>* [User Metadata (Programmer Integration Guide)](/help/authentication/user-metadata-feature.md)
-->
