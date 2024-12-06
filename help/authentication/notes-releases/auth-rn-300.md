---
title: Adobe Pass Authentication 3.0發行說明
description: Adobe Pass Authentication 3.0發行說明
exl-id: 9284151a-8458-44a3-937b-35f379ca0e4e
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Adobe Pass Authentication 3.0發行說明 {#pt-authn-300-rn}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-300}

* [建置編號](#build-number-300)
* [版本總覽](#release-overview-300)

### 建置編號 {#build-number-2651}

Adobe Pass驗證： adobe-pass-**3.0**
發行日期： **09/10/2024 - 09/12/2024**

### 新功能 {#new-features-300}

#### REST API v2 {#rest-apis}

##### 程式碼

* 從adobe-pass-**3.0**&#x200B;版開始，新的和現有的使用者端應用程式可以整合或移轉至新的REST API v2，以受益於最新的Adobe Pass功能。

##### 檔案

* 若要開始使用新的REST API v2，請參閱下列檔案：
   * [REST API v2 - API — 概觀](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [REST API v2 — 流程 — 概觀](../integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
* REST API v1之公開檔案的URL已變更，請參閱下列檔案：
   * [REST API v1 - API — 概觀](../integration-guide-programmers/legacy/rest-api-v1/apis/rest-api-overview.md)
   * [REST API v1 - API — 參考](../integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)

##### 工具

* 若要嘗試新的REST API v2，請參閱來自[Adobe Developer](https://developer.adobe.com/adobe-pass)網站的新Adobe Pass驗證頁面。

### 錯誤修正 {#bug-fixes-300}

* 修正登出請求中出現時未使用重新導向URL引數的問題。
