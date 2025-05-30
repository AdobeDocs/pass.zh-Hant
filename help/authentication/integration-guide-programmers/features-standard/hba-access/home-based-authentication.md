---
title: 家庭式驗證(HBA)
description: 家庭式驗證(HBA)
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: ffedb5db269644c8d9c81480d27dff43bd4eb5d6
workflow-type: tm+mt
source-wordcount: '1308'
ht-degree: 0%

---

# 家庭式驗證(HBA) {#home-based-authentication}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

家用驗證(HBA)是TV Everywhere功能，可讓付費電視訂閱者在連線至家用網路時，無需輸入MVPD憑證即可線上上存取電視內容，大幅提升驗證體驗。

根據開放式驗證技術委員會(OATC)：

> 「住家自動驗證是指讓MVPD/OVD使用住家網路的特性（或自動在住家網路裝置之間存取的識別碼），來驗證哪個訂閱者帳戶與該住家網路相關聯的過程，如此一來，使用者就不需要在啟動TVE工作階段時手動輸入認證，即可存取受保護的內容。」

如需家用驗證(HBA)以及相關產業標準的詳細資訊，請參閱下列資源：

>[!MORELIKETHIS]
>
> * 由CTAM [即時存取(HBA)](http://www.ctamtve.com/instantaccess)
> * [OATC的家用驗證使用案例與需求](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf)
> * 按Adobe[&#128279;](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640)的家用驗證資訊圖
> * [驗證已啟用OAuth2的MVPD](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
> * [啟用SAML的MVPD驗證](/help/authentication/integration-guide-mvpds/authn-usecase.md)
> * [使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

## HBA優點 {#hba-benefits}

家用驗證(HBA)是一項關鍵功能，可移除使用中纜線訂閱的家庭觀眾登入障礙。 對於所有的電視服務來說，這個障礙是一個巨大的挑戰，幾乎一半的登入嘗試都會導致失敗。

HBA可大幅改善觀眾參與度，提供順暢而卓越的使用者體驗，讓您隨時隨地存取電視內容。

## HBA支援 {#hba-support}

>[!IMPORTANT]
>
> 如果您有興趣利用HBA功能，請洽詢您的Adobe Pass驗證銷售代表，以取得詳細資訊，因為某些HBA流程已包含在進階工作流程套件中。

許多與Adobe Pass驗證整合的MVPD都支援HBA，但若要從HBA中獲益，您可能需要執行一些其他步驟。

**SAML MVPD**

對於SAML MVPD，HBA僅在MVPD端啟動。

**OAuth2 MVPD**

若為OAuth2 MVPD，您可以依照[TVE儀表板整合使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)中的步驟，透過[Adobe Pass TVE儀表板](https://experience.adobe.com/#/pass/authentication)來開啟或關閉HBA。

### MVPDs {#hba-support-mvpds}

**SAML MVPD**

下表提供支援HBA且已啟用SAML的MVPD的概觀：

| MVPD | 有基本功能嗎？ | 是否在驗證回應時傳送標幟？ |
|--------------|--------------------------------|----------------------------------------|
| DirecTV | 是 | 否 |
| 碟子 | 是 | 否 |
| 頻譜 | 是 | 是 |
| Cox | 是 | 否 |
| AT&amp;T | 是 | 否 |
| Verizon | 是 | 是 |
| Cablevision | 是 | 否 |
| Mediacom | 是 | 否 |
| 中大陸 | 是 | 否 |
| 馬西隆 | 是 | 否 |
| AlticeOne | 是 | 是 |

**OAuth2 MVPD**

下表概略說明支援HBA且已啟用OAuth2的MVPD：

| MVPD | 有基本功能嗎？ | 是否在驗證回應時傳送標幟？ |
|---------|--------------------------------|----------------------------------------|
| Comcast | 是 | 是 |

### Adobe Pass 驗證 {#hba-support-adobe-pass-authentication}

本節概述啟用HBA的體驗，並詳述Adobe Pass驗證所提供的支援，其中重點說明關鍵功能，例如：

* **HBA識別碼：**&#x200B;向程式設計師指出驗證是否為HBA或非HBA的功能(需要MVPD支援)。
* **可設定的驗證TTL：**&#x200B;可設定HBA與非HBA驗證的不同驗證存留時間(TTL)值(需要MVPD支援)。

下表提供HBA的使用者體驗與一般（非HBA）驗證流程的整體概觀：

| 驗證型別 | 使用者體驗 |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HBA | 使用者選取其MVPD，並在連線至家庭網路時自動驗證。 驗證過期後，使用者需要重新選取其MVPD，之後他們會自動重新驗證。 |
| 一般（非HBA） | 使用者選取他們的MVPD，系統會提示您輸入其認證，無論他們連線到家庭網路為何。 驗證過期後，使用者必須重新選取其MVPD並重新輸入其認證。 |

**SAML MVPD**

下表提供啟用SAML之MVPD時的HBA實施概觀：

| 使用者動作 | 系統動作 |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 使用者嘗試播放視訊。<br/><br/>顯示MVPD選取器。<br/><br/>使用者選取他們的MVPD並繼續登入。 | 背景檢查是在MVPD套用其規則以識別使用者時執行的。 例如，這可能需要將使用者的IP位址對應到散發者布建的資料機或寬頻連線的機上盒的MAC位址。 |
| 畫面會顯示持續約3秒的畫面。<br/><br/>插入式頁面可能會通知使用者，他們正使用其MVPD帳戶自動登入。 | 應用程式會在使用者代理程式中開啟驗證URL，起始對Adobe Pass驗證端點的HTTP請求。<br/><br/>Adobe Pass驗證端點透過使用者代理程式重新導向，將要求轉送至MVPD的驗證端點。<br/><br/>MVPD應該以SAML回應的形式傳送驗證決定，其中包含HBA旗標(`hba_status`)，其值為&quot;true&quot;或&quot;false&quot;。<br/><br/>Adobe Pass驗證後端向MVPD使用者設定檔端點提出要求，以公開`hba_status`標幟作為[使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)的一部分。 |
| 使用者已經過驗證，現在可以瀏覽已授權的TV Everywhere內容。 | 應用程式會擷取使用者設定檔，然後進一步進行決定流程以播放內容。 |

**OAuth2 MVPD**

下表提供啟用OAuth2之MVPD時的HBA實施概觀：

| 使用者動作 | 系統動作 |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 使用者嘗試播放視訊。<br/><br/>顯示MVPD選取器。<br/><br/>使用者選取他們的MVPD並繼續登入。 | 背景檢查是在MVPD套用其規則以識別使用者時執行的。 例如，這可能需要將使用者的IP位址對應到散發者布建的資料機或寬頻連線的機上盒的MAC位址。 |
| 畫面會顯示持續約3秒的畫面。<br/><br/>插入式頁面可能會通知使用者，他們正使用其MVPD帳戶自動登入。 | 應用程式會在使用者代理程式中開啟驗證URL，起始對Adobe Pass驗證端點的HTTP請求。<br/><br/>Adobe Pass驗證端點透過使用者代理程式重新導向，將要求轉送至MVPD的驗證端點。<br/><br/>MVPD驗證端點傳送授權代碼至Adobe Pass驗證端點。<br/><br/>Adobe Pass驗證會使用授權代碼，從MVPD的權杖端點要求重新整理權杖和存取權杖。<br/><br/>MVPD應傳送驗證決定，其中包含HBA旗標(`hba_status`)，其值為&quot;true&quot;或&quot;false&quot;，作為`id_token`的一部分。<br/><br/>Adobe Pass驗證後端向MVPD使用者設定檔端點提出要求，以公開`hba_status`標幟作為[使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)的一部分。<br/><br/>MVPD會將重新整理權杖TTL設定為MVPD同意的值，而Adobe會將驗證TTL設定為小於或等於重新整理權杖的值。 |
| 使用者已經過驗證，現在可以瀏覽已授權的TV Everywhere內容。 | 應用程式會擷取使用者設定檔，然後進一步進行決定流程以播放內容。 |

>[!IMPORTANT]
>
> 在使用OAuth 2.0驗證通訊協定的MVPD之HBA流程中，MVPD會發行重新整理權杖，其TTL由其業務需求定義，而Adobe會發行HBA驗證權杖，其TTL不得超出重新整理權杖的TTL。

## 常見問答 {#faqs}

1. 為何SAML和OAuth2通訊協定的HBA實作有所差異？

   SAML和OAuth2通訊協定的Home-Based Authentication (HBA)之間存在分離，因為這些通訊協定在驗證機制、設定和實施彈性方面的運作方式不同。 對於SAML MVPD，程式設計師不需要採取任何動作來啟用HBA，而對於OAuth2 MVPD，可以透過[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)開啟或關閉HBA。

1. 啟用HBA後，使用者在初次驗證期間是否仍需要輸入其使用者名稱和密碼？

   否，不需要使用者名稱和密碼。

1. 如何強制執行家長監護？

   Adobe Pass驗證可以停用HBA來與需要家長監護核准的通道整合。 此外，我們與OATC合作撰寫一份UX檔案，其中會建議如何使用家長監護設定HBA體驗。

1. 與一般驗證相比，支援HBA的提供者對HBA使用較短的TTL視窗嗎？

   TTL設定是可設定的。 建議您為MVPD與HBA的整合設定較短的TTL，以免處理錯誤。
