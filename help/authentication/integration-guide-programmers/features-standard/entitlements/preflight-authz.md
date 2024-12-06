---
title: 預檢授權
description: 預檢授權
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 0%

---

# 預檢授權 {#preflight-authorization}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>

## 概觀 {#overview}

此功能為多個資源提供輕量授權檢查。 此輕量型檢查的目的是裝飾UI （例如，使用鎖定和解鎖圖示表示存取狀態）。 預檢授權儘可能簡便和高效，因此單一API呼叫會產生資源清單的授權狀態。 請注意，此功能在授權資源方面並不權威。

在允許播放之前，仍必須進行`getAuthorization(resource)`或`checkAuthorization(resource)`呼叫。

預檢授權也提供不同使用案例的支援，在這種情況下，程式設計師需要請求多個資源ID的授權，才能允許播放一個媒體內容專案。 程式設計師可以對所需資源執行初始預檢檢查，如果不符合業務條件，則視回應而定，可能會提前失敗。

如需支援預檢授權的MVPD清單，請參閱[MVPD預檢授權](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md#preflight_support_list)頁面。

>[!NOTE]
>
> 請注意，此功能用於沒有完整預檢授權支援的MVPD時，需要事先與Adobe和MVPD商定。 在這些MVPD上使用預檢授權將屬於[這裡](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md#intro)所述的「最壞情況」，並可能導致效能問題和緩慢的回應時間。</br>
> 此外，請注意，Adobe **必須明確同意使用**&#x200B;超過5個資源的預檢授權。

## AccessEnabler Preflight API {#AE_pre_api}

AccessEnabler會公開API/回呼函式配對，以實作預檢授權。 API呼叫採用單一引數，由資源清單組成。 回呼函式會採用代表實際授權資源的單一引數。 下列範例為ActionScript，但呼叫適用於AccessEnabler的所有風格。

### checkPreauthorizedResources(Array：resources)：void {#checkPreauthRes}

在AccessEnabler物件上呼叫此函式，以要求資源清單的授權狀態。

resources引數是應檢查其授權的資源清單。 清單中的每個元素應為代表資源ID的字串。 資源ID受到與`getAuthorization()`呼叫中的資源ID相同的限制，也就是說，這是程式設計師與MVPD或媒體RSS片段之間建立的公認值。 請注意，Adobe Pass驗證不會以任何方式管理資源，除了根據MVPD實際支援的內容而可轉換資源格式的精簡中繼層。

### preauthorizedResources(Array：authorizedResources) {#preauthRes}

這是一個回呼函式，必須在程式設計師的上層應用程式中實作。 AccessEnabler會在計算授權資源清單之後呼叫此函式。


### JS範例

```javascript
    checkPreauthorizedResources(["CNBC","MSNBC"]);
    ...
    preauthorizedResources() {
        var resource = arguments[0];
        for(i in resources) {
            // Do things with resource list
            // such as decorate UI
            ...
        }
    }
```

## 實作資訊 {#details}

- [使用ChannelID預檢](#preflight_using_channelID)
- [POST/預先授權](#post)
- [儲存](#storage)

API呼叫會嘗試在使用者端的本機儲存空間中尋找目前使用者的已授權資源快取清單。 如果沒有快取清單，則會對AdobePass伺服器進行HTTPS呼叫，以擷取清單。

快取機制會略過網路呼叫，改善後續呼叫的效能時間。 此外，快取清單可在驗證程式中預先填入。  （如需設定此情境的詳細資訊，請參閱MVPD整合指南授權一節中的[Preflight授權整合](/help/authentication/integration-guide-mvpds/authz-usecase.md#preflight_authz_int)）。

此外，快取的資源清單可能會用來最佳化授權流程，也就是說，如果存在快取的資源清單，`checkAuthorization()`可以在執行網路呼叫之前檢查它。 如果資源不在預先授權的資源清單中，則檢查可能會失敗，而無需呼叫Adobe Pass驗證伺服器。


### 使用ChannelID預檢 {#preflight_using_channelID}

從Adobe Pass Authentication 2.4.1版開始，預檢流程的工作方式如下：

1. 在驗證期間，Adobe Pass驗證會從MVPD的SAML回應中讀取`channelIID`元素，並使用此值來設定驗證權杖中的`authorizedResources`元素。
1. 在`checkPreauthorizedResources()` API函式內，Adobe Pass驗證會檢查是否已設定`authorizedResources`專案。
1. 如果設定`authorizedResources`元素，Adobe Pass驗證會讀取該值，並在`authorizedResources`元素的資源清單與從`checkPreauthorizedResources()`引數收到的資源清單之間執行交集。  此交集的結果是預先授權資源的最終清單。
1. 如果未設定`authorizedResources`專案，請執行先前實作的流程，其中從`checkPreauthorizedResources()`引數收到的資源清單會傳遞給PreAuthorizationServlet。 此servlet會對MVPD端點執行授權呼叫，並傳回預先授權的資源清單。

### 使用ChannelID預檢的範例

以下範例顯示了一個範例頻道組合。 請注意，用於傳送通道清單的屬性名稱可能因一個MVPD而不同：

```XML
    <saml:Attribute Name="visible_channels" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
        <saml:AttributeValue xsi:type="xs:string">MSNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FBN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FNC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TNT</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TBS</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TRUTV</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TOON</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">HBO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">MAX</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">EPIXHD</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">BTN-BTN2GO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">SPEED-SPEED2</saml:AttributeValue>
    </saml:Attribute>
```


驗證專案中的`authorizedResources`專案如下所示：

```JSON
    <authorizedResources>
        <authorizedResource resourceID="MSNBC"/>
        <authorizedResource resourceID="CNBC"/>
        <authorizedResource resourceID="FBN"/>
        <authorizedResource resourceID="FNC"/>
        <authorizedResource resourceID="TNT"/>
        <authorizedResource resourceID="TBS"/>
        <authorizedResource resourceID="CNN"/>
        <authorizedResource resourceID="TRUTV"/>
        <authorizedResource resourceID="TOON"/>
        <authorizedResource resourceID="HBO"/>
        <authorizedResource resourceID="MAX"/>
        <authorizedResource resourceID="EPIXHD"/>
        <authorizedResource resourceID="BTN-BTN2GO"/>
        <authorizedResource resourceID="SPEED-SPEED2"/>
    </authorizedResources>
```

程式設計師執行`checkPreauthorizedResources()` API呼叫，傳遞下列引數清單：</span>

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- 「天通電視」
- &quot;fbc-fox&quot;

目前的Preflight實作會與`authorizedResources`專案的資源清單執行交集，並傳回此清單：

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- 「天通電視」



**注意：**&#x200B;請注意，交集不區分大小寫。



### POST/預先授權 {#post}

當沒有快取、授權的資源清單存在時，AccessEnabler會自動執行此呼叫。


#### 請求 {#req}

| 引數 | 型別 | 必填 | 說明 |
| --- | --- | --- | --- |
| `authentication_token` | 字串 | 是 | 驗證權杖。 |
| `resource_id` | 字串 | 是 | 單一資源。 您可以為`checkPreauthorizedResources()` API呼叫中提供的資源陣列的每個元素多次指定此專案。 |


**注意：**可設定要求的資源數目上限。
**最大預設值為5.**


#### 回應 {#resp}

預先授權servlet傳回的回應格式如下：

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <resources>
      <resource>
        <id>TNT</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>TBS</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>CNN</id>
        <authorized>false</authorized>
      </resource>
    </resources>
```

### 儲存 {#storage}

AccessEnabler從服務提供者取得的預先授權資源清單。 此資源清單：

- 會與AuthN和AuthZ權杖一起儲存
- 只要使用者位於相同網站，或直到
AuthN權杖過期
- 每次使用者登陸新Adobe Pass時都會重新擷取
驗證整合網站

每個專案都包含使用者預先授權的資源ID。

例如：


| 資源ID | 已授權 |
| ----------- | ---------- |
| CNN | true |
| TNT | false |
| ... | ... |


此清單名為「預先授權快取」。

#### 流量 {#flow}

1. 程式設計師的應用程式/網站進行`checkPreauthorizedResources(resourceList)`呼叫。
1. AccessEnabler會驗證授權資源的驗證Token：
   1. 如果驗證Token包含授權資源 — 此清單是授權清單，不應進行呼叫以取得此資訊。 在驗證Token上的授權資源清單中搜尋resourceList中的資源，而且只有`preauthorizedResources()`回呼會傳回找到的資源。
   1. 如果驗證Token DOES NOT contain authorized resources - `resourceList`會與預先授權快取中的資源清單比較。
      1. 如果清單包含相同的資源 — 這表示已呼叫伺服器，而且回應已在預先授權快取中。 `preauthorizedResources()`回呼只會傳回已授權的資源。
      1. 如果清單不包含相同的資源 — 使用者端需要呼叫伺服器以取得resourceList中資源的授權狀態。 系統會擷取回應，並儲存在預先授權快取中，完全取代舊資源。 `preauthorizedResources()`回呼只會傳回已授權的資源。


#### 清單擷取 {#listRetrieve}

每當呼叫`checkPreauthorizedResources()`時，就會根據預先授權快取檢查要驗證授權的資源清單。 如果清單包含相同的資源集，則不會呼叫服務提供者，因為觸發`preauthorizedResources()`回呼所需的所有資源都已在快取中。


#### logout() {#logout}

登出時會清空預先授權快取。


## 相依性 {#depends}

Preflight API的效能取決於特定的MVPD實作。  如需實作選項，請參閱MVPD Integration Guide的Authorization一節中的[Preflight Authorization Integration](/help/authentication/integration-guide-mvpds/authz-usecase.md#preflight_authz_int)。


## 安全性 {#security}

使用者端API可供所有程式設計師使用。

實作使用HTTPS作為傳輸，但為了進行較輕的呼叫，不採用額外的安全措施（無簽署、無FAXS）。

**注意：**&#x200B;請勿以授權方式使用此API來決定是否應授與使用者對受保護資源的存取權。 此API的目的是為了提供UI裝飾和/或預先決定業務決策。 `getAuthorization()`和`checkAuthorization()`呼叫一律應在允許播放之前進行。


## 相容性 {#compat}

AccessEnabler的所有風格皆支援此功能： AS、JS、AIR、iOS、Android、Xbox （在第2熒幕驗證流程中）。

預檢授權不支援包含CDATA區段的預先授權資源。 目前預檢系統的重點是支援頻道層級篩選。 包含CDATA區段的資源可能是資產層級資源。 只要不包含CDATA，Preflight就不會支援簡單的`mrss`資源，以進行通道層級的預先授權。

## 與其他功能整合 {#integ_w_other_features}

如果資源不在預先授權的資源清單中，授權流程可能會發生可能的最佳化，以便快速失敗。

在此最佳化中，伺服器預檢端點會與降級機制整合。

如果MVPD和請求者的「AuthN All」規則已準備就緒，則預檢端點會直接映象回請求中收到的所有資源。

如果至少為請求中存在的其中一個資源啟用「AuthZ All」，則相同情況會發生。

<!--
## Related Information

  - `checkPreauthorizedResources()`
      - [iOS](#checkPreauth)
      - [Android](#checkPreauth)
      - [JavaScript](#checkPreauthRes)
      - [ActionScript](#checkPreauthRes)
  - [Xbox](#2nd%20Screen%20Authentication)
  - [MVPD Integration Guide: Preflight AuthZ](#)
-->
