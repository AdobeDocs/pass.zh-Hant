---
title: 無用戶端 API 實施 - 錯誤代碼/消息，可能的原因/原因
description: 無用戶端 API 實施 - 錯誤代碼/消息，可能的原因/原因
exl-id: 616e35fc-9b72-422b-9a05-e6248bd52490
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# 無用戶端 API 實施 - 錯誤代碼/消息，可能的原因/原因 {#clientless-api-implementation--error-codes-messages-with-probable-reason-cause}

>[!NOTE]
>
>本頁面上的內容僅供參考。 使用此 API 需要 Adobe Systems 的最新許可證。 不允許未經授權的使用。

</br>


## 錯誤： 未授權

### 原因：

1. POST遺失授權標頭
1. 授權標頭問題 - 檢查時間是否請求毫秒為單位。

## 錯誤：驗證期間使用 SC 400

### 原因：

1. 伺服器找不到為特定要求者和環境創建的註冊代碼。
1. 您可能會遇到跨網域 指令碼 的問題
1. 適當的欺騙應該添加到 /etc/hosts 檔案

## 錯誤： 400 請求無效

### 原因：

1. POST/GET URL 格式錯誤
1. SAMLAssertionParserException – 加密的 SAML 斷言無法在Adobe Systems端解密

## 錯誤： 403 禁止

### 原因：

1. 快速請求過多 - API 管理的一項功能，用於防止 DoS 攻擊。
2. 如果使用 prequal 環境則添加欺騙，否則確保已從 /etc/hosts 檔案中刪除欺騙

## 錯誤： 無法登入 MVPD 頁面

### 原因：

1. 使用者名稱和密碼不符
2. 登入可能已被停用
3. 檢查登入是用於生產或測試


<!--

## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)

-->
