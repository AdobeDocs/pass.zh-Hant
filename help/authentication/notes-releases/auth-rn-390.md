---
title: Adobe Pass Authentication 3.9.0發行說明
description: Adobe Pass Authentication 3.9.0發行說明
source-git-commit: 7ec140485418d07e16a181d43b651ea6de331477
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Adobe Pass Authentication 3.9.0發行說明 {#authn-390-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-390}

* [建置編號](#build-number-390)
* [版本總覽](#release-overview-390)

### 建置編號 {#build-number-390}

Adobe Pass驗證： adobe-pass-**3.9.0.1**\
發行日期： **09/08/2026 - 09/10/2026**

### 版本總覽 {#release-overview-390}

此版本專注於REST API V2和ESM量度改善。

#### 增強功能

* 改善REST API V2合作夥伴單一登入，確保針對以OAuth2設定的MVPD傳回有效的驗證要求。
* 改善REST API V2決定功能，在授權失敗時傳回明確的錯誤回應，而非空白回應。
* 改善註冊程式碼的產生機制，避免出現視覺上模糊的字元，讓程式碼更易於閱讀及正確輸入。
* ESM控制面板增強功能支援預檢AuthZ量度。
