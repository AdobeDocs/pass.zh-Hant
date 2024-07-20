---
title: 程式設計師概觀
description: 程式設計師概觀
exl-id: 64a12e49-0ecb-4b81-977d-60c10925bb59
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '4274'
ht-degree: 0%

---

# 程式設計師概觀 {#programmers-overview}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 簡介 {#introduction}

此概覽適用於計畫將Adobe®傳遞整合至其網站或應用程式的內容提供者（程式設計人員）。 如需其他檔案，包括Kickstart和整合指南，請參閱下方的「相關資訊」。

現在，您的檢視者可以隨時隨地上網，並直接向程式設計師請求存取受保護的內容。 他們可能想要觀看單次事件，或可能想要取得您正在播放的整個電視影集的觀看權。

但在您允許存取受保護的內容之前，您必須確定客戶是否有權檢視該內容。 他們是否訂閱了多頻道視訊製作經銷商(MVPD)？ 如果是，該訂閱是否包含您的程式設計？

對於程式設計師而言，決定檢視者權益並不總是簡單的。 MVPD保有客戶的識別資料和存取許可權。  此外，觀眾嘗試存取您的受保護內容時，也訂閱了各種MVPD （每個都有不同的系統），很容易看出決定觀眾是否有權使用受保護內容會迅速變得複雜且具有技術挑戰性：

![](assets/user-ent-by-progr.png)

*圖：程式設計師直接決定的使用者軟體權利檔案*

Adobe Pass Authentication for TV Everywhere安全地協調程式設計師和MVPD之間的這些權益交易。 Adobe Pass驗證讓程式設計師可以輕鬆、快速、安全地為有效客戶提供受保護的內容：

![](assets/user-ent-mediatedby-authn.png)

*圖：Adobe Pass驗證所中介的使用者軟體權利檔案*

Adobe Pass驗證會作為您的Proxy與參與的MVPD交換，因此您可以使用一致的跨網站介面來呈現檢視器。 Adobe Pass驗證也可讓您為檢視者提供單一登入(SSO)驗證和授權。 系統會追蹤所有參與服務的驗證和授權，讓訂閱者在自己的系統上完成首次驗證後，不需要再次登入。

* **驗證** — 向MVPD確認指定使用者是已知客戶的程式。
* **授權** — 向MVPD確認已驗證的使用者具有指定資源之有效訂閱的程式。

### Adobe Pass驗證的運作方式 {#HowItWorks}

程式設計師的內容檢視應用程式會使用Access Enabler使用者端元件或無使用者端API的RESTful網路服務（適用於智慧型電視、遊戲主機、機上盒等無法連線網路的裝置），與Adobe Pass驗證互動。 Access Enabler會在使用者的系統上執行，方便所有權益工作流程。  客戶存取您的網站並請求受保護的內容時，Access Enabler元件會從Adobe的代管網站下載。  Adobe Pass驗證伺服器會主控無使用者端解決方案中使用的RESTful Web服務。

Adobe Pass驗證會處理實際權益工作流程，同時提供您用於下列目的的原始程式：

* 設定您的身分。 (程式設計師是Adobe Pass驗證權益流程中的「請求者」。)
* 使用特定MVPD驗證使用者。  (MVPD是Adobe Pass驗證權利流程中的「身分提供者」或「IdP」。)
* 使用MVPD授權使用者使用特定資源。
* 登出使用者。

程式設計師負責執行下列動作的上層網頁或播放器應用程式：

* 實作使用者介面
* 與Access Enabler或無使用者端API網路服務互動

Adobe Pass驗證的目標是建立簡單的模組化方式，讓程式設計師和MVPD都可處理軟體權利檔案驗證。

## 瞭解Token {#understanding-tokens}

Adobe Pass驗證權利解決方案的中心，在於產生在成功完成驗證和授權工作流程時建立的特定資料片段。 這些資料片段稱為Token。 權杖的生命週期有限；到期時，需要透過重新初始化驗證和授權工作流程重新發行權杖。

如需權杖的詳細資訊，請參閱下列章節：

* [代號型別](#token-types)
* [Token儲存](#token-storage)
* [權杖安全性](#token-security)
* [權杖共用](#token-sharing)

### 代號型別 {#token-types}

在驗證和授權工作流程期間會核發三種型別的權杖。 AuthN和AuthZ權杖為「長期」，可提供使用者檢視體驗的連續性。 媒體代號是短期的代號，可支援業界最佳實務，以防止透過串流擷取進行詐騙。 程式設計師會根據與MVPD達成的協定，為每種型別的Token指定存留時間(TTL)值。 程式設計師決定最適合您的企業和客戶的TTL值。

* **AuthN權杖** （「長效性」）：在成功驗證後，Adobe Pass驗證會建立與請求裝置和全域唯一識別碼(GUID)相關聯的AuthN權杖。
   * Adobe Pass驗證會將AuthN權杖傳送至Access Enabler，後者會在使用者端系統上安全地快取該權杖。  雖然AuthN權杖已存在且未過期，但所有使用Adobe Pass驗證的應用程式皆可使用此權杖。 存取啟用程式會將AuthN權杖用於授權流程。
   * 在任何指定時刻，僅快取一個AuthN權杖。 每當核發新的AuthN權杖且舊權杖已存在時，Adobe Pass驗證就會覆寫快取的權杖。
* **AuthZ Token** （「長壽命」）：在成功授權後，Adobe Pass驗證會建立與請求裝置和特定受保護資源相關聯的AuthZ Token。  受保護的資源由唯一的資源ID識別。
   * Adobe Pass驗證會將AuthZ權杖傳送至Access Enabler，由其在本機系統上安全地快取。 存取啟用程式接著會使用AuthZ權杖建立用於實際檢視存取的短期媒體權杖。
   * 在任何指定時間，每個資源僅快取一個AuthZ權杖。 Adobe Pass驗證可以快取多個AuthZ權杖，前提是這些權杖與不同資源相關聯。 每當核發新的AuthZ權杖且相同資源的舊權杖已存在時，Adobe Pass驗證就會覆寫快取的權杖。
* **媒體權杖** （「短期」）： Access Enabler會使用AuthZ權杖產生短期（預設值： 7分鐘）媒體權杖。 這是系統認為已發生成功播放要求的時間點。
   * 在提供受保護資源的存取權之前，您的媒體伺服器必須使用Adobe Pass驗證元件（媒體權杖驗證器）來驗證媒體權杖。
   * 由於媒體權杖未繫結至裝置，因此其存留期明顯比長效的AuthN和AuthZ權杖短（預設值： 7分鐘）。
   * 短期媒體權杖僅限於一次性使用，永遠不會快取。 每次呼叫授權API時，都會從Adobe Pass驗證伺服器擷取授權API。

### Token儲存 {#token-storage}

Access Enabler會將長效權杖（AuthN和AuthZ）儲存在其環境的特定位置：

* **Flash10.1** （或更高版本）：長效權杖會儲存為本機共用物件。
* **HTML5**：長效權杖安全地儲存在HTML5瀏覽器的本機存放區中。
* **iOS**：長效的權杖儲存在永久性的剪貼簿上，其他Adobe Pass驗證使用者端應用程式可在此存取權杖。
* **Android**：長效權杖儲存在共用資料庫檔案中，其他Adobe Pass驗證使用者端應用程式可在此存取權杖。
* **無使用者端API裝置**：權杖儲存在Adobe Pass驗證伺服器上。

### 權杖安全性 {#token-security}

Adobe Pass驗證伺服器會使用裝置ID （衍生自裝置的硬體特性），以數位方式簽署所有長效權杖。 數位簽章的產生、保護和驗證方式因環境而異：

* **Flash10.1** （或更高） — 裝置識別碼依賴裝置認證，這是由Adobe個人化伺服器所核發的唯一憑證。 此安全性相當於FAXS DRM技術。 此伺服器端驗證會將權杖中的唯一裝置ID與裝置認證進行比較(安全地從Flash Player傳達Adobe Pass驗證)。 裝置認證也會識別FAXS使用者端版本和發行該認證的Flash Player(或AIR)版本。 裝置繫結比HTML5更強，因此Token的存留時間(TTL)通常比Flash長。
* **HTML5** — 裝置在使用者端已個人化。 它會使用JavaScript提供的特徵來產生虛擬裝置ID，其中包含瀏覽器和作業系統版本、IP位址和瀏覽器Cookie GUID （全域唯一識別碼）。 系統會比較此權杖裝置ID與裝置目前的虛擬裝置ID。 由於IP位址在正常使用期間可能會變更，即使在相同工作階段中，Adobe Pass驗證也會將HTML5權杖儲存在兩個位置：localStorage和sessionStorage。 如果IP變更且sessionStorage權杖在其他方面仍然有效，則會維護工作階段。 使用HTML5時，裝置繫結沒有那麼強，因此權杖的TTL通常比Flash的短。
* **原生使用者端** (iOS和Android) — 長效權杖會儲存原生裝置ID個人化資訊，因此會繫結至提出請求的裝置。 驗證和授權請求會透過HTTPS傳送，而裝置ID資訊會先由Access Enabler程式庫進行數位簽署，再傳送至後端伺服器。 在伺服器端，裝置ID資訊會根據其相關聯的數位簽名進行驗證。
* **無使用者端API使用者端** — 無使用者端API解決方案有一組安全性通訊協定，會以數位方式簽署所有API呼叫。 權益流程期間產生的權杖會安全地儲存在Adobe Pass驗證伺服器上。

Adobe Pass驗證會驗證每個長效權杖，以確儲存取內容的裝置與發行權杖的裝置相同。 對於所有Token，使用者端驗證會確保數位簽名完整，並保留Token的完整性。 當裝置ID驗證失敗時，驗證工作階段會失效，並提示使用者再次登入，以重設權杖。

### 權杖共用 {#token-sharing}

不同平台上的應用程式不共用代號。 原因有很多，包括：

* 如[Token Storage](#token-storage)中所述，儲存Token的方法在不同平台之間有所不同(例如，Flash的本機共用物件、JavaScript的WebStorage)。
* 不同平台的Token安全性程度不同。 例如，Flash權杖會使用FAXS強烈繫結至裝置。 純JavaScript環境中的權杖沒有Flash中相同等級的DRM支援。  與Flash應用程式共用JS權杖時，可能會增加安全性較差的權杖在利用安全性較高的環境時的可能性。

## 程式設計師整合生命週期 {#prog-integ-lifecycle}

![](assets/progr-flow-int-lifecycle.png)

*圖：整合驗證與程式設計師的網站和應用程式*


## 邏輯流程 {#logical-flows}

### 權益流程圖 {#chart}

下列流程圖顯示確認軟體權利檔案的整體程式(使用Adobe Pass Authentication Access Enabler使用者端元件)：

![](assets/ent-flowchart.png)

*圖：確認軟體權利檔案的程式*

### 驗證步驟 {#authn-steps}

下列步驟提供Adobe Pass驗證流程的範例。  這是軟體權利檔案程式的一部分，程式設計師會在此程式中判斷使用者是否為MVPD的有效客戶。  在此案例中，使用者是MVPD的有效訂閱者。  使用者正嘗試使用程式設計師的Flash應用程式檢視受保護的內容：

1. 使用者瀏覽至程式設計師的網頁，該網頁會將程式設計師的Flash應用程式和Adobe Pass驗證存取啟用程式元件載入到使用者的電腦上。 Flash應用程式會使用Access Enabler設定程式設計師的識別碼並進行Adobe Pass驗證，而Adobe Pass驗證會為該程式設計師（「請求者」）設定存取啟用碼的設定和狀態資料。 執行任何其他API呼叫之前，存取啟用程式必須從伺服器接收此資料。 技術備註：程式設計師使用Access Enabler的`setRequestor()`方法設定其身分；如需詳細資訊，請參閱[程式設計師整合指南](/help/authentication/programmer-integration-guide-overview.md)。
1. 當使用者嘗試檢視程式設計師的受保護內容時，程式設計師的應用程式會向使用者顯示MVPD清單，使用者可從中選擇提供者。
1. 系統會將使用者重新導向至Adobe Pass驗證伺服器，其中針對使用者選取的MVPD建立加密的[SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language)要求。 此要求會以代表程式設計師的驗證要求形式傳送給MVPD。 根據MVPD的系統，使用者的瀏覽器會重新導向至MVPD的網站以登入，或在程式設計師的應用程式中建立登入iFrame。
1. 在任何一種情況下（重新導向或iFrame），MVPD都會接受要求並顯示其登入頁面。
1. 使用者使用MVPD登入，MVPD會驗證使用者作為付費客戶的狀態，然後MVPD會建立自己的HTTP工作階段。
1. 驗證使用者後，MVPD會建立回應（SAML和加密），MVPD會將其傳回Adobe Pass驗證。
1. Adobe Pass驗證會收到MVPD回應、看到Adobe Pass驗證HTTP工作階段已開啟、驗證MVPD的SAML回應，並重新導向回程式設計師的網站。
1. 程式設計師的網站已重新載入，存取啟用程式已重新載入，程式設計師再次呼叫setRequestor()。  需要對setRequestor()進行第二次呼叫，因為目前的設定已變更 — 現在有旗標顯示，通知Access Enabler伺服器上正在等候產生AuthN權杖。
1. 存取啟用程式發現擱置驗證，並向Adobe Pass驗證伺服器要求Token。 Token是透過叫用Flash Player的DRM功能從伺服器擷取。
1. AuthN權杖儲存在程式設計師的Flash PlayerLSO快取中；驗證現已完成，且工作階段已在Adobe Pass驗證伺服器上損毀。

### 授權步驟 {#authz-steps}

從[驗證步驟](#authn-steps)繼續下列步驟：

1. 當使用者嘗試存取程式設計師受保護的內容時，程式設計師的應用程式會先檢查使用者本機電腦或裝置上的AuthN權杖。  如果該Token不存在，則執行上述[驗證步驟](#authn-steps)。  如果有AuthN權杖，授權流程會由程式設計師的應用程式進行，起始對存取啟用程式的呼叫，並要求取得使用者對受保護內容特定專案的檢視許可權。
1. 受保護內容的特定專案由「資源識別碼」表示。  這可以是簡單的字串或較複雜的結構，但在任何情況下，資源識別碼的性質都是預先在程式設計師和MVPD之間商定。  程式設計師的應用程式會將資源識別碼傳遞給Access Enabler。  存取啟用程式會檢查使用者本機電腦或裝置上的AuthZ權杖。  如果沒有AuthZ權杖，存取啟用程式會將要求傳遞至後端Adobe Pass驗證伺服器。
1. Adobe Pass驗證伺服器使用標準化通訊協定與MVPDs授權端點通訊。  如果MVPD的回應指出使用者有權檢視受保護的內容，則Adobe Pass驗證伺服器會建立AuthZ權杖並將其傳回存取啟用程式，以將AuthZ權杖儲存在使用者的電腦上。
1. 將AuthZ權杖儲存在使用者的電腦或裝置上，程式設計師的應用程式會呼叫存取啟用程式，以從Adobe Pass驗證伺服器取得媒體權杖，並將該權杖提供給程式設計師的應用程式。
1. 最後，程式設計師的應用程式會使用媒體權杖驗證器元件，以確認適當的使用者正在檢視適當的內容，並且在媒體權杖到位後，使用者就可以檢視受保護的內容。


## 註冊與初始化 {#reg-and-init}

使用Adobe Pass驗證的第一步是向Adobe或Adobe Pass驗證授權的合作夥伴註冊。

註冊時，需提供與Adobe Pass驗證通訊的網域清單。 例如，Turner Broadcasting System網域包含tbs.com和tnt.tv，作為Adobe Pass驗證註冊的網域。 這些內容網站都可存取Adobe Pass驗證，並獲指派自己的要求者ID （例如「TBS」和「TNT」）。 如果您決定新增其他網站，必須通知Adobe其他網域名稱，才能將其加入允許網域清單，並取得其他請求者ID。

>[!WARNING]
>
>測試整合時，請確定測試伺服器位於您打算用於生產環境的註冊網域上。 來自未列入白名單之網域的請求（甚至測試請求）會被忽略。 例如，如果您要求將domain.com用於生產環境，請務必在domain.com、test.domain.com和staging.domain.com下部署測試整合。
>
>在URL中包含使用者名稱和/或密碼的請求會被忽略，即使網域已列入白名單。 範例： `//username@registered-domain/`

在與Adobe Pass Authentication的Access Enabler使用者端元件進行的所有通訊中，要求者ID會唯一識別程式設計師的使用者端。 與程式設計師相關聯的所有Adobe Pass驗證靜態資料都會與此ID連線。

>[!TIP]
>
>除了您的要求者ID，當您註冊時，也會收到Access Enabler使用者端元件和媒體權杖驗證器的功能URL。

## 整合步驟 {#integration-steps}

>[!TIP]
>
>如果您正在使用Adobe的Open Source Media Framework(「OSMF」)進行媒體播放器開發，使用Adobe Pass驗證的最快方法是將OSMF外掛程式&#x200B;*（已棄用）*&#x200B;整合到您的播放器程式碼中。
>
><!--For details, see [Adobe Pass Authentication Plugin For OSMF](https://tve.helpdocsonline.com/9-2-2) in the Programmer Integration Guide.-->

1. [請求者設定](#requestor)
1. [處理驗證和授權](#authn-authz)
1. [支援單一登出](#ssl)

### 1.請求者設定 {#requestor}

#### 1a。 向Adobe註冊

您的第一步是向Adobe或Adobe Pass驗證授權合作夥伴註冊。  註冊時，系統會向您發出一或多個全域唯一識別碼(GUID)。 您簽發的每個GUID都與允許存取Adobe Pass驗證的網域相關聯。 您可以傳遞要求網域的GUID （稱為要求者ID），為您與Access Enabler互動的每個工作階段註冊您的身分。 如需詳細資訊，請參閱程式設計師整合指南中的[註冊與初始化](#reg-and-init)。

#### 1b. Initial Access Enabler整合

下一步是將Access Enabler整合至您現有的媒體播放器應用程式或網頁：

* 您可以將Flash版本`AccessEnabler.swf`內嵌在Flash型影片播放器中，或直接將其內嵌在網頁的HTML中。 您可以在ActionScript或JavaScript中與Access EnablerSWF通訊。 基礎APIActionScript，但如果您偏好使用JavaScript，完整的包裝函式庫可供納入您的頁面。
* 對於非Flash環境，您可以：
   * 使用HTML5/JavaScript版本AccessEnabler.js，並透過JavaScript API與其通訊
   * 使用適用於Adobe Pass驗證的AIR原生擴充功能，將原生程式碼與內建ActionScript類別結合
   * 使用其中一個原生使用者端版本的Access Enabler資料庫(iOS或Android)

### 2.處理驗證與授權 {#authn-authz}

#### 2a。 與存取啟用程式通訊

Access Enabler與您的網頁或播放器應用程式之間的通訊為非同步。 您的應用程式會呼叫Access Enabler方法，而Access Enabler會透過您向Access Enabler程式庫註冊之回呼回應。

* 如果您的應用程式提出授權要求，但本機系統上尚未具備有效的AuthN權杖，Access Enabler會自動起始驗證要求。 驗證成功時，使用者的AuthN權杖會儲存在本機，因此他們不需要再次登入。 如果他們已透過Adobe Pass驗證成功驗證任何其他內容（例如，透過MVPD網站或使用其他程式設計人員），則Access Enabler可存取本機AuthN權杖，且不需要其他驗證。
* 當使用者嘗試存取您的受保護內容時，您向存取啟用碼傳送授權請求。 驗證（或起始）驗證後，Access Enabler會聯絡MVPD (透過Adobe Pass驗證伺服器)以判斷客戶是否有權檢視受保護的內容。 您的應用程式只需要將要求傳送到Access Enabler，然後處理回應（授權成功或失敗）。 如果授權成功，就會在使用者端系統上儲存AuthZ權杖。  最後，您的應用程式會收到一個短期的媒體Token，以用於您自己的授權程式。

>[!NOTE]
>
>* 驗證會以SAML交換的形式進行，在作為服務提供者(SP)的Adobe Pass驗證和作為身分提供者(IdP)的MVPD之間進行。
>
>* 授權會使用Adobe Pass驗證(SP)和MVPD (IdP)之間的後端通道（伺服器對伺服器） Web服務交換。


#### 2b. 提供軟體權利檔案使用者介面 {#entitlement-ui}

您提供自己的UI，讓使用者存取您的內容。 有些元素（例如實際登入程式）是由MVPD提供，有些元素可選擇性作為Adobe Pass驗證的一部分使用。 您至少要執行下列動作：

* **實作MVPD選取介面，讓新使用者識別其MVPD並首次登入**。 對於開發， Access Enabler提供基本的使用者介面，讓客戶可以選擇MVPD並起始登入程式。 對於生產，您必須實作您自己的MVPD選擇器對話方塊。 有些MVPD會重新導向至自己的網站登入，有些則要求登入頁面必須顯示在iFrame中。 您必須實作建立此iFrame的回呼，以處理使用者的MVPD在iFrame中顯示其登入頁面的情況。
* **識別受保護的內容**。 受保護的內容需要授權才能存取。 您的介面應指出哪些內容受到保護，以及哪些內容已獲得授權。  授權狀態通常會以「已解除鎖定」和「已鎖定」圖示表示。
* **顯示使用者已驗證**。 您應該將使用者的驗證狀態指示為您用於識別受保護內容的任何方法的一部分。 您可以查詢Access Enabler來判斷客戶是否已驗證。

#### 2c。 整合媒體權杖驗證器 {#int-media-token-ver}

您必須將Adobe Pass驗證媒體權杖驗證器元件整合至媒體伺服器。  因此，您現有的權杖驗證器可以在成功授權的情況下，識別從Adobe Pass驗證提供的短期媒體權杖。 媒體權杖驗證器會驗證媒體權杖，作為您向使用者提供受保護內容存取權之前的最後一個安全性步驟。 當您註冊Adobe時，會收到下載媒體權杖驗證器的位置。

### 3.支援單一登出 {#ssl}

在大多數情況下，您的媒體播放器會負責透過簡易的Access Enabler API處理使用者登出。 當您呼叫logout()時，Access Enabler會執行下列動作：

* 登出目前的使用者
* 清除登出使用者的所有驗證和授權資訊
* 從使用者的本機系統刪除所有AuthN和AuthZ權杖

如果使用者讓電腦閒置足夠長的時間，讓他們的權杖到期，使用者仍然可以返回工作階段並成功起始登出。 Adobe Pass驗證可確保刪除所有Token，並通知MVPD一併刪除其工作階段。

從未與Adobe Pass驗證整合的網站起始登出時，MVPD可以透過瀏覽器重新導向來叫用Adobe Pass驗證單一登出服務。

## 瞭解使用者ID {#user-ids}

在概念上，每個起始權利流程的使用者都與單一不重複的使用者ID相關聯。  不過，在軟體權利檔案流程的過程中，該使用者ID可能會以不同方式呈現，端視您從哪個API取得該ID而定。

Short Media權杖中的sessionGUID是UserID的安全形式，可透過sendTrackingData()呼叫取得。   在所有目前的整合中，這是使用者在不同時間和裝置間使用的永久GUID，但GUID的來源會從MVPD之SAML回應中的UserID開始。   不過，有些MVPD未來可能會改變心意，並開始傳送暫時GUID。  如果程式設計師想要確保AuthN回應中的MVPD來源UserID是永久性的，他們應在與MVPD的合約中安排此專案。

以下是Adobe Pass驗證API中表示使用者ID的不同方式：

* `sendTrackingData()` GUID屬性 — 這是MVPD UserID的Adobe雜湊版本。  它會經過雜湊處理，所以此使用者ID無法從MVPD追蹤回來源。   此ID不重複且通常為持續性，但無法與MVPD共用，以比較特定使用行為與MVPD本身的行為。   它不是以數位方式簽署，因此對於防止詐騙並非沒有欺騙性，但非常適合用於分析。  在Adobe Pass驗證於AuthN/AuthZ流程中產生的所有事件上，都會在使用者端提供此形式的使用者ID。
* 短媒體權杖的`sessionGUID`屬性 — 這與透過`sendTrackingData()`的UserID相同，不過這個屬性經過數位簽章以保護其完整性。  如此一來，此值足以應付同時使用情形的詐騙追蹤。 我們打算在使用我們的驗證器程式庫後，在伺服器端處理它，並在將視訊資料流發佈給使用者端之前，針對詐騙模式進行分析。  這些工作的執行由程式設計師決定。
* `getMetadata() userID `屬性 — 此屬性將允許Adobe將實際來源MVPD UserID公開給程式設計師。 會使用我們從程式設計師取得的憑證公開金鑰進行加密，因此使用者端不會以明確的方式看到該金鑰。 這可讓程式設計師從MVPD取得實際UserID，因此可直接與MVPD用於帳戶連結或詐騙調查。

**結論**

* MVPD使用者ID通常是從MVPD產生&#x200B;**，並在成功驗證**&#x200B;時傳遞給Adobe的永久性唯一ID，不過不保證會如此。 除了某些例外情況，所有網路通常都是一致的。
* MVPD使用者ID不包含PII，且不是帳號。 它不需要以加密形式公開，因為我們已透過所有MVPD驗證未傳送任何PII。


使用者ID的使用方式取決於使用案例：

* 如果您需要它來追蹤/分析，最實用的方式是從`sendTrackingData()`取得它。
* 若您在伺服器端需要串流發佈、詐騙或操作資料的憑證，您可以從Media Token Validator取得。
* 如果您因帳戶連結和較深的詐騙行為而需要它，請洽詢您的Adobe聯絡人以取得可用性。

<!--
>[!RELATEDINFORMATION]
>
>* **Kickstart Guides** These guides explain the initial steps to take once you have decided to begin integrating with Adobe Pass Authentication.
>* **Programmer Integration Guide** This is a lower level technical guide for Programmers, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>* [Overview For MVPDs](/help/authentication/mvpd-overview.md) This provides a similar level of conceptual information as in this Programmer overview, but is directed toward MVPDs.
-->
