---
title: REST API逐步指南（使用者端對伺服器）
description: Rest API逐步指南使用者端至伺服器。
exl-id: f54a1eda-47d5-4f02-b343-8cdbc99a73c0
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# （舊版） REST API逐步指南（使用者端對伺服器） {#rest-api-cookbook-client-to-server}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

## 概觀 {#overview}

本檔案逐步說明程式設計師的工程團隊，如何使用REST API服務整合「智慧型裝置」（遊戲主機、智慧型電視應用程式、機上盒等）與Adobe Pass驗證。 這種使用者端對伺服器方法使用REST API，而不是使用者端SDK，可讓不同平台有更廣泛的支援，針對這些平台，開發大量不重複SDK將不可行。 如需無使用者端解決方案運作方式的廣泛技術概覽，請參閱[無使用者端技術概覽](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)。


此方法需要兩個元件（串流應用程式和AuthN應用程式）才能完成所需的流程：串流應用程式中的啟動、註冊、授權和檢視媒體流程，以及AuthN應用程式中的驗證流程。

### 節流機制

Adobe Pass驗證REST API受[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)所控管。

## 元件 {#components}

在工作中的使用者端對伺服器解決方案中，涉及以下元件：



| 型別 | 元件 | 說明 |
| --- | --- | --- |
| 串流裝置 | 串流應用程式 | 位在使用者串流裝置上並播放已驗證視訊的程式設計師應用程式。 |
| | \[Optional\]驗證模組 | 如果串流裝置具有使用者代理程式（即網頁瀏覽器），則AuthN模組會負責在MVPD IdP上驗證使用者。 |
| \[Optional\] AuthN裝置 | AuthN應用程式 | 如果串流裝置沒有使用者代理程式（亦即Web瀏覽器），則AuthN應用程式為程式設計人員網頁應用程式，可使用網頁瀏覽器從個別使用者的裝置進行存取。 |
| Adobe基礎結構 | Adobe Pass服務 | 此服務會與MVPD IdP和AuthZ服務整合，並提供驗證和授權決策。 |
| MVPD基礎結構 | MVPD IdP | MVPD端點，提供認證型驗證服務以驗證其使用者的身分。 |
| | MVPD AuthZ服務 | MVPD端點，可根據使用者的訂閱、家長監護等提供授權決策。 |

## 流程{#flows}

### 動態使用者端註冊(DCR)

Adobe Pass使用DCR來保護程式設計人員應用程式或伺服器與Adobe Pass服務之間的使用者端通訊。 DCR流程是獨立的，在[動態使用者端註冊概觀](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)檔案中有所說明。


### 串流（智慧型裝置）應用程式流程

![](../../../../assets/smart-device-app-flow.png)

#### 啟動流程

1. 您的應用程式會啟動並載入其初始UI。

2. 取得/產生裝置識別碼。

3. 發出Check-authentication呼叫以檢視裝置是否已驗證。  例如： [`<SP_FQDN>/api/v1/checkauthn [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

4. 如果`checkauthn`呼叫成功，請從步驟2開始進行授權流程。  如果失敗，請啟動「註冊流程」。



#### 註冊流程

1. 取得註冊碼和URL，讓您的使用者使用來存取您的第二熒幕登入應用程式，並將這些呈現給使用者：

   a.傳送POST要求至Adobe註冊代碼服務，傳遞雜湊裝置ID和「註冊URL」。  例如： [`<REGGIE_FQDN>/reggie/v1/[requestorId]/regcode [device ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)

   b.向使用者呈現傳回的註冊代碼和URL。

   c.指示使用者切換到可支援網頁的裝置，導覽至URL，然後輸入註冊代碼。



#### 授權流程

1. 使用者從第二熒幕應用程式返回，並按裝置上的「繼續」按鈕。 或者，您可以實作輪詢機制來檢查驗證狀態，但Adobe Pass驗證建議使用繼續按鈕方法來取代輪詢。 <!--(For information on employing a "Continue" button versus polling the Adobe Pass Authentication backend server, see the Clientless Technical Overview: Managing 2nd-Screen Workflow Transition.)-->例如： [\&lt;SP\_FQDN\>/api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)

2. 傳送GET請求至Adobe Pass Authentication Authorization Service以啟動授權。 例如： `<SP_FQDN>/api/v1/authorize [device ID, Requestor ID, Resource ID]`

<!-- end list -->

* 如果回應指出成功：使用者具備有效的AuthN權杖，且使用者已獲授權可觀看要求的媒體（此使用者具備有效的AuthZ權杖）。

* 如果回應指出失敗：請檢查擲回的例外狀況，以判斷其型別（AuthN、AuthZ或其他專案）：

   * 如果是AuthN錯誤，請重新啟動註冊流程。

   * 如果這是AuthZ錯誤，則使用者無權觀看請求的媒體，並且應向使用者顯示某種錯誤訊息。

   * 如果發生其他錯誤（連線錯誤、網路錯誤等），則向使用者顯示適當的錯誤訊息。



#### 檢視Media流程

1. 展示媒體選擇。 使用者選擇要檢視的媒體。

2. 媒體是否受到保護？

   a.您的應用程式會檢查媒體是否受到保護。

   b.如果媒體受到保護，您的應用程式會啟動授權
(AuthZ)流量高於。

   c.如果媒體未受保護，則播放的媒體
使用者。

3. 播放媒體。


### AuthN （第2個畫面）應用程式流程

![](../../../../assets/secnd-screen-authn-flow.png)

1. 取得此使用者的MVPD清單。 例如： [`<SP_FQDN>/api/v1/config/[requestorID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)

1. 啟動驗證流程。  例如： [`<SP_FQDN>/api/v1/authenticate [requestorID, MVPD ID, Redirect URL, Domain name, Registration Code, "noflash=true"]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)

1. 檢查驗證是否成功。 例如：[`<SP_FQDN>/api/v1/checkauthn/[registration code][requestor ID]`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)

1. 將使用者傳回您的智慧裝置應用程式，以完成授權流程。

## 合作夥伴單一登入 {#partner-sso}

某些裝置提供合作夥伴單一登入(SSO)的專屬支援：

* [APPLE SSO](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)

## 平台單一登入 {#platform-sso}

某些裝置提供平台單一登入(SSO)的專屬支援：

* [AMAZON SSO](../../sso-access/amazon-sso-cookbook-rest-api-v1.md)

## REST API的TempPass和Promotification TempPass {#temppass}

對於不需要使用者輸入認證的TempPass和Promotional TempPass實作，可以直接在串流應用程式中實作驗證。

**為了使用此API，串流應用程式需要確定裝置ID的唯一性，因為這是用來識別權杖以及選用的額外資料。**


![](../../../../assets/temp-pass-promo-temppass.png)
