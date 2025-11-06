---
title: 關於Adobe Pass驗證
description: 關於Adobe Pass驗證
exl-id: 5edeaccb-f9fa-4395-83b4-706c518d5a03
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 0%

---

# 關於Adobe®通過驗證 {#about-adobe-pass-authentication}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 關於所有地方的電視 {#about-tv-everywhere}

現今的電視觀眾期待隨時隨地都能順暢存取付費電視內容。 隨著網際網路連線裝置的興起，受眾正消費越來越多平台上的內容，包括：

* 筆記型電腦
* 平板電腦
* 智慧型手機
* 網站
* 同盟應用程式
* 遊戲主機
* 機上盒
* 智慧型電視

TV Everywhere是一項產業計畫，可確保付費電視訂閱者能夠透過家庭內外的多部裝置存取他們已付費的內容。

雖然傳統（線性）電視仍呈現強勁趨勢，但增長速度最快的視訊觀看區段是時移內容、線上串流及替代熒幕。 這種轉變已經顛覆視訊發佈市場，使TV Everywhere成為協調&#x200B;**程式設計師、付費電視提供者和訂閱者利益的重要解決方案。**

### 隨處可見的電視目標 {#goals-tv-everywhere}

**技術目標**

* 讓Pay Tv客戶能夠順暢地存取其所有裝置和平台的訂閱內容。

**業務目標**

* 保留並強化現有的客戶關係，同時提供新的商機。
* 讓程式設計師和內容擁有者觸及更廣泛的受眾，並最大化高階內容的價值。
* 透過與檢視者的直接線上互動來擴展品牌。

### 各處電視的挑戰 {#challenges-tv-everywhere}

隨著各處電視的機遇來臨，各種重大挑戰擺在眼前，軟體權利檔案是最關鍵的。 在檢視者可以存取訂閱內容之前，系統必須驗證其權利。

主要問題包括：

* 使用者是否擁有付費電視提供者的有效訂閱？
* 他們的訂閱是否包含要求的內容？

對於程式設計師和內容擁有者來說，決定權益特別具有挑戰性，因為付費電視操作員可以控制客戶資料和存取許可權。

除了權益之外，還會出現一些技術和整合挑戰，包括：

* 開發可確保順暢存取的多裝置策略。
* 管理程式設計師和付費電視提供者之間的複雜關係。
* 防止詐騙存取並強制實施服務條款。
* 在網站和應用程式之間提供一致、方便使用的驗證體驗。
* 維持快速上市時間，以符合附屬機構協定。
* 控制多重整合的相關成本。

這些挑戰使得程式設計師與多個付費電視驗證系統之間的直接整合需要大量資源，同時需要時間與技術專業知識。

為了克服這些障礙，**Adobe® Pass Authentication**&#x200B;簡化並簡化許可權驗證，讓您可以順暢安全地存取TV Everywhere內容。

## Adobe Pass驗證簡介 {#introduction-adobe-pass-authentication}

Adobe Pass驗證安全地協調程式設計師和付費電視提供者之間的權益交易，確保適當的客戶可以輕鬆存取適當的內容。

![](../assets/programmers-connect-authn.png)

*透過Adobe Pass驗證連線的部分程式設計師和付費電視提供者*

**受益於Adobe Pass驗證的人員**

* **程式設計師**

  想要輕鬆與付費電視供應商（也稱為MVPD，多頻道視訊節目經銷商）整合，以觸及最廣泛的對象並最佳化收入。

* **付費電視提供者(MVPD)**

  希望透過單一整合與多位內容擁有者（程式設計師）建立聯絡的客戶，促進線上訂閱內容的存取，進而提升客戶滿意度。

* **付費電視客戶**

  想要隨時隨地透過任何裝置觀看已付費內容的使用者。

**程式設計師**

程式設計師若想將受眾觸及範圍和收入最大化，可以：

* 輕鬆整合頂級付費電視供應商，無需管理多重直接連線。
* 透過確保跨所有主要提供者和平台的驗證來擴大其閱聽眾。
* 使用安全驗證保護高階內容，限制對授權使用者和裝置的存取。
* 啟用單一登入(SSO)以改善跨應用程式和網站的使用者體驗。

**付費電視提供者(MVPD)**

付費電視提供者可透過以下方式提升客戶體驗並簡化營運：

* 透過單一整合與多個內容擁有者連線。
* 提供品牌化、順暢的跨裝置和平台檢視體驗。
* 確保安全驗證，以防止未經授權的存取，並管理每個家庭的同時資料流。

**付費電視客戶**

訂閱者可隨時隨地透過電視收看他們已付費的內容。

### 整合Adobe Pass驗證 {#integrating-adobe-pass-authentication}

無論您是付費電視提供者或程式設計人員，與Adobe Pass驗證整合都需要主動參與。 以下為兩個角色程式的高階概觀。

#### 程式設計師整合程式 {#programmer-integration-process}

整合正式啟動後，可使用其他指引，但程式通常涉及以下步驟：

**必要條件**

* 內容管理系統(CMS)。
* 內容傳遞機制，其中可能包括協力廠商內容傳遞網路(CDN)。
* 具有內嵌在網站或獨立應用程式中的媒體播放器的現有線上視訊平台。

**整合工作**

* 整合Adobe Pass驗證[REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)。
* 整合Adobe Pass驗證[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)。
* 整合Adobe Pass驗證[媒體權杖驗證器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)。
* 開發用於驗證、授權和登出工作流程的使用者介面。

如需程式設計師整合程式的詳細資訊，請參閱[程式設計師快速入門手冊](/help/authentication/kickstart/programmer-kickstart-guide.md)和[程式設計師整合指南](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md)檔案。

#### 付費電視提供者整合程式 {#pay-tv-provider-integration-process}

整合正式啟動後，可使用其他指引，但程式通常涉及以下步驟：

**必要條件**

* 簽署Adobe Pass驗證保密協定(NDA)。
* 向Adobe Pass驗證工程團隊提供驗證和授權系統的規格。 建議您使用SAML型身分提供者(IdP)進行驗證，並使用SOAP型授權系統來進行最簡單的整合。

**整合工作**

* 建立與Adobe Pass驗證伺服器的連線。
* 完成測試版並確保品質保證。
* 完成生產版本並確保品質保證。

付費電視提供者可免費使用標準整合，包括檔案及基本電子郵件支援。 然而，需要大量協助或加速時間表的提供者可能會產生支援費用，或選擇與經驗豐富的協力廠商合作夥伴（例如Synacor）合作。

Adobe Pass驗證可透過兩種主要方式，有效支援付費電視提供者特有的商業邏輯：

* 針對自成一體、且由付費電視提供者在收到授權請求時套用的商業邏輯，Adobe會提供必要資料（例如唯一裝置ID、IP位址）以支援強制執行。
* 對於需要使用者介入的業務邏輯或Adobe的特定處理，可以為每個付費電視提供者維護自訂屬性。 這些設定可能包括在驗證流程的特定點觸發的預定義工作流程。

如需付費電視提供者整合程式的詳細資訊，請參閱[MVPD kickstart指南](/help/authentication/kickstart/mvpd-kickstart-guide.md)和[MVPD整合指南](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md)檔案。

### 權益流程 {#entitlement-flow}

Adobe Pass Authentication作為Proxy，透過為雙方提供安全一致的介面，促進程式設計師和MVPD之間的權利流通。

對於程式設計師而言，Adobe Pass驗證提供API作為&#x200B;**Standard**&#x200B;或&#x200B;**Premium**&#x200B;層級的一部分：

* 標準Adobe Pass驗證API：
   * [REST API DCR](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* Premium Adobe Pass驗證API：
   * [重設Temp Pass API](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [TempPass功能](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [降級API](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [退化特徵](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [權益服務監控API](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

如需軟體權利檔案流程的詳細資訊，請參閱[程式設計師整合指南](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md#entitlement-flow)檔案。

#### 瞭解權益 {#understanding-entitlements}

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

#### 建立使用者介面 {#building-user-interface}

程式設計師負責為其網站或應用程式內的軟體權利檔案工作流程設計和實作使用者介面(UI)，而某些元素（例如登入程式）則由付費電視提供者處理。

程式設計師至少必須：

* **實作提供者選擇介面**
   * 允許新使用者識別他們的付費電視提供者，並首次登入。
   * 有些付費電視提供者會將使用者重新導向至外部登入頁面，有些則需要在iframe內登入。 程式設計師必須實作回呼函式，才能在需要時產生iframe。

* **管理支援的付費電視提供者清單**
   * 確保使用者只能透過核准的提供者存取內容。

* **表示驗證狀態**
   * 顯示使用者在應用程式或網站內進行驗證的時間。

* **識別受保護的資源**
   * 清楚指出哪些內容在檢視前需要授權。
   * 更新UI以反映在授予存取權後成功的授權。

## 常見問題集 {#faqs}

**哪裡都有電視？**

TV Everywhere是一項產業計畫，可讓Pay TV客戶透過各種網際網路連線裝置，存取他們已訂閱的優質內容。 這包括個人電腦、平板電腦、智慧型手機、遊戲主機、機上盒和智慧型電視。 主要難題是確保驗證程式順暢且方便使用，讓客戶能夠存取其訂閱內容，而不需要多次登入或技術障礙。

**什麼是Adobe Pass驗證？它如何支援所有的電視節目？**

Adobe Pass驗證以簡單有效率的方式安全驗證使用者的內容許可權，讓所有地方的電視畫面栩栩如生。 這是一種託管服務，可根據程式設計師和付費電視提供者設定的商業規則，促進快速後端整合。 如此一來，所有利害關係人的上市時間都會加快，環境會更安全，避免詐騙；此外，使用者體驗也會更好，可在多個平台存取更多電視內容。

**如何傳遞Adobe Pass驗證？**

Adobe Pass驗證是以軟體即服務(SaaS)解決方案的形式提供。 此方法可確保一般使用者、程式設計師和付費電視提供者之間的安全通訊，以進行內容許可權驗證。

**Adobe Pass驗證與其他隨處電視解決方案有何不同？**

Adobe Pass驗證提供數個優於替代解決方案的優勢：

* **順暢的單一登入(SSO)** — 與個別提供者直接整合不同，Adobe Pass驗證可當使用者在不同網站和應用程式之間移動時，提供永續的登入體驗。
* **廣泛的市場滲透** — 程式設計師一旦與Adobe Pass驗證整合，就可以立即存取涵蓋超過90%美國家庭的付費電視營運商。
* **與Adobe生態系統整合** — 與其他Adobe解決方案順暢合作，提供內容遞送、保護及營利，包括Adobe Analytics。

**Adobe Pass驗證的安全性如何？**

安全性是重中之重。 Adobe Pass驗證可確保只有授權的使用者才能透過繫結對使用者裝置的存取權來存取進階內容。 此外，也提供選項來限制每戶同時觀看的串流、工作階段或裝置數量。

**Adobe Pass驗證支援哪些裝置？**

Adobe Pass驗證的設計可在各種裝置上運作，例如網頁裝置、行動裝置或電視連線裝置。

**一般使用者的Adobe Pass驗證費用是多少？**

不適用。 Adobe Pass驗證可供一般使用者免費使用。
