---
title: Adobe Pass Authentication 3.7.0發行說明
description: Adobe Pass Authentication 3.7.0發行說明
source-git-commit: 89b5fbd8e8510cbf84ce7908e8cf86551e7a0cb9
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Adobe Pass Authentication 3.7.0發行說明 {#authn-370-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-370}

* [建置編號](#build-number-370)
* [版本總覽](#release-overview-370)

### 建置編號 {#build-number-370}

Adobe Pass驗證： adobe-pass-**3.7.0.2**\
發行日期： **05/12/2026 - 05/14/2026**

### 版本總覽 {#release-overview-370}

此版本專注於MVPD整合升級、錯誤修正和TVE儀表板改善。

#### MVPD整合

* 新增PKCE對OAuth2型MVPD驗證的支援。

#### 增強功能

* 已發行TVE Dashboard 1.5.1版。

#### 錯誤修正

* 修正造成Apple SSO忽略某些MVPD設定不符的問題。
* 修正因回應標頭中的無效字元而導致授權拒絕出現HTTP 500錯誤的問題。
