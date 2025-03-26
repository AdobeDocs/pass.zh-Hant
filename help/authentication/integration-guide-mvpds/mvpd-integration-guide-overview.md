---
title: MVPD整合指南
description: MVPD整合指南
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 07bb12f7983f39b58e1b9795fdaa1bec4f68e674
workflow-type: tm+mt
source-wordcount: '1307'
ht-degree: 0%

---

# MVPD整合指南 {#mvpd-integration-guide}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

本整合指南適用於計畫與Adobe® Pass Authentication整合的多頻道視訊程式設計經銷商(MVPD)。

TV Everywhere (TVE)是付費電視產業中的革新性計畫，讓訂閱者能夠透過多部裝置（無論是在家中或外出）存取他們已付費的內容。 對付費電視提供者而言，TVE提供了重要的機會，其中可強化現有客戶關係，並開啟通往新客戶關係的大門。 然而，這些機會也伴隨著挑戰。

在TVE生態系統中，**程式設計師**&#x200B;提供內容，而&#x200B;**MVPD** （多頻道視訊節目經銷商）管理驗證檢視者是否為合格訂閱者所需的客戶資料。 雖然使用單一程式設計人員協調驗證與授權可能易於管理，但若使用數十甚至數百個程式設計人員，則會帶來相當的複雜性。

這是&#x200B;**Adobe® Pass Authentication**&#x200B;簡化程式的地方。 MVPD只需要實作單一、簡化的Adobe Pass整合，即可存取整個TVE生態系統。 提供的整合架構可加快上市時間、提供安全環境以減少詐騙，並透過跨多個平台提供更多電視內容來強化客戶體驗。

## 適用於所有電視的Adobe Pass驗證 {#adobe-pass-authentication-for-tv-everywhere}

Adobe Pass Authentication以SaaS (Software as a Service)解決方案的形式運作，旨在實現快速後端（伺服器對伺服器）整合，並遵循多頻道視訊程式設計經銷商(MVPD)和程式設計人員的商業規則。

### 標準和通訊協定 {#standards-protocols}

Adobe Pass Authentication與TV Everywhere (TVE)的新興標準和通訊協定完全一致，支援整個生態系統的緊密整合與法規遵循。

* **CableLabs OLCA （線上內容存取）規格**\
  Adobe Pass驗證遵循CableLabs OLCA規格，該規格定義從線上來源傳送視訊內容給Pay TV客戶的技術需求和架構。 Adobe於2011年6月積極參與CableLabs互通性測試專案，成功通過服務供應商實作的測試程式。

Adobe Pass驗證的架構可支援多種通訊協定（例如SAML、OAuth 2.0等），這種彈性可因應不斷變化的需求，支援未來擴充功能，包括自訂通訊協定。

大部分的整合使用SAML （安全性宣告標籤語言）通訊協定，這是驗證的主要標準。 Adobe Pass驗證會成為SAML架構內的Proxy服務提供者，將SAML驗證回應當做安全權杖儲存在Adobe公用網域中。

基本上，Adobe Pass驗證不受通訊協定限制，其設計與OLCA標準高度一致。 這些標準為程式設計人員、MVPD和服務提供者建立了通用框架，確保以一致的方法實施TVE功能。

### 整合與支援 {#integration-support}

Adobe Pass驗證會與MVPD技術團隊合作，根據其特定需求量身打造整合功能，如下所示：

* **標準整合**\
  免費提供，包括檔案和基本電子郵件支援。

* **增強支援**\
  若是重大自訂或加速時間表，可能需支付支援費用。

Adobe Pass驗證可支援有效率地處理MVPD商業邏輯，如下所示：

* **獨立式商業邏輯**\
  對於在授權請求期間由MVPD完全強制執行的商業邏輯，Adobe會提供必要資料。 此資料可包含但不限於發出請求之使用者裝置的不重複裝置ID和IP位址。

* **自訂屬性支援**\
  對於需要使用者干預或Adobe特定處理的商業邏輯，Adobe可以維護每個MVPD的自訂設定。 這些原則可啟用預先定義的工作流程，這些工作流程可在軟體權利檔案工作流程中的特定時間點觸發。 如需詳細資訊，請聯絡您的Adobe代表。

## 權益流程 {#entitlement-flow}

權利流程是程式設計人員(TVE)應用程式必須完成的一系列步驟，才能將受保護的內容串流。 此流程包含數個涉及與MVPD互動的階段：

* [驗證階段](#authentication-phase)
* （選擇性）預先授權階段
* [授權階段](#authorization-phase)
* 登出階段

使用者初次造訪程式設計人員(TVE)應用程式時，權益流程會依照概述的順序進行。 不過，在後續的造訪中，應用程式可能會根據驗證狀態和適用的檢視原則，略過某些步驟。

>[!NOTE]
>
> 本檔案使用程式設計工具(TVE)應用程式，以統稱為Adobe Pass驗證支援之不同平台（瀏覽器、行動裝置、電視連線裝置等）上執行的應用程式型別。

### 驗證階段 {#authentication-phase}

下列步驟概述在SAML整合情況下的高階步驟：

1. **程式設計師的應用程式（網站）載入**\
   使用者導覽至程式設計師的應用程式（網站），其整合了Adobe Pass驗證[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)。

1. **受保護的內容要求**\
   當使用者嘗試存取受保護的內容時，程式設計師的應用程式會顯示可供使用者選取的MVPD清單。

1. **驗證要求初始化**\
   選取MVPD後，系統會將使用者重新導向至Adobe Pass驗證伺服器。 若是SAML整合，此處會產生所選MVPD的加密SAML驗證請求。 此要求會代表程式設計師傳送至MVPD。 根據MVPD的系統，使用者的瀏覽器會重新導向至MVPD的登入頁面，或內嵌於程式設計師應用程式中的登入iFrame。

1. **MVPD登入**\
   MVPD接受請求並顯示其登入介面，可透過重新導向或iFrame進行。

1. **使用者登入及驗證**\
   使用者使用其MVPD憑證登入。 MVPD會驗證使用者的訂閱狀態，並建立自己的HTTP工作階段。

1. **MVPD對Adobe Pass驗證的回應**\
   驗證完成後，MVPD會產生SAML回應（加密）並將其傳回Adobe Pass驗證。

1. **設定檔產生**\
   Adobe Pass驗證會驗證SAML回應、產生快取的使用者設定檔，並將使用者重新導向回程式設計師的應用程式（網站）。

### 授權階段 {#authorization-phase}

**高階步驟**

下列步驟概述高階步驟：

1. **資源識別碼處理**\
   受保護的內容由[資源識別碼](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)識別，可能是簡單的字串或較複雜的結構。 此識別碼由程式設計師和MVPD預先定義並同意。 程式設計師的應用程式會將資源識別碼傳送至Adobe Pass驗證[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)。

1. **MVPD授權檢查**\
   Adobe Pass驗證伺服器使用標準化的通訊協定與MVPD的授權端點通訊。

1. **MVPD對Adobe Pass驗證的回應**\
   驗證完成後，MVPD會確認使用者是否有權存取內容，並將回應傳回Adobe Pass驗證。

1. **決定和媒體Token產生**\
   Adobe Pass驗證會驗證回應、產生快取的[決定](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)，並將包含媒體權杖的決定傳回程式設計師的應用程式（網站）。

1. **內容存取驗證**\
   程式設計師的應用程式使用[媒體權杖驗證器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)來確認存取正確內容的使用者是否正確。 驗證後，該使用者將被授予檢視受保護內容的許可權。

## 瞭解權益 {#understanding-entitlements}

Adobe Pass驗證解決方案以建立許可權為中心，也就是在成功完成驗證和授權工作流程後產生的特定資料片段。 這些許可權可授予對受保護內容的存取權，但具有有限的生命週期。 權利到期後，必須透過重新起始驗證或授權流程來續約。

如需許可權的詳細資訊，請參閱下列檔案：

* **設定檔**

  在成功驗證後，Adobe Pass驗證會建立與請求應用程式、裝置和服務提供者識別碼（請求者識別碼）相關聯的已驗證設定檔（「長期」）。

* **[使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  在成功驗證後（在某些情況下，也會在授權後），Adobe Pass驗證會從MVPD接收使用者中繼資料，這會將其公開給請求的應用程式。

* **[決定](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  在成功授權後，Adobe Pass驗證會建立與請求應用程式、裝置、服務提供者識別碼（請求者識別碼）和特定受保護資源（資源識別碼）相關聯的授權決定（「長期」）。

* **[媒體代號](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  在成功授權後，Adobe Pass驗證會建立與成功播放請求相關聯的媒體代號（「短期」），並支援減少詐騙（例如串流擷取）的業界最佳實務。

設定檔和決策的存留時間(「TTL」)值是根據程式設計師與付費電視提供者之間的協定所設定，這些提供者就最適合所有相關人員的價值達成共識。
