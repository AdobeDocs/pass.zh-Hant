---
title: Adobe Pass Authentication 3.1.0發行說明
description: Adobe Pass Authentication 3.1.0發行說明
exl-id: cf9fc8e2-4b37-4b0a-a6ed-cda1b6738e76
source-git-commit: 0c6aec04ae9df410228730b5bce6ced1aeecd312
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Adobe Pass Authentication 3.1.0發行說明 {#authn-310-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-310}

* [建置編號](#build-number-310)
* [版本總覽](#release-overview-310)

### 建置編號 {#build-number-310}

Adobe Pass驗證： adobe-pass-**3.1.0**

發行日期： **02/25/2025 - 02/27/2025**

### 版本總覽 {#release-overview-310}

#### REST API v2

* 新的`partner_logout`動作名稱和`partner_interactive`動作型別，可區分REST API v2 [登出API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)回應中的一般登出和合作夥伴單一登入。
* 新`reason`欄位，可針對REST API v2 [工作階段API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)與[工作階段SSO API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)回應中的動作名稱提供深入分析。

#### 錯誤修正

* 修正了阻止Spectrum訂閱者透過REST API v2 [驗證API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)進行驗證的問題。
* 修正了無法透過REST API V2 [驗證API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)產生的事件在ESM中正確彙總的問題。
* 修正REST API v2 `notBefore`設定檔API[回應中，使用者設定檔的](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)時間戳記計算錯誤的問題。

#### JavaScript SDK

* 已移除[AccessEnabler JavaScript SDK](authn-rn-javascript-471.md)的3.5.0版。
