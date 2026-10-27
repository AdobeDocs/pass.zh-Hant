---
title: Adobe Pass Authentication 3.10.0發行說明
description: Adobe Pass Authentication 3.10.0發行說明
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: d60b5256f5809b52122d70d04eb873018707fcaf
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%
---
# Adobe Pass Authentication 3.10.0發行說明 {#authn-3100-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-3100}

* [建置編號](#build-number-3100)
* [版本總覽](#release-overview-3100)

### 建置編號 {#build-number-3100}

Adobe Pass驗證： adobe-pass-**3.10.0**\
發行日期： **10/27/2026 - 10/29/2026**

### 版本總覽 {#release-overview-3100}

此版本專注於REST API V2和MVPD設定管理的「單一登入」控制項、使用者中繼資料增強功能和可靠性改善。

#### 增強功能

* 新增服務權杖單一登入(SSO)的整合層級控制。 服務權杖SSO僅在明確啟用整合時使用。
* 新增驗證、授權和預檢期間對自訂使用者中繼資料（例如`packageType`）的支援。 REST API V2授權和預檢決定現在包含已設定的使用者中繼資料，包括快取回應，同時遵循中繼資料存取和加密設定。

#### 錯誤修正

* 修正使用OAuth2 MVPDs透過SAML進行整合時，造成REST API V2 Apple SSO設定檔要求失敗，並出現無效SAML回應錯誤的問題。
* 修正相同資源的預檢和授權決定可能會覆寫彼此的快取到期時間和使用者中繼資料的問題。
* 修正刪除代理的MVPD時，留下未參考的平台設定而無法儲存後續設定的問題。
