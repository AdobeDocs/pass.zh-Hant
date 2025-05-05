---
title: REST API v2 概述
description: REST API v2 概述
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: a02ba4ca1b6579781e40ecd0d12dbfdd23ea7398
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# REST API v2 概述 {#rest-api-v2-overview}

>[!IMPORTANT]
>
> 確保您隨時了解產品公告[&#128279;](/help/authentication/product-announcements.md)頁面中匯總的最新Adobe Pass Authentication產品公告和停用時程表。

您想提高TVE應用的成本效益嗎？

您是否希望減少在多個平臺上支援 TVE 應用程式所需的開發時間和資源？

您想確保跨平台的一致使用者體驗嗎？

您是否希望減少維護工作並簡化更新、錯誤修復或改進的提供？

## REST API V2 簡介 {#rest-api-v2-introduction}

Adobe Pass Authentication很高興宣佈REST API V2正式推出，旨在提升使用者體驗，並簡化與Pass服務的整合。

我們對於新REST API的可能性感到興奮，這代表我們的平台向前邁出了一大步，並為新功能和應用程式流程敞開了大門。

## 新增功能 {#rest-api-v2-whats-new}

跨所有平台的獨特實施

客戶的應用程式現在可以跨平台使用相同的實施，讓您更輕鬆地啟動新功能或維護即時應用程式。

### 跨裝置SSO {#rest-api-v2-cross-device-sso}

REST API V2可讓驗證工作階段安全地在不同裝置之間傳遞。 只要在裝置之間傳遞工作階段，使用者就可以在行動裝置上進行驗證，並在已連線電視的裝置上串流視訊，而不需要重新驗證。

### 多個作用中的驗證工作階段 {#rest-api-v2-multiple-active-authentication-sessions}

現在可以使用不同的作用中MVPD工作階段，客戶可視需要選擇在TempPass與一般MVPD整合之間切換。

### 增強的安全性機制 {#rest-api-v2-enhanced-security-mechanism}

動態使用者端註冊是用於所有流程與功能的安全性機制。 它允許對客戶的應用程式進行更安全和精細的控制，並且可以在所有平臺上註冊應用程式。

### 改進了性能，縮短了響應時間 {#rest-api-v2-improved-performance}

增強的快取機制可減少對 MVPD 流量，從而縮短回應時間並減少延遲。 總體而言，在視訊開始之前，API 調用的數量會減少。

### 增強了所有流上的錯誤代碼 {#rest-api-v2-enhanced-error-codes}

現在，所有的Pass流程都可使用相同格式的進階錯誤碼，以及其他詳細資訊，引導應用程式改善整體使用者體驗。

### 改進了對所有身份驗證會話的控制 {#rest-api-v2-improved-control}

新的 REST API V2 允許同時對多個經過身份驗證的工作階段執行作。

### 降低維護成本 {#rest-api-v2-reduce-maintenance-costs}

所有回應和錯誤資訊現已標準化。

## 什麼是下一個？ {#rest-api-v2-whats-next}

目前通過 SDK 或 REST 調用使用我們 API 的所有客戶都可以繼續這樣做，因為我們計劃在 2025 年底之前繼續提供支援。

不過，所有未來開發將以REST API V2為基礎。 我們強烈建議您開始移轉程式，以受益於最新的Adobe Pass功能。

## 想要進一步瞭解嗎？ {#rest-api-v2-want-to-learn-more}

若要開始使用，請瀏覽我們的公開檔案：

- [網路研討會](#rest-api-v2-webinar)
- [字彙表](rest-api-v2-glossary.md)
- [檢查清單](rest-api-v2-checklist.md)
- [常見問答](rest-api-v2-faqs.md)
- [API](apis/rest-api-v2-apis-overview.md)
- [流程](flows/rest-api-v2-flows-overview.md)
- 逐步指南
- 附錄
- [最低系統需求](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

我們的專用支持團隊也可以幫助您解決您可能需要的任何問題或技術説明。

### 網路研討會 {#rest-api-v2-webinar}

觀看有關新 REST API V2 的網路研討會，其中我們概述了新特性和功能，並現場演示了 REST API V2 的實際運行情況。

>[!VIDEO](https://video.tv.adobe.com/v/3457461/?quality=12&learn=on)

## 想要嘗試REST API V2嗎？ {#rest-api-v2-want-to-try}

您現在可以透過[Adobe Developer](https://developer.adobe.com/adobe-pass/)網站的產品專屬頁面，探索REST API V2。
