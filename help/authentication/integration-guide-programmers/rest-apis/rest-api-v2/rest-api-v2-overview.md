---
title: REST API V2概觀
description: REST API V2概觀
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: fab5964aeb832d419702b41a6d3bc5676cb3354f
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---

# REST API V2概觀 {#rest-api-v2-overview}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

您要提高TVE應用程式的成本效益嗎？

您想要減少在多個平台上支援TVE應用程式所需的開發時間和資源嗎？

您想要確保跨平台提供一致的使用者體驗嗎？

您想要減少維護工作並簡化提供更新、錯誤修正或改進專案？

## REST API V2簡介

Adobe Pass Authentication很高興宣佈REST API V2正式推出，旨在提升使用者體驗，並簡化與Pass服務的整合。

我們對於新REST API的可能性感到興奮，這代表我們的平台向前邁出了一大步，並為新功能和應用程式流程敞開了大門。

## 新增功能

跨所有平台的獨特實施

客戶的應用程式現在可以跨平台使用相同的實施，讓您更輕鬆地啟動新功能或維護即時應用程式。

### 跨裝置SSO

REST API V2可讓驗證工作階段安全地在不同裝置之間傳遞。 只要在裝置之間傳遞工作階段，使用者就可以在行動裝置上進行驗證，並在已連線電視的裝置上串流視訊，而不需要重新驗證。

### 多個作用中的驗證工作階段

現在可以使用不同的作用中MVPD工作階段，客戶可視需要選擇在TempPass與一般MVPD整合之間切換。

### 增強的安全性機制

動態使用者端註冊是用於所有流程與功能的安全性機制。 它可提供對客戶應用程式更安全、更精細的控制，並可在所有平台上註冊應用程式。

### 提升效能，加速回應時間

增強的快取機制可減少傳入MVPD的流量，改善回應時間並減少延遲。 整體而言，在視訊開始之前，API呼叫的數量會減少。

### 增強所有流程的錯誤碼

現在，所有的Pass流程都可使用相同格式的進階錯誤碼，以及其他詳細資訊，引導應用程式改善整體使用者體驗。

### 改善對所有驗證工作階段的控制

新的REST API V2允許同時對多個已驗證工作階段執行動作。

### 降低維護成本

所有回應和錯誤資訊現在都已標準化。

## 接下來呢？

目前透過SDK或REST呼叫使用我們API的所有客戶可以繼續這樣做，因為我們計畫到2025年底繼續提供支援。

不過，所有未來開發將以REST API V2為基礎。 我們強烈建議您開始移轉程式，以受益於最新的Adobe Pass功能。

## 想要進一步瞭解嗎？

若要開始使用，請瀏覽我們的公開檔案：

- [字彙表](rest-api-v2-glossary.md)
- [常見問答](rest-api-v2-faqs.md)
- [API](apis/rest-api-v2-apis-overview.md)
- [流程](flows/rest-api-v2-flows-overview.md)
- 逐步指南
- 附錄
- [最低系統需求](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

我們專門的支援團隊也會協助您解決任何可能需要的疑問或技術協助。

## 想要嘗試REST API V2嗎？

您現在可以透過[Adobe Developer](https://developer.adobe.com/adobe-pass/)網站的產品專屬頁面，探索REST API V2。
