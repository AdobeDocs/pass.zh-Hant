---
title: Adobe Pass Authentication 3.0.3發行說明
description: Adobe Pass Authentication 3.0.3發行說明
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Adobe Pass Authentication 3.0.3發行說明 {#authn-303-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-303}

* [建置編號](#build-number-303)
* [版本總覽](#release-overview-303)

### 建置編號 {#build-number-303}

Adobe Pass驗證： adobe-pass-**3.0.3**

發行日期： **10/29/2024 - 10/31/2024**

### 版本總覽 {#release-overview-303}

#### REST API v2

##### 程式碼

* REST API V2增強功能(由於Adobe Pass 3.0主要發行版本提供[REST API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md))。
* 已在`/sessions/{code}`上新增`notBefore`和`notAfter`欄位，以傳回驗證碼有效性的相關資訊。
* 改善平台識別功能。

##### 檔案

* 若要從新的REST API v2開始，請參閱[REST API v2概觀](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)檔案。

##### 工具

* 若要嘗試新的REST API v2，請參閱來自[Adobe Developer](https://developer.adobe.com/adobe-pass)網站的新Adobe Pass驗證頁面。
