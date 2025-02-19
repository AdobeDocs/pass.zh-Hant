---
title: Adobe Pass Authentication JavaScript 4.7.0發行說明
description: Adobe Pass Authentication JavaScript 4.7.0發行說明
exl-id: 07f90270-e64a-4c6b-a072-183af0f53352
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.7.0發行說明 {#javascript-sdk-470-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 建置編號 {#build-number-470}

Adobe Pass驗證： JavaScript 4.7.0

發行日期： **02/27/2024 - 02/29/2024**

## 版本總覽 {#release-overview-470}

* 由於安全漏洞，已移除Access Enabler JavaScript SDK的2.0.1版。
  <br/><br/>
不再支援下列URL，且將傳回HTTP 410狀態代碼：
   * https://entitlement.auth.adobe.com/entitlement/AccessEnabler.js
   * http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js
   * https://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.js
   * http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.js

## 發行套件 {#release-package-470}

生產URL為： https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

測試URL為：https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
