---
title: 追蹤預防評估Google Chrome
description: 追蹤預防評估Google Chrome
exl-id: f3d552da-2fd7-4ac8-9f82-876625af5d47
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 0%

---

# （舊版）預防追蹤評估 — Google Chrome {#tracking-prevention-assessment-google-chrome}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀

本檔案會彙總實用資源，並評估Google Chrome作為其逐步淘汰第三方Cookie計畫的一部分而規劃的近期變更。

此評估針對在Google Chrome瀏覽器上執行且使用Adobe Pass Access Enabler JavaScript SDK v4與Adobe Pass驗證後端服務整合的TV Everywhere (TVE)應用程式而執行。

## 公用資源

請參閱下方的Google開發人員網站彙總的資源清單，以及其官方部落格，其中建議您向客戶諮詢：

* [在Chrome中逐步淘汰協力廠商Cookie的下一步](https://blog.google/products/chrome/privacy-sandbox-tracking-protection/)
* [隱私權沙箱的開發人員檔案](https://developers.google.com/privacy-sandbox)
* [準備第三方Cookie限制](https://developers.google.com/privacy-sandbox/3pcd)
* [準備第三方Cookie逐步淘汰](https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout)
* [正在準備結束第三方Cookie](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2023oct)
* [預設會為1%的Chrome使用者限制第三方Cookie](https://developers.google.com/privacy-sandbox/blog/cookie-countdown-2024jan)

## 時間軸

簡而言之，Google Chrome已開始測試[追蹤保護](https://privacysandbox.com/)，此新功能會限制影響所有第三方Cookie的跨網站追蹤。

起初，這項計畫始於2024年初，影響約1%的使用者，其（暫定）計畫從2024年第三季度開始將影響延伸至100%的使用者。

## 評估

Google發佈了一份檔案，彙總其建議的行動手冊，為第三方Cookie逐步淘汰做好準備，請前往以下連結： https://developers.google.com/privacy-sandbox/3pcd/prepare/prepare-for-phaseout。

我們遵守此行動手冊，以評估在Google Chrome瀏覽器上執行，且使用Adobe Pass Access Enabler JavaScript SDK v4與Adobe Pass Authentication後端服務整合的TV Everywhere (TVE)應用程式。

### 結論

根據我們的測試，模擬Google Chrome即將進行的更新，主要TVE業務流程&#x200B;**將繼續如預期運作**。

不過，務必瞭解Google更廣泛的策略，其中不僅包括停止使用第三方Cookie，還包括分割第三方儲存空間。

因此，Chrome使用者將會遇到單一登入(SSO)、單一登出(SLO)和被動驗證功能發生中斷的情況，而必須對其使用的每個TVE應用程式執行個別登入/登出動作（與目前在Safari上的體驗一致）。

## 呼叫自我評估

我們敦促客戶主動進行類似的評估，以便及早發現潛在問題，並熟悉修訂後的Google Chrome使用者體驗。

這項評估應包含第一方服務和第三方服務，尤其是有關Adobe Pass Access Enabler JavaScript SDK v4整合的服務。

如果您遇到與TVE業務流程相關的任何問題，包括驗證、預先授權、授權、使用者中繼資料或登出，我們邀請您透過Zendesk票證與我們的客戶服務團隊提交報告。

如需制定自我評估計畫的協助，請參閱以下章節。

### 稽核Cookie的使用

從Chrome 118開始，[DevTools問題](https://developer.chrome.com/docs/devtools/issues/)索引標籤會醒目提示可能受影響的Cookie，並出現下列訊息： `Cookie sent in cross-site context will be blocked in future Chrome versions`。

標籤為第三方使用的Cookie可由其`SameSite=None`屬性值識別。

請依照此連結閱讀更多資訊： https://developers.google.com/privacy-sandbox/3pcd/prepare/audit-cookies

### 測試中斷

若要測試中斷，請使用`--test-third-party-cookie-phaseout`命令列旗標或從Chrome 118啟動Chrome，在`chrome://flags/`中啟用`#test-third-party-cookie-phaseout`。

這將設定Google Chrome以封鎖第三方Cookie並確保未來功能作用中，以便在逐步淘汰後以最佳方式模擬狀態。

深入瞭解下列Chrome旗標的技術規格是值得的：

* `#test-third-party-cookie-phaseout`
* `#third-party-storage-partitioning`

請依照此連結閱讀更多資訊： https://developers.google.com/privacy-sandbox/3pcd/prepare/test-for-breakage

## 其他瀏覽器

### Firefox

Firefox在幾年前推出了名為`Enhanced Tracking Protection`的機制。

請參閱以下來自Firefox的實用資源：

* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-desktop
* https://support.mozilla.org/en-US/kb/enhanced-tracking-protection-firefox-android

### Safari

Safari幾年前推出了其名為`Intelligent Tracking Prevention`的機制。

請參閱以下來自Safari的一些實用資源：

* https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/
* https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/
* https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/
* https://webkit.org/blog/8142/intelligent-tracking-prevention-1-1/
* https://webkit.org/blog/7675/intelligent-tracking-prevention/

請參閱以下來自Adobe Pass的一些實用資源：

* [追蹤預防評估 — Apple Safari](tracking-prevention-assessment-apple-safari.md)
