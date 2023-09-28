---
title: iOS SDK 3.1+上的WKWebView支援
description: iOS SDK 3.1+上的WKWebView支援
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---

# iOS SDK 3.1+上的WKWebView支援 {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>

**由於Apple在iOS上淘汰UIWebView，我們已更新iOS SDK 3.1並支援WKWebView。**

## 相容性 {#compatibility}

從iOS SDK 3.1版開始，實作者現可互換使用WKWebView或UIWebView。 由於Apple已棄用UIWebView，應用程式應移轉至WKWebView，以避免未來iOS版本發生問題。

請注意，移轉僅表示使用WKWebView切換UIWebView類別，沒有針對Adobe的AccessEnabler需完成的特定工作。

## 已知問題 {#known-issues}

Adobe的AccessEnabler使用隱藏的內部UIWebView執行個體來執行»[被動驗證](/help/authentication/sso-passive-authn.md)&quot; （適用於特定MVPD）。 對於需要針對每個請求者ID進行驗證的MVPD，「被動」流程非常有用，並且此流程讓那些在多個iOS應用程式中使用相同團隊ID以模擬SSO體驗(AdobeSSO)的程式設計師受益。 此功能目前由有限數量的MVPD使用。

此功能使用UIWebView的行為，允許Adobe擷取驗證Cookie，並在「被動」流程中重新執行。 WKWebView引入更強大的安全性，可防止Adobe擷取登入時設定的Cookie，並使用WKWebView的隱藏例項重新顯示它們。 由於此安全性改善，且考慮到「被動」流程在非常特定的實作情境（多個應用程式使用相同的團隊ID）中僅對非常有限的MVPD組有利，Adobe移除了使用網頁檢視進行驗證的MVPD的「被動驗證」功能。

此功能仍適用於設定為使用SFSafariViewController的MVPD，但請注意，在此情況下，使用者可以看到「被動」驗證，因為SFSafariViewController無法以「隱藏」方式使用。
