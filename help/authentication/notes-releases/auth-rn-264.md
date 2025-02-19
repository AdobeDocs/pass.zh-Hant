---
title: Adobe Pass Authentication 2.64發行說明
description: Adobe Pass Authentication 2.64發行說明
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Adobe Pass Authentication 2.64發行說明 {#authn-264-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-264}

* [建置編號](#build-number-264)
* [版本總覽](#release-overview-264)

### 建置編號 {#build-number-264}

Adobe Pass驗證： adobe-pass-**2.64**

發行日期： **11/08/2022 - 11/10/2022**

### 版本總覽 {#release-overview-264}

* 基礎建設更新，旨在縮短伺服器回應時間，提升系統整體效能。
* 新平台識別機制的改進。
* 能夠封鎖來自MVPD的未經請求的驗證回應，其中SAML判斷提示中缺少&quot;in_response_to&quot;引數。

#### 錯誤修正

* 修正部分舊版TempPass權杖格式不正確的問題。
* 已解決第二個畫面驗證流程上的次要問題。
