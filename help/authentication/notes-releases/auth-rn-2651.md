---
title: Adobe Pass Authentication 2.65.1發行說明
description: Adobe Pass Authentication 2.65.1發行說明
exl-id: 28d112db-b038-4d11-93c5-d6ab67a29700
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 0%

---

# Adobe Pass Authentication 2.65.1發行說明 {#authn-2651-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-2651}

* [建置編號](#build-number-2651)
* [版本總覽](#release-overview-2651)

### 建置編號 {#build-number-2651}

Adobe Pass驗證： adobe-pass-**2.65.1**

發行日期： **06/20/2023 - 06/22/2023**

### 版本總覽 {#release-overview-2651}

此版本新增下列內容：

自此版本開始，我們將引進在所有API中新增速率限制規則的技術支援，以保護整體生態系統，避免錯誤使用。

初始目標是監視應用程式行為，以建立適當的限制值，在這個版本中不會套用任何限制。 如果特定應用程式產生的流量超過某些限制，我們會與每位客戶合作，驗證實施情形。

我們驗證從所有客戶應用程式產生的流量後，今年稍後將套用速率限制規則。
