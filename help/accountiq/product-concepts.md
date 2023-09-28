---
title: 帳戶IQ字彙表
description: 產品術語表。
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '1383'
ht-degree: 0%

---

# 產品概念和字彙表 {#glossary}

請參閱下列產品術語及其定義。

## [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

儀表板面板中的圖表，將目前區段共用分數分為「極低」、「低」、「中」、「高」和「極高」共用範圍類別。

## [!UICONTROL Action] {#action-def}

與「 」相關聯的直接或間接事件 [作業](#operation-def) 這會影響相關作業區段（或同類群組）的特性（例如共用分數或使用中的裝置數）。

## [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

控制面板面板上有圖表，可將目前區段共用分數分為「極低」、「低」、「中等」、「高」和「極高」共用範圍類別，以及每個類別佔區段串流總量的百分比。

## [!UICONTROL AuthN] {#authn-def}

驗證，或驗證嘗試的次數。 驗證嘗試是指將目前沒有有效驗證狀態的使用者重新導向至他們選擇的MVPD的程式，在那裡，使用者會向MVPD識別自己 — 通常使用使用者名稱和密碼。

## [!UICONTROL AuthN OK] {#authn-ok-def}

成功的驗證數目。 當MVPD確認使用者身分識別，並導致將使用者重新導向回程式設計人員應用程式或網站時，就會發生成功的驗證。

## [!UICONTROL AuthZ] {#authz-def}

授權或授權要求的數目。 授權要求是程式設計師透過Adobe向MVPD要求許可權，以開始串流使用者要求內容的程式。 MVPD通常會根據與使用者的MVPD訂閱相關聯的內容許可權（例如與內容相關聯的頻道是否在使用者的訂閱中）來授與要求。 Adobe會快取部分授權要求回應，讓Adobe能夠立即回應，而不會將要求傳遞至MVPD。

## [!UICONTROL AuthZ OK] {#authz-ok-def}

成功的授權數目。

## [!UICONTROL Channel] {#channel-def}

色版（也稱為「屬性」）是主題相關的視訊內容來源。 傳統上，表示MVPD提供的不同、數值可定址的連續視訊摘要。 此頻道直接對應到訂閱者透過其機上盒(STB)可用的可存取內容頻道。

## [!UICONTROL Cluster] {#cluster-def}

叢集是位置和裝置的集合。 叢集是透過尋找裝置之間的共同位置來建立。 在共同位置看到的裝置會視為屬於相同叢集。 兩個裝置可以在同一個叢集中，即使它們沒有共同的位置，但可以透過其他裝置的位置連線。

### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

沒有靜態裝置的叢集。

### [!UICONTROL Static cluster] {#static-cluster-def}

至少有一個靜態裝置的叢集。

## [!UICONTROL Concurrency] {#consurrency-def}

同時播放是由兩個（或更多）同時播放或時間非常接近的串流所定義，因此以正常速度行進時，無法對齊它們之間的間隔。
同時使用率是使用2個不同叢集之間的最大速度（英里/小時）計算。 如果使用者在小於124哩距離上的速度超過124米/小時，或如果速度超過400米/小時距離超過124哩，則被視為同時使用。 會計算來自不同叢集的位置之間的距離。 相同叢集中允許同時使用。

## [!UICONTROL Device] {#device-def}

一種數位視訊硬體產品，可播放各處的電視內容，並受Adobe Pass支援。 例如，智慧型手機、筆記型電腦和桌上型電腦、遊戲主機，以及智慧型電視。

## [!UICONTROL Geographical Span] {#geographical-span-def}

一組位置中最遠點之間的距離。

## [!UICONTROL Granularity] {#granularity-def}

參照時間範圍，是指期間的大小，例如一週或一個月。

## [!UICONTROL Industry Average Index] {#industry-avg-index-def}

在選取的時間範圍內，針對所有「程式設計人員」和MVPD的每個「風險指數」（帳戶、使用狀況、整體）計算的值。

## [!UICONTROL IP] {#ip-def}

網際網路服務提供者指派給裝置的網際網路通訊協定位址。 例如，有線服務提供者，以及儲存格服務提供者。

## [!UICONTROL Isolation Mode] {#isolation-mode-def}

一種共用分析型別，帳戶評估僅限於在所選區段中直接發生在程式設計師的事件。  通常會對所有帳戶事件進行評估，而這會提供更準確的共用預估值。  某些MVPD資料的結構方式只允許隔離模式分析。

## [!UICONTROL Location] {#location-def}

地球上唯一的一點。 亦稱為特定播放要求的地理位置，使用Pass資料偵測，精確度為1000mx1000m （1平方公里）。

<!-- ### Home location {#home-location-def}

the precision is 0.01 ~ 2000mx2000m (4 square km) - at this moment the home location is detected using an ML algo, based on data provided by two mvpds. The probability is ~ 80%. We are not using the zip code for the majority of the users. Currently, this information is not used in assessing the sharing probability. -->

## [!UICONTROL Media Company] {#media-company-def}

Media Company是擁有一組媒體網路的公司。

## [!UICONTROL Metric] {#metric}

量度是訂閱者帳戶的屬性（例如，訂閱者的MVPD、程式設計師和串流內容頻道、使用的裝置數）。

## [!UICONTROL Mobile device] {#mobile-device-def}

具備高行動性的裝置。 例如行動電話和平板電腦。

## [!UICONTROL MVPD] {#mvpd-def}

MVPD （也稱為Distributor）是Media Company視訊內容的彙總、轉銷商和經銷商。

## 作業 {#operation-def}

Operation是建立用來追蹤特定之效果的記錄 [動作](#action-def) 在關聯的區段上。 作業的範例可是對節段所識別之科目所允許的並行資料流數量進行限制。

## [!UICONTROL Overall sharing score] {#overall-sharing-score}

這個值可協助使用者瞭解程式設計人員屬性或MVPD訂閱者共用密碼的程度，並提供採取行動的緊迫感。

<!--**Aggregated Risk Index**
Also known as Risk Index and Sharing Risk Index, it is a value that helps users understand the magnitude of password sharing on Programmer properties or by MVPD subscribers and provides them a sense of urgency to act upon it.-->

<!--**Risk Index - Overall**
A value computed as an average of "Risk Index - Accounts" and the "Risk Index - Usage". Overall Sharing Risk Index-->

## [!UICONTROL Play Request] {#play-requests-def}

使用者端應用程式或網站向Adobe提出請求，要求媒體權杖來記錄和保護資料流開始。

## [!UICONTROL Programmer] {#programmer-def}

Programmer （也稱為Network）是擁有和管理一個或多個管道的大型公司（公司）的子公司。

## [!UICONTROL requestorID] {#requestorid-def}

媒體公司用來識別自己或MVPD子公司的ID。  根據程式設計人員實施，這可以對應到媒體公司、程式設計人員或頻道。  媒體公司用來識別自己或MVPD子公司的常見ID。  根據程式設計人員實施，這可以對應到媒體公司、程式設計人員或頻道。  傳統上，這會對應至管道。  隨著虛擬頻道的建立(如MML (March Madness Live))和技術驅動的移動，以解決MVPD驅動的資料限制，requestorID開始與媒體公司建立更多關聯。

## [!UICONTROL resourceID] {#resource-id-def}

一般使用者要求的內容。  傳統上，這已識別與使用者請求的內容相關聯的頻道。  系統增強功能可讓ID代表特定節目（例如具有特定分級），而ID會繼續識別關聯的頻道。

## [!UICONTROL Risk Index - Usage] {#risk-index-usage}

此值也稱為「來自共用帳戶的使用量」，是根據每個帳戶發出的播放請求數加上每個帳戶的共用機率計算出的值。 也稱為「共用帳戶使用風險指數」。

## [!UICONTROL Segment] {#segmet-def}

區段是一組符合所選量度所指定之使用者定義條件的科目（例如「曾經觀看管道X、Y或Z的MVPD A、B、C、D或E使用者」）。

## [!UICONTROL Sharing level] {#sharing-level-def}

風險指數 — 帳戶或共用帳戶風險指數，是根據一組所選MVPD中每個帳戶計算出的平均共用機率來計算的，該組所選MVPD在所選時間範圍內從其中一個所選程式設計人員管道串流。

## [!UICONTROL Static device] {#static-device-def}

行動力極低的裝置。 例如，遊戲機、機上盒和電視機。

## [!UICONTROL Time frame] {#time-frame-def}

也稱為Period或Time Slot，它是包含使用者介面和表格中顯示的播放要求活動的時間視窗，從開始到結束。

## [!UICONTROL Top MVPDs in segment] {#top-mvpds-def}

所選區段中的前（最多10個） MVPD，以共用層次、共用帳戶的使用情況或整體共用分數來測量。

## [!UICONTROL Trend] {#trend-def}

目前和上一個時段間相關量度的百分比差異（例如播放要求總數的百分比）。

## [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

在指定期間內與程式設計人員TV Everywhere應用程式或涉及Adobe Pass的網站互動的指定期間內不重複MVPD帳戶數目。  該互動包括程式設計人員應用程式或網站上導致呼叫Adobe Pass服務的任何活動。 例如，檢查authN或authZ狀態、驗證和授權。 如果程式設計師的AdobeTempPass （即免費預覽）是區段的一部分，不重複訂閱者的總數也將包含不重複裝置的數量。

## [!UICONTROL Usage] {#usage-defs}

### [!UICONTROL Avid user] {#avid-user-def}

每月超過37個播放請求。

### [!UICONTROL Infrequent user] {#infrequent-users-def}

每月少於9個播放請求。

### [!UICONTROL Regular user] {#regular-user-def}

每月有9到37個播放請求。

## [!UICONTROL Usage Pattern] {#usage-patern-def}

套用至帳戶的有限類別標籤集之一，最能說明帳戶使用者在社交群組或行為方面的特性（例如，小型家庭、旅行者或通勤者、社交分享等）。

## [!UICONTROL Zip Code] {#zip-code-def}

美國境內地點相關的美國郵遞區號。
