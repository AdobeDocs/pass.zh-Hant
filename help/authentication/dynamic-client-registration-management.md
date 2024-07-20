---
title: 動態使用者端註冊管理
description: 動態使用者端註冊管理
exl-id: 2c3ebb0b-c814-4b9e-af57-ce1403651e9e
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1321'
ht-degree: 0%

---

# 動態使用者端註冊管理 {#dynamic-client-registration-management}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

隨著客戶的應用程式廣泛採用[Android Chrome自訂標籤](https://developer.chrome.com/multidevice/android/customtabs){target_blanck}和[Apple Safari檢視控制器](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target_blanck}，我們正在更新Adobe Pass驗證中的使用者驗證流程。 更具體來說，我們無法再達成維護狀態的目標，因此驗證MVPD訂閱者的使用者代理流程可以在重新導向之間追蹤。 此工作先前是使用HTTP Cookie完成。 此限制是開始將所有API移轉至OAuth 2.0 [RFC6749](https://tools.ietf.org/html/rfc6749){target_blanck}的驅動程式。

透過此更新，Adobe驗證使用者端成為OAuth 2.0使用者端，並部署自訂OAuth 2.0授權伺服器以滿足Adobe Pass驗證服務的需求。

為了讓使用者端應用程式能夠利用OAuth 2.0授權，伺服器必須動態註冊以取得特定資訊（使用者端憑證），才能與其互動。 在註冊程式中，使用者端必須向使用者端註冊端點提供一組內建的中繼資料。

此中繼資料以軟體陳述式傳達，其中包含「software_id」，可讓我們的授權伺服器使用相同的軟體陳述式來關聯應用程式的不同執行個體。

**軟體陳述式**&#x200B;是一個JSON Web權杖(JWT)，它會以套件形式宣告有關使用者端軟體的中繼資料值。 當作為使用者端註冊請求的一部分呈報給授權伺服器時，軟體陳述式必須使用JSON Web簽名(JWS)進行數位簽署或MAC編輯。

您可以在官方檔案[RFC7591](https://tools.ietf.org/html/rfc7591)中找到軟體陳述式及其運作方式的詳細說明。

軟體陳述式應該與應用程式一起部署在使用者裝置上。

在此更新之前，我們有兩個機制可允許應用程式執行對Adobe Pass驗證的呼叫：

* 以瀏覽器為基礎的使用者端已透過允許的[網域清單](/help/authentication/programmer-overview.md#reg-and-init)登入
* 原生應用程式使用者端(例如iOS和Android應用程式)是透過&#x200B;**簽署的請求者**&#x200B;機制註冊


透過使用者端註冊授權機制，您必須將應用程式新增到TVE儀表板。

若要讓客戶開始實作新的Android SDK和即將推出的iOS SDK，他們需要軟體宣告。 軟體陳述式會識別在TVE儀表板中建立的應用程式。

請依照下列各節中的步驟，在TVE儀表板中建立「已註冊的應用程式」。

## 建立註冊的應用程式 {#create_app}

在TVE Dashboard中建立「註冊的應用程式」有兩種方式：

* [程式設計人員層級](#prog-level) — 可讓您建立已註冊的應用程式，並將其連結至任何或所有程式設計人員管道。

* [頻道層級](#channel-level) — 可讓您建立已註冊的應用程式，此應用程式僅永久連結至此頻道。

### 在程式設計人員層級建立註冊的應用程式 {#prog-level}

移至&#x200B;**程式設計師** > **已註冊的應用程式**&#x200B;標籤。

![](assets/reg-app-progr-level.png)

在[已註冊的應用程式]索引標籤中，按一下[新增應用程式]****。 在新視窗中填寫必填欄位。

如下圖所示，您應填寫的欄位包括：

* **應用程式名稱** — 應用程式的名稱

* **指派給頻道** — 您的頻道名稱，此應用程式連結到的t</span>o。 下拉式遮罩中的預設設定是&#x200B;**所有色版。**&#x200B;介面可讓您選取一個通道或所有通道。

* **應用程式版本** — 預設會設為「1.0.0」，但強烈建議您使用自己的應用程式版本加以修改。 最佳做法是，如果您決定變更應用程式的版本，請透過為其建立新的註冊應用程式來反映該版本。

* **應用程式平台** — 要連結的應用程式平台。 您可以選擇選取全部或多個值。

* **網域名稱** — 要連結的應用程式網域。 下拉式清單中的網域是來自您所有管道的所有網域的統一選擇。 您可以選擇從清單中選取多個網域。 網域的含義是重新導向URL [RFC6749](https://tools.ietf.org/html/rfc6749)。 在使用者端註冊程式中，使用者端應用程式可請求允許使用重新導向URL來結束驗證流程。 當使用者端應用程式要求特定的重新導向URL時，將會根據與軟體陳述式關聯的這個註冊應用程式中列出的網域進行驗證。


![](assets/new-reg-app.png)


在欄位中填入適當的值後，您必須按一下「完成」，應用程式才會儲存在設定中。

請注意，沒有&#x200B;**選項可修改已建立的應用程式**。 如果發現已建立的某個專案不再符合需求，則需要建立新的註冊應用程式，並搭配其符合需求的使用者端應用程式使用。


### 在通道層級註冊新的應用程式 {#channel-level}

若要在頻道層級建立註冊的應用程式，請導覽至「頻道」功能表，然後選擇要為其建立應用程式的頻道。 接著，導覽至「註冊的應用程式」索引標籤後，按一下「新增應用程式」按鈕。

![](assets/reg-new-app-channel-level.png)

如下所示，與在程式設計人員層級執行的相同動作相比，這裡稍微不同的是「已指派管道」下拉式清單，此清單未啟用，因此沒有選項可將註冊的應用程式繫結到目前管道以外的其他管道。

![](assets/new-reg-app-channel.png)

## 列出應用程式 {#list-reg-app}

建立註冊的應用程式後，可能會取得軟體陳述式，以作為要求的一部分呈現授權伺服器。

您可以導覽至已建立註冊應用程式的程式設計師或管道（在清單中列出），以完成此操作。

如下圖所示，清單中的每個專案都將由其所繫結平台的名稱、版本和符號進行識別。

![](assets/reg-app-list.png)

對於每個檔案，您可以：

* [檢視](#view)
* [下載軟體宣告](#download-statement)

### 檢視註冊的應用程式 {#view}

在應用程式清單中，選擇其中一個應用程式並按一下「檢視」按鈕，將顯示建立應用程式時使用的詳細資訊。 如前所述，沒有任何選項可修改任何內容。


![](assets/view-reg-app.png)


### 下載軟體宣告 {#download-statement}

按一下需要軟體陳述式之清單專案上的[下載]按鈕將產生文字檔。 此檔案將包含類似於以下範例輸出的內容。


![](assets/download-software-statement.png)

以「software_statement」為檔案前置詞並加入目前時間戳記，可唯一識別檔案名稱。

請注意，對於相同的註冊應用程式，每次按一下下載按鈕時都會收到不同的軟體陳述式，但這不會使先前為此應用程式取得的軟體陳述式失效。 發生此情況是因為會根據動作要求即時產生。

有關下載動作有一個&#x200B;**限制**。 如果在建立註冊的應用程式後不久按一下「下載」按鈕以請求軟體陳述式，而且此陳述式尚未儲存，且設定json未同步，則下列錯誤訊息將顯示在頁面底部。

![](assets/error-sw-statement-notready.png)

這會包裝從核心收到的「HTTP 404找不到」錯誤碼，因為已註冊應用程式的ID尚未傳播，而核心並不知道該專案。

解決方案是在建立已註冊的應用程式後，最多等待2分鐘讓設定同步。 發生此情況後，將無法再收到錯誤訊息，且包含軟體陳述式的文字檔將可供下載。

如需端對端流程運作方式的詳細資訊，或深入瞭解要求的執行方式及預期的回應，請參閱下文相關資訊中的連結，以及其他實用連結。

<!--
## Related Information {#related}

* [Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)
* [TVE Dashboard User Guide](/help/authentication/tve-dashboard-user-guide.md)
-->

## 功能示範 {#tutorial}

請觀看[此網路研討會](https://my.adobeconnect.com/pzkp8ujrigg1/)，其中提供更多功能內容，並包含如何使用TVE儀表板管理軟體陳述式以及如何使用Adobe提供的示範應用程式測試產生的陳述式的示範，該示範應用程式是Android SDK的一部分。
