---
title: Adobe Pass Authentication 2.66發行說明
description: Adobe Pass Authentication 2.66發行說明
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# Adobe Pass Authentication 2.66發行說明 {#authn-266-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-266}

* [建置編號](#build-number-266)
* [版本總覽](#release-overview-266)

### 建置編號 {#build-number-266}

Adobe Pass驗證： adobe-pass-**2.66.0.1**

發行日期： **07/11/2023 - 07/13/2023**

### 版本總覽 {#release-overview-266}

在此版本中，我們持續進行新REST API的內部更新。

#### 錯誤修正

* 修正了基於SAML的MVPD的登出流程，其中登出請求中缺少RelayState引數。 我們將在發行後鎖定設定更新，以還原受影響的MVPD的登出流程。
* 新增在SOAP授權端點的設定中更新SSL憑證的功能。
* 修正在某些ESM報表中，程式設計師欄位中記錄不正確資料的極端情況。
