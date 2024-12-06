---
title: Adobe Pass Authentication 3.0.3發行說明
description: Adobe Pass Authentication 3.0.3發行說明
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Adobe Pass Authentication 3.0.3發行說明 {#pt-authn-303-rn}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-303}

* [建置編號](#build-number-303)
* [版本總覽](#release-overview-303)

### 建置編號 {#build-number-2651}

Adobe Pass驗證： adobe-pass-**3.0.3**
發行日期： **2024年10月29日至2024年10月31日**

### 新功能 {#new-features-303}

#### REST API v2 {#rest-apis}

##### 程式碼

* REST API V2增強功能(由於Adobe Pass 3.0主要發行版本提供[REST API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md))。
* 已在`/sessions/{code}`上新增`notBefore`和`notAfter`欄位，以傳回驗證碼有效性的相關資訊。
* 改善平台識別功能。

##### 檔案

* 若要從新的REST API v2開始，請參閱[REST API v2概觀](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)檔案。

##### 工具

* 若要嘗試新的REST API v2，請參閱來自[Adobe Developer](https://developer.adobe.com/adobe-pass)網站的新Adobe Pass驗證頁面。
