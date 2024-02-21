---
title: Adobe Pass Authentication JavaScript 4.7.0發行說明
description: Adobe Pass Authentication JavaScript 4.7.0發行說明
source-git-commit: 9c491478a5560d1f9208e786de285733997bb24b
workflow-type: tm+mt
source-wordcount: '121'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.7.0發行說明 {#javascript-sdk-470-release-notes}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

此頁面說明此版本的新功能、變更和已知問題：

## 建置編號 {#build-no-javascript-sdk-470}

Adobe Pass驗證： JavaScript 4.7.0

發行日期： **02/27/2024 - 02/29/2024**

## 版本總覽 {#overview-javascript-sdk-470}

* 移除Access Enabler JavaScript SDK 2.0.1版已棄用的安全漏洞。
不再支援下列URL，且將傳回HTTP 410狀態代碼：
   * https://entitlement.auth.adobe.com/entitlement/AccessEnabler.js
   * http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js
   * https://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.js
   * http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.js

## 發行套件 {#rel-pkg-javascript-sdk-470}

生產URL為： https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

測試URL為：https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js