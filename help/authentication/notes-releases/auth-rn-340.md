---
title: Adobe Pass Authentication 3.4.0發行說明
description: Adobe Pass Authentication 3.4.0發行說明
exl-id: 7a9b8c6d-4e5f-4a3b-8c7d-9e0f1a2b3c4d
source-git-commit: 3a275f64f7f8cffa3bdc0d546c8e2db517840069
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Adobe Pass Authentication 3.4.0發行說明 {#authn-340-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-340}

* [建置編號](#build-number-340)
* [版本總覽](#release-overview-340)

### 建置編號 {#build-number-340}

Adobe Pass驗證： adobe-pass-**3.4.0**
發行日期： **09/09/2025 - 09/11/2025**

### 版本總覽 {#release-overview-340}

#### REST API v2

* 新增對[Experience Cloud ID (ECID)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md)的支援，以改善使用者識別與追蹤功能。
* 已將REST API V2 [設定API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)的標頭AP-Device-Identifier和X-Device-Info變更為選用。

#### 錯誤修正

* 修正在REST API V2 [決定](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)回應的權杖中，MVPD ID和Proxy MVPD ID顛倒的問題。
* 修正REST API V2 [決定](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)回應的權杖中不存在要求者ID的問題。
* 修正將太多資源傳遞至REST API V2 [預先授權決定](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API導致回應中的決定清單空白，而非[增強式錯誤碼](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) **too_many_resources**&#x200B;的問題。

#### 雜項

* 已修補安全性漏洞。