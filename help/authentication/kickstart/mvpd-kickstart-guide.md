---
title: MVPD快速入門手冊
description: MVPD快速入門手冊
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# MVPD快速入門手冊 {#mvpd-kickstart-guide}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

本Kickstart指南適用於計畫與Adobe® Pass Authentication整合的多頻道視訊程式設計經銷商(MVPD)。

本檔案概述確保順利且有效率地開始整合程式的重要初始步驟。 此課程旨在釐清客戶的期望，並指引我們與合作夥伴協力達成成功整合。

Adobe提供一系列資源，協助您整合Adobe Pass驗證。 請參考&#x200B;**「您將提供」**&#x200B;及&#x200B;**「Adobe將會提供」**&#x200B;以下各節的提及內容。

>[!CAUTION]
>
> 每次使用者起始軟體權利檔案流程時，系統都會為其指派單一、不透明、不重複的使用者ID。 此ID源自MVPD，用於識別程式設計師應用程式內的使用者。
>
> <br/>
>
> 使用者ID不可包含任何個人識別資訊(PII)或可單獨使用或與其他詳細資料結合使用以識別、聯絡或尋找使用者的資料。

## 設定程式 {#setup-process}

設定程式包括下列步驟：

![Adobe®通過驗證整合程式](/help/authentication/assets/mvpd-int-lifecycle.png)

*Adobe®通過驗證整合程式*

### 啟動 {#kickoff}

**您將在啟動階段提供**：

* **顯示名稱**

  這是提示使用者選取付費電視提供者時，程式設計師網站或應用程式上顯示的字串。

* **標誌URL**

  這是檔案，測量112 x 33畫素，包含程式設計師網站或應用程式提示使用者選擇付費電視提供者時顯示的標誌。

* **存留時間(TTL)**

  TTL是MVPD通常在驗證或授權程式中設定的值。 不過，Adobe可以覆寫這些TTL值，並根據程式設計師和MVPD雙方所同意的內容提供不同的值。

* **認證集**

  這些憑證用於透過MVPD驗證和授權使用者，或僅驗證使用者。 一般而言，這些證明資料包含使用者名稱和密碼，必須同時提供這兩個設定檔（測試和生產）。

### 中繼資料交換(SAML) {#metadata-exchange-saml}

**Adobe將在中繼資料交換階段提供**：

* **中繼環境中繼資料**

  Adobe的SP中繼資料可從https://sp.auth-staging.adobe.com/sp/metadata擷取。

* **生產環境中繼資料**

  Adobe的SP中繼資料可從https://sp.auth.adobe.com/sp/metadata擷取。

**您將在中繼資料交換階段提供**：

* **中繼資料**

  MVPD中繼環境的中繼資料。

* **生產中繼資料**

  MVPD的生產環境中繼資料。

### 連線能力 {#connectivity}

**您將提供**&#x200B;從Adobe允許清單IP的方式，因為Adobe Pass驗證需要防火牆允許流量通過連線埠80和443，以便在驗證和授權程式期間允許存取受限制的資源。

**您將在中繼設定檔中提供**&#x200B;部署，以測試連線。

### 開發 {#development}

**Adobe將提供**&#x200B;個工程時間，以便與MVPD密切合作，確保技術整合可正確建立。 此程式牽涉到根據MVPD的特定需求量身打造自訂程式碼。

### 在中繼環境中部署 {#deployment-staging}

**Adobe將為**&#x200B;組建提供所需的程式碼更新，這些更新將首先部署在PRE-QUAL中繼環境中。 在此階段中，也會實作必要的設定變更，以整合MVPD與`TestDistributors`服務提供者以進行測試。

**您和Adobe將提供**&#x200B;品質保證(QA)時間，以確保整合在PRE-QUAL測試環境中成功測試。 在此階段之後，MVPD將會移至RELEASE中繼環境，以供使用實際程式設計師進行進一步測試。

### 在生產環境中部署 {#deployment-production}

**您將在生產設定檔中提供**&#x200B;部署，以測試連線。

**Adobe將為**&#x200B;組建提供所需的程式碼更新，這些更新將部署在PRE-QUAL生產環境中。

**您和Adobe將提供**&#x200B;品質保證(QA)時間，以確保使用生產設定檔成功測試整合。 如果現在一切正常，Adobe可以將整合移至RELEASE生產環境（「即時」），供所有使用者使用。

>[!IMPORTANT]
>
> 一旦整合在RELEASE生產環境中上線，維持最佳客戶體驗便極為重要。 為了有效解決伺服器故障的情況，MVPD必須向Adobe提供詳細的向上呈報程式檔案，以便管理此類問題。
>
> 作為回報，Adobe會確保MVPD接收最新版本的Adobe Pass驗證升級程式，以簡化問題解決方案。

## 環境存取權 {#access-environments}

**Adobe將為開發流程的不同階段提供**&#x200B;環境存取權：

* **資格預審(PRE-QUAL)**

  PRE-QUAL環境會主控下一個發行候選者，並作為新合作夥伴的初始整合平台。 在改用RELEASE環境之前，合作夥伴有時間測試他們在PRE-QUAL中的整合。

* **發行版本（發行版本）**

  RELEASE環境會託管目前（穩定）的生產組建。

有關如何使用這些環境的詳細資訊，請參閱[瞭解Adobe環境](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md)檔案。

>[!IMPORTANT]
> 
> 依照建立的變更請求流程，必須透過您的Adobe代表明確請求對這些環境的設定變更。

## 存取客戶支援 {#access-customer-support}

**Adobe將透過** Zendesk[提供](https://tve.zendesk.com/home)我們的客戶支援系統存取權。 若要存取Zendesk，您必須在https://tve.zendesk.com/home註冊並建立帳戶。

Adobe Pass驗證團隊可涵蓋我們在整合過程中可能遇到的任何問題或技術問題。 請透過[tve-support@adobe.com](mailto:tve-support@adobe.com)聯絡我們。

## 存取檔案 {#access-documentation}

**Adobe將透過** Adobe Experience League[提供](https://experienceleague.adobe.com/zh-hant/docs/pass/authentication/home)對公開檔案的存取權。

Adobe Pass驗證團隊針對[MVPD整合指南](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md)區段下的可用功能和工作流程提供完整檔案。 請參考本節下的目錄，以取得每個主題的詳細資訊連結。

## 存取測試工具 {#access-testing-tool}

**Adobe將透過** Adobe Developer[網站提供](https://developer.adobe.com/adobe-pass/)我們的API探索工具存取權。
