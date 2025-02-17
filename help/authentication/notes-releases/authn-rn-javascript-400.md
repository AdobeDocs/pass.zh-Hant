---
title: Adobe Pass Authentication JavaScript 4.0.0發行說明
description: Adobe Pass Authentication JavaScript 4.0.0發行說明
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.0.0發行說明 {#javascript-sdk-400-rn}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

此頁面說明此版本的新功能、變更和已知問題：

## 建置編號 {#build-number-400}

Adobe Pass驗證： JavaScript 4.0.0

發行日期： **07/05/2018**

## 版本總覽 {#release-overview-400}

* 實作對動態使用者端註冊機制的支援，跨平台的統一安全機制。 跨平台的安全存取管理可由程式設計師透過TVE儀表板完成。
* 免除對所有功能使用所有第三方Cookie和幾乎所有第一方Cookie （除了仍使用第一方Cookie的個別化，未來將會移除），因此它更相容於有關第三方Cookie或一般Cookie的新興瀏覽器政策，且更能確保未來安全性(例如 Safari的ITP)。 請注意，先前的3.x版本僅使用第一方Cookie，因此可如預期正常運作，但新瀏覽器版本的發行可能會改變此狀況。
* 支援停用預檢授權檢查的快取。
* 此版本不回溯相容，且以新路徑託管： https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js 。 為了受益於這個新的JS SDK，需要程式設計師的移轉流程以使用新的動態使用者端註冊機制註冊其Web應用計畫並指向新的JS SDK URL。

## 發行套件 {#release-package-400}

生產URL為： https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

測試URL為：https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
