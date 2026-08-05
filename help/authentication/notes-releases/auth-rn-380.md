---
title: Adobe Pass Authentication 3.8.0發行說明
description: Adobe Pass Authentication 3.8.0發行說明
hold: true
source-git-commit: ce9e8de3d69699d03cf68c86be1bb811967501dc
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# Adobe Pass Authentication 3.8.0發行說明 {#authn-380-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-380}

* [建置編號](#build-number-380)
* [版本總覽](#release-overview-380)

### 建置編號 {#build-number-380}

Adobe Pass驗證： adobe-pass-**3.8.0**\
發行日期： **08/11/2026 - 08/13/2026**

### 版本總覽 {#release-overview-380}

此版本專注於Adobe Pass驗證服務的穩定性、增強功能和安全性更新。

#### 錯誤修正

* 修正由於deviceId中的某些無效字元導致V2 API發生HTTP 500錯誤的問題。

#### 增強功能

* 改善重新整理權杖處理以支援滾動權杖續約。
* 增強分析專用次要裝置上的visitorId辨識功能。
* 增強URL引數驗證，以加強安全性控制並改善整體系統完整性。
* TVE Dashboard 1.5.2版，包含微幅的UI改善。
