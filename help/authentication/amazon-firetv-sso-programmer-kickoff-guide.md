---
title: Amazon fireTV SSO — 程式設計師啟動指南
description: Amazon fireTV SSO — 程式設計師啟動指南
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---

# Amazon fireTV SSO — 程式設計師啟動指南 {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>

## 簡介 {#intro}

本檔案說明在您的fireTV應用程式中整合新&#x200B;**Adobe Pass驗證的fireTV SDK**&#x200B;所需的資訊。 這個新SDK利用Amazon的fireTV平台上的OS層級整合，因此提供&#x200B;**單一登入**&#x200B;支援。 若要從單一登入中獲益，您需要花費一些心力，將應用程式從無使用者端API移轉到新的fireTV SDK。 驗證流程有一些變更，將於下文詳細說明。

## 高階架構與作業系統層級的整合 {#high}

為了在Amazon fireTV平台上的TV Everywhere應用程式之間達成單一登入，並改善此平台的整體體驗，我們決定在fireTV OS層級整合我們的核心SDK。 程式設計師必須針對Adobe提供的Stub程式庫進行編譯。 Amazon的fireTV OS中的Adobe程式庫將提供實際功能。

在Amazon提供fireTV模擬器來整合我們的作業系統層級程式庫之前，您只能使用真正的fireTV裝置進行開發。

## 優點 {#bene}

* Amazon fireTV平台上的所有Adobe支援TV Everywhere應用程式與所有整合式MVPD之間的單一登入。
* 能夠從HBA中獲益（搭配支援的MVPD）。
* 可使用最新的fireTV SDK，而不需在每次發行新SDK版本時更新應用程式。
* 所有TVE應用程式都可藉由使用共用系統程式庫而獲益，因為您不必擁有AccessEnabler程式庫的本機復本。 這麼做也能確保所有應用程式都使用相同的SDK版本。
* 單熒幕驗證 — 不需要註冊代碼和第二熒幕工作流程。

## 從無使用者端API型應用程式移轉至fireTV SDK型應用程式 {#migra1}

若要從無使用者端API移轉至fireTV SDK，您必須移除與無使用者端API相關的程式碼基底，並整合新的fireTV SDK。

相較於無使用者端API型應用程式，新的fireTV SDK將驗證移至第一個畫面，因此不再需要第二個畫面驗證。

這要求程式設計師將MVPD選擇器新增至其應用程式中，讓使用者能夠直接在fireTV裝置上選擇其電視提供者。 選取MVPD後，使用者會在fireTV裝置上顯示MVPD登入頁面。

可在[Amazon Fire TV - MVVPD登入使用者流程](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/)找到描述fireTV上一般、HBA和SSO案例的使用者流程線框。

## 從Android SDK型應用程式移轉至fireTV SDK型應用程式 {#migra2}

這個新的fireTV SDK與現有的Android SDK非常類似，在我們準備好fireTV SDK檔案之前，都可以使用我們目前用於&#x200B;**整合Android SDK** <!--http://tve.helpdocsonline.com/android-technical-overview-->的檔案。 如果您已有使用我們Android SDK的Android應用程式，在fireTV應用程式中整合fireTV SDK應該相當簡單明瞭。

與現有的Android SDK相比，在fireTV SDK中，驗證程式將更容易開發，因為管理/展示MVPD登入頁面和擷取AuthN權杖的工作將由AccessEnabler程式庫在內部執行。

## 常見問答 {#faq}

1. **SSO**&#x200B;如何運作？

   * SSO可用於採用Adobe Pass Authentication之所有程式設計人員應用程式，這些應用程式在同一Amazon fireTV裝置上使用新的fireTV SDK
   * 不支援在無使用者端REST API上實作的程式設計師應用程式與在fireTV SDK **上實作的應用程式之間的SSO**

1. fireTV SSO的MVPD涵蓋範圍為何？

   * **所有由Adobe Pass驗證整合的MVPD**&#x200B;將在fireTV SDK上獲得技術上的SSO支援。

1. 除了使用新的SDK之外，程式設計師應該注意其他&#x200B;**工作流程變更**&#x200B;有哪些呢？

   * 程式設計師需要實作fireTV平台的MVPD選擇器。

1. 驗證&#x200B;**TTLs**&#x200B;是否有任何變更？

   * 關於驗證TTL的行為沒有改變。
   * 第一個有效的驗證Token將用於執行SSO，在此情況下，將透過SSO驗證的所有其他應用程式將使用相同的TTL，直到它過期為止。 因此，從一個應用程式導覽到另一個應用程式時，第二個應用程式會共用第一個驗證之應用程式的TTL。

1. **效能降低API**&#x200B;如何運作？

   * 降級API不需要任何變更，使用者體驗將會與Android裝置上相同。

1. **TempPass**&#x200B;流程會受到哪些影響？

   * TempPass流程是單一熒幕，其行為與任何其他原生裝置相同。

1. 其他Adobe功能會如以往般運作嗎？

   * 所有Adobe Pass驗證功能都將在FireTV上運作，如同在Android裝置上一樣。
