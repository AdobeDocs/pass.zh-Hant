---
title: Adobe Pass Authentication 3.5.0發行說明
description: 瞭解此版本的新功能、變更和已知問題。
source-git-commit: 6ff46a124f5f3c78173028ae3efed68d71ee6e41
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---


# Adobe Pass Authentication 3.5.0發行說明

上次更新日期：2025年12月9日星期二00:00:00 GMT+0000 （國際標準時間）

* 主題：
* 驗證

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-350}

* [建置編號](#build-number-350)
* [版本總覽](#release-overview-350)

### 建置編號 {#build-number-350}

Adobe Pass驗證： adobe-pass-**3.5.0**\
發行日期： **12/09/2025 - 12/11/2025**

### 版本總覽 {#release-overview-350}

#### REST API v2

* 在ESM （以下稱「api」）中新增維度，以協助客戶建置可根據實作型別區分事件的報表： SDK、REST V1或REST V2。

#### 錯誤修正

* 修正TVE儀表板中設定的自訂降級訊息未出現在REST API V2錯誤詳細資料中的問題。
* 修正REST API V2中，已驗證的設定檔過期時，未傳回`authenticated_profile_expired`錯誤碼的問題。
* 修正REST API V2中授權延遲計算和預檢TTL值不正確的問題。
* 修正DCR權杖過期時，傳回回應格式不一致的問題。
