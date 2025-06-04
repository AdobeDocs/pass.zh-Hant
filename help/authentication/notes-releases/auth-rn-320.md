---
title: Adobe Pass Authentication 3.2.0發行說明
description: Adobe Pass Authentication 3.2.0發行說明
exl-id: 4008512a-3155-4d96-9988-da0b0a496612
source-git-commit: 13b0bb640aa599109e8c2f68d1e16fbdc3840951
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Adobe Pass Authentication 3.2.0發行說明 {#authn-320-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-320}

* [建置編號](#build-number-320)
* [版本總覽](#release-overview-320)

### 建置編號 {#build-number-320}

Adobe Pass驗證： adobe-pass-**3.2.0**

發行日期： **06/10/2025 - 06/12/2025**

### 版本總覽 {#release-overview-320}

#### REST API v2

* 已針對[工作階段API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)回應中缺少引數的情況新增原因`missing_parameters_fallback`。
* 新欄位「裝置」已新增至[工作階段API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)回應。

#### 新功能

* 展開退化API ，新增在單一呼叫中為多個MVPD套用退化規則的功能

#### 錯誤修正

* 修正當使用者中繼資料處理失敗時，驗證流程無法成功完成的問題。
* 修正未正確計算授權原因的問題。
* 修正設定檔回應中不存在upstreamUserID的問題。
* 修正造成驗證請求不包含REST API V2起始驗證請求的範圍設定資訊的問題。
