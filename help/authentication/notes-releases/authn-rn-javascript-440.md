---
title: Adobe Pass Authentication JavaScript 4.4.0發行說明
description: Adobe Pass Authentication JavaScript 4.4.0發行說明
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 0%

---

# Adobe Pass Authentication JavaScript 4.4.0發行說明 {#javascript-sdk-440-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 建置編號 {#build-number-440}

Adobe Pass驗證： JavaScript 4.4.0

發行日期： **06/22/2021**

## 版本總覽 {#release-overview-440}

### 新功能

預檢授權

* 新的預先授權API — 這是將用於預檢授權功能的新API呼叫，其也會為預檢流程引入增強的錯誤報告。
* 此功能必須在Primetime驗證設定上啟用，才能應要求提供。 如需如何啟用此功能的詳細資訊，請聯絡您的TAM。
* 棄用checkPreauthorizedResources API。

平台識別

* 在所有SDK呼叫上新增AP-SDK-Identifier標題，以便更妥善識別SDK型別和版本。

其他

* 內部架構改良。

### 錯誤修正

* 修正同時呼叫setRequestor和getAuthentication時產生的競爭條件。
* 修正了無法在中繼環境中正確載入許可權的問題。
* 已解決導致背景登出流程無法在Safari瀏覽器上完成的問題，因此使用者在頁面重新整理前似乎仍可驗證。 已引入逾時，目前設定為30秒，因此如果在此期間沒有來自Primetime驗證伺服器的回應，SDK將會叫用setAuthenticationStatus回呼。

## 發行套件 {#release-package-440}

生產URL為： https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

測試URL為：https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
