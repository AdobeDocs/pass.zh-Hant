---
title: 附錄B「偵錯技巧」
description: 附錄B「偵錯技巧」
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 0%

---

# 附錄B：偵錯技巧 {#appendix-b-debugging-tips}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。


## 清除暫時資料 {#clearing-temporary-data}

Adobe Pass驗證會儲存暫時資料，例如瀏覽器快取、LSO快取和Cookie。 清除臨時資料很重要，以確保您在測試時獲得乾淨的記錄。

- [清除瀏覽器快取和Cookie](#clearing-the-browser-cache-and-cookies)
- [正在清除LSO快取](#clearing-lsos-cache)


## 清除瀏覽器快取和Cookie {#clearing-the-browser-cache-and-cookies}

它是瀏覽器可信賴的，但在Firefox中：「工具」 — \>「清除最近的歷史記錄……」 — \>在「清除的時間範圍：」上選取「全部」；在「詳細資料」上選取「Cookie」和「快取」 — \>按一下「立即清除」。


## 正在清除LSO快取 {#clearing-lsos-cache}

存取[Flash Player說明](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html)。

選取```entitlement.\*``` （視測試專案而定）並按一下[刪除網站]。


## 偵錯工具 {#tools}

Adobe Pass驗證工程師會使用以下除錯工具：

- Firebug - <http://www.getfirebug.com/>
- Flashbug （適用於flash player的偵錯版本）
- Fiddler - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>


<!--
## Related Information

- [Programmer Integration Guide](/help/authentication/programmer-integration-guide-overview.md)

- [Using Charles Proxy (Tech Note)](https://tve.zendesk.com/hc/en-us/articles/204962849-Using-Charles-Proxy)
-->
