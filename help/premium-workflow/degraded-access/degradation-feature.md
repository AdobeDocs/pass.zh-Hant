---
title: 退化特徵
description: 退化特徵
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# 退化特徵 {#degradation-feature}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

在即時體育和重大活動的動態世界中，確保順暢的檢視者體驗至關重要。 發生這些事件期間的高流量可能會讓多頻道視訊播放經銷商(MVPD)驗證和授權端點不知所措，導致延遲或中斷。

Adobe Pass驗證使用其&#x200B;**降級功能**&#x200B;解決這些挑戰，此解決方案可暫時略過特定MVPD驗證和授權端點。 此功能在流量尖峰事件期間尤其有用，因為尖峰事件的回應時間可能會因MVPD系統負載過重而降低。

**效能降低功能**&#x200B;對程式設計師而言是重要的保障，可確保服務的連續性。 雖然其主要對象包括直播體育和新聞頻道，但其公用程式可延伸至任何程式設計師，以降低MVPD端點造成的中斷風險。

>[!IMPORTANT]
>
> 降級API是進階功能，目前需要Adobe的授權。

透過套用降級規則，程式設計師可以暫時啟用自動驗證或自動授權，確保在套用降級期間持續存取內容。 降解動作一律會由程式設計師根據與MVPD預先安排的協定來起始。 雖然Adobe目前不會直接觸發效能降低，但若已建立服務等級協定(SLA)，未來的功能可能會包括主動式管理。

此功能設計與[使用監控API](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md)搭配使用，根據與MVPD的預先合約，Adobe Pass驗證提供強大的工具，在關鍵時刻平衡使用者體驗、可靠性和操作控制。

>[!IMPORTANT]
>
> 此功能不允許略過Adobe Pass驗證服務本身。 如果Adobe Pass驗證無法使用，則本服務中沒有方便使用者存取的內建機制。 在這種情況下，網站或應用程式可能會選擇實作自己的替代路由解決方案，以維持內容傳遞。

## 降級API存取 {#degradation-api-access}

存取[降級API](#degradation-api)之前，您必須完成動態使用者端註冊(DCR)程式中的必要步驟。 此強製程式可確保您具備必要的存取Token，能與降級API互動。

如需完整的指示，請參閱[Dynamic Client註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)檔案。

## 降級API {#degradation-api}

降級API是RESTful API，可讓程式設計師管理特定MVPD的降級規則。 API提供了方法來啟用、移除以及擷取作用中降級規則的狀態。

若要深入瞭解Degradation API，請參閱下列Zendesk檔案[Adobe Pass驗證 | 降級API v3](https://tve.zendesk.com/hc/en-us/articles/33912526308372-Adobe-Pass-Authentication-Degradation-API-v3)，並尋找要下載的PDF檔案。

## REST API V2 {#rest-api-v2}

運用降級功能需要實作程式碼更新，以修改您所有位置的電視(TVE)應用程式與Adobe Pass驗證[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)互動的方式。

如需這些更新和相關工作流程的完整指南，請參閱[降級存取流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)檔案。
