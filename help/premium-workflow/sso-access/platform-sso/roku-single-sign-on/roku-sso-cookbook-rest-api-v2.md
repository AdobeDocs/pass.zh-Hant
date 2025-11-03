---
title: Roku SSO逐步指南(REST API V2)
description: Roku SSO逐步指南(REST API V2)
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: e4d243ebf293f3ecc38e532d77116c065a22ebd2
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Roku SSO逐步指南(REST API V2) {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

Adobe Pass Authentication REST API V2支援在RokuOS上執行之使用者端應用程式的一般使用者使用平台單一登入(SSO)。

此檔案可作為現有[REST API V2總覽](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)的延伸，提供高階檢視和說明如何使用平台識別流程實作[單一登入的檔案](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)。

## 使用平台身分流程的Roku單一登入 {#cookbook}

Adobe Pass驗證會與Roku合作，為電視訂閱者改善登入使用者體驗，並促進跨所有電視應用程式的單一登入(SSO)。

### 先決條件 {#prerequisites}

繼續使用平台身分識別流程進行Roku單一登入之前，請確定Roku SSO已啟用。 Roku SSO預設為啟用，除非程式設計師或MVPD要求停用SSO。

每個程式設計師都可以透過[Adobe Pass TVE儀表板](https://experience.adobe.com/pass/authentication)啟用或停用Roku平台上的單一登入(SSO)以進行特定整合。

### 工作流程 {#workflow}

**使用者端對伺服器**

對於使用使用者端對伺服器架構來整合REST API V2的程式設計師應用程式，Roku SSO可順暢運作，無需任何修改。

RokuOS會自動將兩個HTTP標題附加至傳送至Adobe Pass驗證端點的所有請求。

**伺服器對伺服器**

對於使用伺服器對伺服器架構來整合REST API V2的程式設計師應用程式，程式設計師必須與Roku團隊協調，設定這些標題以包含在導向其網域的所有API流程中。

若要啟用跨應用程式和跨裝置SSO，應用程式傳入時應使用Roku提供的訂閱者ID，而非裝置ID。

如需詳細資訊，請參閱下列檔案：

* [頁首 — X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [頁首 — AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

如需所需標題格式的特定詳細資訊，請聯絡您的Adobe代表。

### 常見問答 {#faqs}

* **SSO如何運作？**

  SSO將可在與同一Roku使用者相關聯的所有Roku裝置上，使用由Adobe Pass驗證提供支援的所有程式設計人員應用程式。 並非所有MVPD都允許Roku SSO。


* **驗證TTL是否有任何變更？**

  第一個有效的驗證Token將用於執行SSO，在此情況下，將透過SSO驗證的所有其他應用程式將使用相同的TTL，直到它過期為止。 因此，從一個應用程式導覽至另一個應用程式時，第二個應用程式會共用第一個驗證之應用程式的TTL。


* **其他Adobe功能是否會如以前般運作？**

  所有Adobe Pass驗證功能都會如常運作。


* **Roku平台上的SSO是否有程式設計師選擇加入/選擇退出程式？**

  這將是Adobe TVE儀表板中的設定變更。 每個程式設計師都可以啟用或停用Roku平台上的SSO以進行特定整合。


* **有哪些常見問題？**

  程式設計師應確認其目前根據Adobe的REST API進行的實施不會妨礙Roku的平台 — SSO。

  請參閱以下的可能問題清單以及應如何解決這些問題。

| 問題 | 可能的原因 | 可能的解決方案 |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| 未將Roku SSO標頭傳送至Adobe | 對Adobe Pass驗證網域進行的呼叫使用HTTP而非HTTPS | 使用HTTPS |
| 未顯示/未針對SSO權杖更新MVPD標誌 | UI依賴本機儲存 | 應用程式應在檢查驗證後更新UI （和本機儲存空間，如有需要） |
| 無AuthZ時觸發登出 | 應用程式設計 | 應用程式應更新為永不執行幕後登出 |
