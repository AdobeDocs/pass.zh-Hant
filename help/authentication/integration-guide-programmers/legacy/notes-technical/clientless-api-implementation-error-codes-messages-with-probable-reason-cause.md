---
title: 無使用者端API實施 — 錯誤代碼/包含可能原因/原因的訊息
description: 無使用者端API實施 — 錯誤代碼/包含可能原因/原因的訊息
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# （舊版）無使用者端API實作 — 錯誤代碼/包含可能原因/原因的訊息 {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

</br>


## 錯誤：未獲授權

### 原因：

1. post中缺少授權標頭
1. 授權標頭問題 — 檢查要求時間是否以毫秒為單位。

## 錯誤：驗證期間為SC 400

### 原因：

1. 伺服器找不到針對特定請求者和環境建立的註冊代碼。
1. 您可能會遇到跨網域指令碼問題
1. /etc/hosts檔案中應新增適當的偽造

## 錯誤： 400錯誤請求

### 原因：

1. POST/GET的格式錯誤url
1. SAMLAssertionParserException — 無法在Adobe端解密加密的SAML宣告

## 錯誤： 403禁止

### 原因：

1. 太多快速要求 — API管理功能，可防止DoS攻擊。
2. 如果使用先決條件環境，則新增詐騙，否則請確認已從/etc/hosts檔案中移除詐騙

## 錯誤：無法登入MVPD頁面

### 原因：

1. 使用者名稱和密碼不符
2. 登入可能已停用
3. 檢查登入是用於生產或測試


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
