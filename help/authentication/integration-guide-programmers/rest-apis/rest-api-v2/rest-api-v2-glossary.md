---
title: rest API V2字彙表
description: rest API V2字彙表
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '1742'
ht-degree: 0%

---

# rest API V2字彙表 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

本檔案提供整合Adobe Pass Authentication REST API V2時所使用的字詞定義。

>[!MORELIKETHIS]
>
> * [動態使用者端註冊(DCR)字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## 字彙表辭彙 {#glossary-terms}

### A {#a}

#### 驗證 {#authentication}

驗證是允許使用者在[MVPD](#mvpd)驗證使用者訂閱後，向[程式設計人員](#programmer)證明其身分的程式，以取得受保護內容（[資源](#resource)）的存取權。

#### 驗證代碼 {#code}

驗證代碼是Adobe Pass驗證概念，儲存使用者啟動[驗證](#authentication)程式時產生的唯一值，並在驗證程式完成之前唯一識別使用者的[驗證工作階段](#session)。

驗證碼可由[主要（程式設計人員）應用程式](#primary-application)或[次要（程式設計人員）應用程式](#secondary-application)使用，以完成[驗證](#authentication)程式、擷取[驗證工作階段](#session)的相關資訊，或存取使用者[設定檔](#profile)。

與先前使用之註冊代碼的同義字。

#### 驗證工作階段 {#session}

驗證工作階段是Adobe Pass驗證概念，可儲存使用者從[程式設計員](#programmer)應用程式啟動（或繼續）的驗證程式相關資訊，並由[驗證代碼](#code)唯一識別。

驗證工作階段也可以指示[程式設計員](#programmer)應用程式繼續進行[授權](#authorization)程式，作為[權益](#entitlement)流程的下一個階段（若使用者已經驗證）。

#### Authorization {#authorization}

授權是允許使用者在使用[MVPD](#mvpd)驗證使用者許可權後，根據擁有的[MVPD](#mvpd)訂閱，從[程式設計師](#programmer)目錄存取受保護內容（[資源](#resource)）的程式。

### C {#c}

#### 設定 {#configuration}

此設定是Adobe Pass驗證概念，可儲存有關[程式設計員](#programmer)和[MVPD](#mvpd)整合設定的資訊，並可在[驗證](#authentication)程式期間，要求使用者從使用中整合清單中選取其[電視提供者](#tv-provider)時使用。

### 第天{#d}

#### 決定 {#decision}

決定是Adobe Pass驗證概念，其儲存有關[MVPD](#mvpd) [授權](#authorization)或[預先授權](#preauthorization)處理序查詢的資訊，以允許或拒絕使用者存取[程式設計師](#programmer)受保護的內容。

#### 降級 {#degradation}

降級是Adobe Pass驗證功能，可讓使用者存取受保護的內容，即使其[MVPD](#mvpd)發生服務中斷亦然。

如需詳細資訊，請參閱[降級功能](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)檔案。

#### 裝置ID {#device-id}

裝置ID是繫結至使用者裝置的唯一識別碼，必須由[程式設計師](#programmer)應用程式在[權益](#entitlement)流程的所有階段提供。

### E {#e}

#### 權利 {#entitlement}

權利檔案是Adobe Pass驗證概念，結合可協助使用者通過不同階段的可用流程和功能，以存取受保護的內容，範圍包括[驗證](#authentication)、[預先授權](#preauthorization)、[授權](#authorization)以及最後[登出](#logout)。

#### 增強錯誤碼 {#enhanced-error-code}

增強的錯誤碼是Adobe Pass驗證概念，可提供有關處理請求時發生的錯誤的其他資訊。

如需詳細資訊，請參閱[增強錯誤碼](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)檔案。

### H {#h}

#### HBA {#hba}

家用驗證(HBA)是讓消費者在連線至其家用網路（屬於訂閱合約內的位置）的選取裝置上，自動被授與存取[TV Everywhere (TVE)](#tve)內容的許可權。

### 我{#i}

#### 身分提供者 {#identity-provider}

身分提供者是一家公司，在[TV Everywhere (TVE)](#tve)的內容中，透過有線電視、衛星或網際網路服務為消費者提供身分識別服務。

與[MVPD](#mvpd)和[電視提供者](#tv-provider)同義。

### L {#l}

#### 登出 {#logout}

登出是允許使用者在Adobe Pass Authentication中終止其已驗證的[設定檔](#profile)並更新[程式設計員](#programmer)應用程式以反映使用者狀態的程式。

### M {#m}

#### 媒體權杖 {#media-token}

媒體權杖是Adobe Pass驗證產生的權杖，這是授權[決定](#decision)的結果，旨在提供受保護內容的存取權。

媒體權杖已傳遞給[程式設計師](#programmer)，然後程式設計師會驗證它，以確保該[資源](#resource)的存取安全性。

與使用短授權Token的前一個詞同義。

#### 媒體權杖驗證器 {#media-token-verifier}

媒體權杖驗證器是由Adobe Pass驗證所分配的程式庫，負責驗證[媒體權杖](#media-token)的真實性。

如需詳細資訊，請參閱[媒體權杖驗證器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)檔案。

#### MVPD {#mvpd}

多頻道視訊節目經銷商(MVPD)是一家透過有線電視、衛星電視或網際網路為消費者提供電視服務的公司。

MVPD由MVPD和Adobe之間上線流程期間定義的唯一值識別。

與[電視提供者](#tv-provider)和[身分提供者](#identity-provider)同義字。

### P {#p}

#### 合作夥伴 {#partner}

合作夥伴是向[程式設計師](#programmer)提供服務或架構以啟用單一登入使用者體驗的公司。

透過唯一值（例如「apple」）來識別合作夥伴，該值是在合作夥伴和Adobe之間的上線流程中定義的。

#### 預先授權 {#preauthorization}

預先授權是允許使用者從[程式設計師](#programmer)目錄預覽他們有權存取的[資源](#resource)子集的程式(使用[MVPD](#mvpd)驗證使用者許可權之後)。

與[預檢](#preflight)同義字。

#### 預檢 {#preflight}

預檢是允許使用者從[程式設計人員](#programmer)目錄預覽他們有權存取的[資源](#resource)子集的程式(使用[MVPD](#mvpd)驗證使用者許可權後)。

與[預先授權](#preauthorization)同義字。

#### 主要（程式設計師）應用程式 {#primary-application}

主要應用程式參考起始[驗證](#authentication)的[程式設計師](#programmer)應用程式，但可能無法使用[使用者代理程式](#user-agent)完成程式，以瀏覽至[MVPD](#mvpd)登入頁面。

#### 個人資料 {#profile}

設定檔是Adobe Pass驗證概念，可儲存關於使用者驗證開始日期和結束日期、[使用者的中繼資料](#user-metadata)以及指出取得驗證方法的其他欄位（例如，「一般」、「已降級」、「暫時」、「單一登入」等）的相關資訊。

與前一個詞語同義的驗證權杖。

#### 程式設計師 {#programmer}

Programmer是一家透過各種平台擁有的管道（品牌）為消費者提供內容的公司。

程式設計師將多個擁有的管道（品牌）群組為[服務提供者](#service-provider)，以便與Adobe Pass驗證整合。

#### Proxy MVPD {#proxy-mvpd}

Proxy MVPD是一間為其他MVPD提供身分服務的公司，並直接與Adobe Pass驗證整合。

#### 代理的MVPD {#proxied-mvpd}

代理的MVPD公司沒有與Adobe Pass驗證直接整合，但透過[代理MVPD](#proxy-mvpd)整合。

#### 平台身分 {#platform-identity}

平台識別碼是由繫結至使用者裝置的服務或架構（程式庫）產生的唯一平台識別碼裝載，提供給[程式設計師](#programmer)以啟用單一登入使用者體驗。

如需詳細資訊，請參閱[使用平台身分識別流程的單一登入](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)檔案。

### R {#r}

#### 資源 {#resource}

資源是使用者嘗試從[程式設計師](#programmer)目錄存取的受保護內容。

資源由程式設計師和MVPD之間議定的唯一值識別。

如需詳細資訊，請參閱[受保護的資源](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources)檔案。

### S {#s}

#### SAML {#saml}

安全性判斷提示標籤語言(SAML)是一種開放標準，用於交換各方之間的驗證和授權資料，尤其是在[身分提供者](#identity-provider)和[服務提供者](#sp)之間。

#### 次要（程式設計師）應用程式 {#secondary-application}

次要應用程式參考到能夠使用[使用者代理程式](#user-agent)來瀏覽至[MVPD](#mvpd)登入頁面，以完成[驗證](#authentication)程式的[程式設計師](#programmer)應用程式。

次要應用程式可在與主要應用程式相同的裝置上執行，或在不同的（次要）裝置上執行，在此情況下，登入體驗稱為「第二個熒幕驗證」使用者體驗。

#### 服務權杖 {#service-token}

服務權杖是由繫結至使用者的服務或架構（程式庫）產生的唯一使用者識別碼，提供給[程式設計師](#programmer)以啟用單一登入使用者體驗。

如需詳細資訊，請參閱[使用服務權杖流程的單一登入](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)檔案。

#### 服務提供者 {#service-provider}

服務提供者是[程式設計師](#programmer)所擁有的管道（品牌）。

透過程式設計師和Adobe之間的上線流程中定義的唯一值來識別服務提供者。

與先前使用的要求者ID同義。

#### SLO {#slo}

單一登出(SLO)是允許使用者從屬於[單一登入(SSO)](#sso)的所有應用程式登出的程式。

#### SP {#sp}

服務提供者(SP)在與[MVPD](#mvpd)的整合中，參考Adobe Pass驗證代表[程式設計師](#programmer)所扮演的角色。

#### SSO {#sso}

單一登入(SSO)是允許使用者驗證一次並存取多個[程式設計師](#programmer)應用程式的程式，而不需要驗證每個應用程式。

### T {#t}

#### TempPass基本 {#temp-pass-basic}

基本的TempPass是Adobe Pass驗證功能，可讓使用者在有限的時間記憶體取受保護的內容，而不需要使用[MVPD](#mvpd)進行驗證。

如需詳細資訊，請參閱[Basic TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#basic-temp-pass)檔案。

#### TempPass促銷 {#temp-pass-promotional}

提升TempPass是Adobe Pass驗證功能，可讓使用者以最大資源數量及有限的時間存取受保護內容，而不需要透過[MVPD](#mvpd)驗證。

如需詳細資訊，請參閱[促銷暫時Pass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)檔案。

#### TTL {#ttl}

存留時間(TTL)是一個值，可指示基礎實體有效的時間量。

可以針對[存取權杖](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token)、[設定檔](#profile)、授權[決定](#decision)或[媒體權杖](#media-token)提及TTL。

#### TVE {#tve}

TV Everywhere (TVE)是一個產業利基市場，可讓消費者在多部裝置（例如智慧型手機、平板電腦、筆記型電腦等）上存取他們最愛的電視節目、電影和其他內容。

#### TVE控制面板 {#tve-dashboard}

TV Everywhere (TVE) Dashboard是提供給[程式設計師](#programmer)的Adobe Pass驗證工具，用來管理其設定和資料。

如需詳細資訊，請參閱[TVE儀表板使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)檔案。

#### 電視提供者 {#tv-provider}

電視提供者是一家公司，透過有線電視、衛星電視或網際網路服務，為消費者提供電視服務。

透過在電視提供者和Adobe之間的上線過程中定義的唯一值，來識別電視提供者。

與[MVPD](#mvpd)和[身分提供者](#identity-provider)同義。

### U {#u}

#### 使用者代理 {#user-agent}

使用者代理程式是指能夠導覽網頁並呈現[MVPD](#mvpd)登入頁面的瀏覽器或類似元件（平台專用）。

#### 使用者 ID {#user-id}

使用者ID是繫結至使用者的唯一識別碼，源自[MVPD](#mvpd)驗證程式。

#### 使用者中繼資料 {#user-metadata}

使用者中繼資料是指由[MVPD](#mvpd)維護且由Adobe Pass驗證提供作為[設定檔](#profile)一部分的使用者特定屬性（例如郵遞區號、家長分級、使用者ID等）。

如需詳細資訊，請參閱[使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)檔案。

### 版本{#v}

#### VSA {#vsa}

視訊訂閱者帳戶(VSA)是提供給[程式設計師](#programmer)的Apple開發架構，可啟用單一登入使用者體驗。

如需詳細資訊，請參閱[視訊訂閱者帳戶架構](https://developer.apple.com/documentation/videosubscriberaccount)和[使用合作夥伴流程的單一登入](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)檔案。
