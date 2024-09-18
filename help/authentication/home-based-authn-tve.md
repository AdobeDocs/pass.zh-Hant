---
title: 適用於所有電視的家用驗證
description: 適用於所有電視的家用驗證
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: acff285f7db1bdd32d5da3e01a770d9581d3ba75
workflow-type: tm+mt
source-wordcount: '1638'
ht-degree: 0%

---

# 適用於所有電視的家用驗證

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 何謂以住家為基礎的驗證？ {#whatis-home-based-authn}

家用驗證(HBA)是TV Everywhere功能，可讓付費電視訂閱者在家時，無需輸入MVPD憑證即可線上上檢視電視內容，大幅改善使用者驗證流程的使用體驗。

由開放式驗證技術委員會(OATC)所定義的以住家為基礎的驗證：「住家自動驗證是MVPD/OVD使用住家網路特性（或可自動在住家網路裝置間存取的識別碼）來驗證哪個訂閱者帳戶與該住家網路相關聯的過程，這樣使用者在建立TVE工作階段以存取TVE受保護的內容時，就不需要手動輸入認證。」



如需有關HBA和產業標準的詳細資訊，請參閱[OATC使用案例和需求](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank}檔案以及&#x200B;**OATC HBA的使用者體驗准則**。

>[!NOTE]
>
>部分HBA流程是Premium Workflow套件的一部分。 如果您有興趣使用此功能，請聯絡您的Adobe Pass銷售代表。

## 為什麼HBA對您很重要 {#why-hba}

HBA很重要，因為它實際上為家中的檢視者移除了登入障礙，而且這些檢視者已有纜線訂閱。 此外，家用驗證可大幅提升觀眾的參與度，並為您所有位置的電視內容提供更佳的使用者體驗。

目前，幾乎一半的登入嘗試都不成功。

在前5大MVPD之一啟動HBA後，其驗證轉換率&#x200B;**會增加40%** （從45%增加到63%）

![](assets/authn-conv-pre-post.png)

此外，在下方，您可以看到與不同MVPD整合之通道的登入轉換率：已為其啟用HBA的通道和沒有HBA的通道。 配備HBA的轉換率比配備HBA的轉換率高很多。

![](assets/hba-vs-non-hba.png)

在為與此MVPD整合的大部分頻道啟用HBA六個月後，我們注意到不重複使用者增加了82% （透過此MVPD存取TV Everywhere頻道的使用者人數幾乎翻了一番）。

2w3相反地，如下列圖表所示，其他未啟用HBA的MVPD在過去6個月中，不重複使用者僅增加26%。

![](assets/unique-visitors-incr.png)

從啟用HBA前6個月和後6個月收集到的資料中，我們發現啟用HBA的管道的觀眾參與度大幅增加。 實際上，來自已啟用HBA之MVPD的使用者，觀看內容的次數一般比來自未啟用HBA之MVPD的使用者多30%。

![](assets/user-engagement-increase.png)

## Adobe Pass驗證HBA支援 {#auth-hba-support}

本節說明Adobe Pass Authentication提供的HBA支援、Adobe Pass Authentication平台在HBA流程中的行為，並提供對實作HBA有用的技術細節。

支援HBA的Adobe Pass驗證功能

* 能夠為HBA與非HBA驗證設定不同的驗證TTL （也需要MVPD支援）
* 在驗證過期時自動選取MVPD （略過MVPD選擇器）的功能。 這在HBA TTL較小的情況下特別有用。
* 如果驗證是否為HBA （也需要MVPD支援），能夠向程式設計師公開

### Adobe Pass驗證平台上的HBA使用者體驗 {#hba-user-exp}

下表提供啟用HBA和未啟用HBA時，支援平台的使用者體驗相關資訊：

| 使用者流程 — 平台型別 | swf、iOS、Android |
|---|---|
| 啟用HBA | 當使用者在家時，會自動進行驗證。 HBA AuthN權杖過期後，使用者會自動重新驗證。 |
| 不含HBA | 系統會要求使用者選取其MVPD並輸入其認證，即使他們是在家中。AuthN權杖過期後，使用者必須再次輸入其認證。 |

| 使用者流程 — 平台型別 | js， Windows （原生） |
|---|---|
| 啟用HBA | 當使用者在家時，會自動進行驗證。 HBA AuthN權杖過期後，使用者必須從選擇器重新選取其MVPD，而且將會自動驗證。 |
| 不含HBA | 系統會要求使用者選取其MVPD並輸入其認證，即使他們是在家中。 AuthN權杖過期後，使用者必須再次輸入其認證。 |

| 使用者流程 — 平台型別 | 無使用者端REST API （第二個畫面驗證） |
|---|---|
| 啟用HBA | 當使用者在家中使用無使用者端REST API應用程式時，在輸入註冊代碼並選取MVPD後，他們會在第二個熒幕裝置上自動驗證。 HBA AuthN權杖過期後，使用者會自動重新驗證（在第二個熒幕裝置上）。 |
| 不含HBA | 系統會要求使用者選取其MVPD並輸入其認證，即使他們是在家中。 AuthN權杖過期後，使用者必須再次輸入其認證。 |

### 實作HBA的技術細節 {#tech-details-hba}

#### Oauth 2.0通訊協定 {#oauth-2-protocol}

在與OAuth 2.0驗證通訊協定整合之MVPD的HBA流程中，MVPD會發出重新整理權杖，而Adobe會發出HBA驗證權杖：

* 重新整理權杖的TTL取決於MVPD的業務需求。
* HBA驗證權杖TTL **必須小於或等於**&#x200B;重新整理權杖TTL。


*OAuth 2.0通訊協定之HBA驗證流程的說明*


| 使用者動作 | 系統動作 |
|---|---|
| 使用者導覽至程式設計師的網站。 當嘗試播放視訊時，會顯示MVPD選擇器。 使用者選取其MVPD並按一下登入。 | 會執行背景檢查。 MVPD會套用其規則集來偵測使用者(例如，將使用者的IP位址對應到散發者布建的資料機或寬頻連線的機上盒的MAC位址)。 |
| 畫面會持續顯示約3秒， 可以顯示插入式頁面，通知使用者正在使用其MVPD帳戶自動登入。 | <ol><li>安裝在程式設計師端的AccessEnabler會將驗證請求（作為HTTP請求）傳送到Adobe Pass驗證端點。</li><li>Adobe Pass驗證端點會將要求重新導向至MVPD驗證端點。 <br />**注意：**&#x200B;要求包含`hba_flag`引數（嘗試HBA = true），表示MVPD應該嘗試HBA驗證。</li><li>MVPD驗證端點傳送授權代碼到Adobe Pass驗證端點。</li><li>Adobe Pass驗證會使用授權代碼，從MVPD的權杖端點要求重新整理權杖和存取權杖。</li><li>MVPD傳送驗證決定以及`id_token`中的`hba_status` (true/false)引數。</li><li>已傳送對MVPD使用者設定檔端點的呼叫，以公開使用者中繼資料](/help/authentication/user-metadata-feature.md#obtaining)中的[hba_status金鑰。</li><li>MVPD會將重新整理權杖TTL設定為MVPD同意的值，而Adobe會將AuthN權杖TTL設定為小於或等於重新整理權杖的值。</li></ol> |
| 使用者已經過驗證，現在可以瀏覽已授權的TV Everywhere內容。 | 驗證Token會傳遞給使用者，讓他們現在可以成功瀏覽程式設計師的網站。 |

#### SAML協定 {#saml-protocol}

SAML驗證通訊協定的HBA驗證流程說明

| 使用者動作 | 系統動作 |
|---|---|
| 使用者導覽至程式設計師的網站。 當嘗試播放視訊時，會顯示MVPD選擇器。 使用者選取其MVPD並按一下登入。 | 會執行背景檢查。 MVPD會套用其規則集來偵測使用者(例如，將使用者的IP位址對應到散發者布建的資料機或寬頻連線的機上盒的MAC位址)。 |
| 畫面會持續顯示約3秒， 可以顯示插入式頁面，通知使用者正在使用其MVPD帳戶自動登入。 | <ol><li>安裝在程式設計師端的AccessEnabler會將驗證請求（作為HTTP請求）傳送到Adobe Pass驗證端點。</li><li>Adobe Pass驗證端點會將要求重新導向至MVPD驗證端點。</li><li>MVPD應該以SAML回應的形式傳送驗證決定，其中應包含HBA旗標： hba_status (true/false)。</li><li>已傳送對MVPD使用者設定檔端點的呼叫，以公開使用者中繼資料](/help/authentication/user-metadata-feature.md#obtaining)中的[hba_status金鑰。</li></ol> |
| 使用者已經過驗證，現在可以瀏覽已授權的TV Everywhere內容。 | 驗證Token會傳遞給使用者，讓他們現在可以成功瀏覽程式設計師的網站。 |


## 如何啟動HBA {#how-to-activate-hba}

* **OAuth通訊協定：**
   * 若要啟用HBA，請參閱[Adobe Pass TVE儀表板使用手冊](/help/authentication/tve-dashboard/old-tve-dashboard/tve-dashboard-user-guide.md)
* **SAML通訊協定：**在MVPD端啟用家用驗證。 程式設計師或Adobe不需要採取任何動作。
如需支援家用驗證之MVPD的詳細資訊，請參閱MVPD的[HBA狀態](/help/authentication/hba-status-mvpds.md)。

## 常見問題集 {#faqs}


**問題：**&#x200B;為何使用SAML和OAuth2通訊協定的家用驗證之間要分開？

**回答：**&#x200B;這兩種通訊協定的HBA流程不同。 從程式設計師的角度來看，沒有必要採取任何動作來確保為SAML MVPD啟用HBA，但若為OAuth2 MVPD，則可以在Adobe Pass TVE儀表板中開啟或關閉HBA。



**問題：**&#x200B;使用者第一次啟用HBA驗證時，是否需要填寫使用者名稱和密碼？

**回答：**&#x200B;否，不需要使用者名稱和密碼。



**問題：**&#x200B;如何強制執行家長監護？

**答案1：** Adobe可以停用HBA來與需要家長監護核准的管道整合。

**回答2：** Adobe正在與OATC合作使用UX檔案，該檔案建議如何使用家長監護設定HBA體驗。



**問題：**&#x200B;支援HBA的提供者的HBA TTL視窗是否比一般驗證時短？

**回答：** TTL設定是可設定的。 建議您為HBA驗證權杖設定較短的TTL，以防止處理錯誤。


## 有用的資訊 {#useful-info}

* [立即存取(HBA) Recommendations](http://www.ctamtve.com/instantaccess){target=_blank} — 由CTAM提供
* [程式設計師應用程式上HBA的實作範例](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/HBA_Flow_Sample.pdf?dc=201604222139-1346){target=_blank} — 依Adobe
  <!--* [Home Based Authentication User Experience Guidelines for TV Everywhere](http://oatc.us/Standards/DownloadRecommendedPractices.aspx){target=_blank} - by OATC-->
* [OATC的家用驗證使用案例與需求](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf){target=_blank}
* [以住家為基礎的驗證資訊圖形](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640){target=_blank} — 按Adobe
* [使用OAuth 2.0通訊協定進行驗證](/help/authentication/authn-oauth2-protocol.md)
* [使用SAML MVPD進行驗證](/help/authentication/authn-usecase.md)
* [Adobe Pass TVE控制面板使用手冊](/help/authentication/tve-dashboard/old-tve-dashboard/tve-dashboard-user-guide.md)
* [hba_status使用者中繼資料](/help/authentication/user-metadata-feature.md#obtaining)
