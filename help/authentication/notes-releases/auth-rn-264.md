---
title: Adobe Pass Authentication 2.64發行說明
description: Adobe Pass Authentication 2.64發行說明
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---

# Adobe Pass Authentication 2.64發行說明 {#authn-264-rn}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#ss-web-clients}

* [建置編號](#build-no-264)
* [新功能](#new-featres-264)
* [錯誤修正](#bug-fixes-264)

### 建置編號 {#build-no-264}

Adobe Pass驗證： adobe-pass-**2.64**

發行日期： **11/08/2022 - 11/10/2022**

### 新功能 {#new-featres-264}

* 基礎建設更新，旨在縮短伺服器回應時間，提升系統整體效能。
* 新平台識別機制的改進。
* 能夠封鎖來自MVPD的未經請求的驗證回應，其中SAML判斷提示中缺少&quot;in_response_to&quot;引數。

### 錯誤修正 {#bug-fixes-264}

* 修正部分舊版TempPass權杖格式不正確的問題。
* 已解決第二個畫面驗證流程上的次要問題。
