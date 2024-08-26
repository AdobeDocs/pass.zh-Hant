---
title: 降級API概觀
description: 降級API概觀
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---


# 降級API概觀 {#degradation-api-overview}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 在使用降級API之前，請確定您符合下列先決條件：
>
> * 依照[擷取使用者端認證](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API檔案中的說明取得使用者端認證。
> * 依照[擷取存取權杖](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) API檔案中的說明取得存取權杖。
>
> 請參閱[Dynamic Client Registration Overview](./dcr-api/dynamic-client-registration-overview.md)檔案，以取得有關如何建立註冊的應用程式及下載軟體陳述式的詳細資訊。

## 一般資訊 {#general_info}

>[!NOTE]
>
>此API並非一般可用。 如需可用性更新資訊，請聯絡您的Adobe代表。

此功能可讓整合中的三大方(程式設計人員、MVPD和Adobe)暫時略過特定的MVPD驗證和授權端點。 通常，是程式設計師啟動此類動作，但無論誰觸發降級事件，該動作取決於先前與受影響MVPD商定的安排。

此功能的主要使用案例會在直播運動或大型活動期間發生。 在這種高流量情況下，特定MVPD端點的負載可能會變得太高，導致使用者的回應時間非常長。 為了在這種情況下保留良好的使用者體驗，程式設計師可能會決定觸發可暫時自動驗證/自動授權使用者的降級規則，或透過從可用的MVPD清單中移除它來停用MVPD。

降級規則只會套用一段固定的時間。 雖然此功能的主要客戶是體育頻道和即時新聞頻道，但任何程式設計師都可能想要存取此功能，因為MVPDs服務確實會不時關閉。

降級記事：

 — 此功能設計為與使用狀況監控API搭配使用，可提供每個MVPD的驗證和授權數目、平均授權延遲，以及完整服務概覽所需其他度量的即時資訊。
 — 此功能不允許略過AdobePrimetim驗證服務。 如果Adobe Pass驗證失敗，服務內沒有機制可用來讓使用者檢視內容。 不過，網站或應用程式可自行繞過Adobe Pass驗證。
-Adobe目前不會直接觸發效能降低 — 決定必須一律由已透過MVPD同意此類條件的特定程式設計師負責。 未來，如果能透過MVPD達成協定(SLA保護)，Adobe Pass驗證可能會主動觸發降級規則。

<!--
## Related Information {#related}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
