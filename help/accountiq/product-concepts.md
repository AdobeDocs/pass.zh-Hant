---
title: Account IQ字彙表
description: 產品術語表。
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 0%

---

# 產品概念和字彙表 {#glossary}

## D2C和電視上的通用術語

下列產品術語及其定義是所有[版本的Account IQ](versions-aiq.md)所共有的。

### [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

控制面板面板，內含圖表，可將目前區段的共用分數劃分為極低、低、中、高和非常高的共用範圍類別。

### [!UICONTROL Action] {#action-def}

與[作業](#operation-def)相關的直接或間接事件，會影響相關作業區段的特性（例如，共用分數或使用中的裝置數目）。

### [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

控制面板面板，內含圖表，可將目前區段的共用分數分為「極低」、「低」、「中等」、「高」和「極高」共用範圍類別，以及區段串流總量的每個類別百分比。

### [!UICONTROL AuthN] {#authn-def}

驗證嘗試次數。 驗證嘗試是使用者嘗試使用D2C服務或MVPD登入的程式。 對於TV Everywhere使用者，使用者會重新導向至他們選擇的MVPD，在那裡他們向MVPD識別自己 — 通常使用使用者名稱和密碼。

### [!UICONTROL AuthN OK] {#authn-ok-def}

成功的驗證數目。 當D2C服務或MVPD確認使用者的識別碼時，就會發生成功的驗證。 針對「電視隨處」使用者，這會導致將使用者重新導向回程式設計人員應用程式或網站。

### [!UICONTROL Cluster] {#cluster-def}

叢集是位置和裝置的集合。 叢集是透過尋找裝置之間的共同位置來建立。 在共同位置看到的裝置會視為屬於相同叢集。 即使兩個裝置沒有共同的位置，它們也可以位於同一個叢集中，但可以透過其他裝置的位置進行連線。

#### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

沒有靜態裝置的叢集。

#### [!UICONTROL Static cluster] {#static-cluster-def}

至少有一個靜態裝置的叢集。

### [!UICONTROL Concurrency] {#consurrency-def}

同時播放是由兩個（或更多）同時播放或時間非常接近的串流所定義，因此以正常速度行進時，無法對齊它們之間的間隔。
同時使用率是使用2個不同叢集之間的最大速度（英里/小時）計算。 如果使用者在小於124哩距離上的速度超過124米/小時，或如果速度超過400米/小時距離超過124哩，則被視為同時使用。 會計算來自不同叢集的位置之間的距離。 相同叢集中允許同時使用。

### [!UICONTROL Device] {#device-def}

一種數位視訊硬體產品，可播放上流內容。 例如，智慧型手機、筆記型電腦和桌上型電腦、遊戲主機，以及智慧型電視。

### [!UICONTROL Evaluation period] {#evaluation-period-def}

評估期間是從與作業相關聯的動作開始到動作或其測量結束的時間。

### [!UICONTROL Geographical Span] {#geographical-span-def}

一組位置中最遠點之間的距離。

### [!UICONTROL Granularity] {#granularity-def}

參照時間間隔，期間的大小；例如一週或一個月。

### [!UICONTROL IP] {#ip-def}

網際網路服務提供者指派給裝置的網際網路通訊協定位址。 例如，有線服務提供者，以及儲存格服務提供者。

### [!UICONTROL Location] {#location-def}

地球上唯一的一點。 它也被稱為特定播放要求的地理位置，精確度為1000米x 1000米（1平方公里）。

### [!UICONTROL Media Company] {#media-company-def}

Media Company是擁有一組媒體網路的公司。

### [!UICONTROL Metric] {#metric}

量度是訂閱者帳戶的屬性（例如，訂閱者的MVPD、程式設計師和串流內容的頻道，以及他們使用的裝置數）。

### [!UICONTROL Mobile device] {#mobile-device-def}

具備高行動性的裝置。 例如行動電話和平板電腦。

### 作業 {#operation-def}

操作是建立的記錄，用於追蹤特定[動作](#action-def)對關聯區段的影響。 作業的範例可是對節段所識別之科目所允許的並行資料流數量進行限制。

### [!UICONTROL Overall sharing score] {#overall-sharing-score}

這個值可協助使用者了解密碼共用的規模，並提供採取行動的緊迫感。

### [!UICONTROL Play Request] {#play-requests-def}

等同於資料流開始。 此事件標示使用者串流內容的開始。

### [!UICONTROL Risk Index-Usage] {#risk-index-usage}

此值也稱為「來自共用帳戶的使用量」，是根據每個帳戶發出的播放請求數加上每個帳戶的共用機率計算出的值。 也稱為「共用帳戶使用風險指數」。

### [!UICONTROL Segment] {#segmet-def}

節段是一組符合所選量度所指定之使用者定義條件的科目。 例如，「地區A、B、C、D或E的使用者擁有超過三部裝置」。

### [!UICONTROL Sharing level] {#sharing-level-def}

風險指數 — 帳戶或共用帳戶風險指數，是根據目前區段中每個帳戶計算出的共用機率平均值（在選取的時間間隔內至少串流過一次）而計算出的值。

### [!UICONTROL Static device] {#static-device-def}

行動力極低的裝置。 例如，遊戲機、機上盒和電視機。

### [!UICONTROL Time interval] {#time-interval-def}

也稱為period，這是包含使用者介面和表格中顯示的播放要求活動的時間視窗，從開始到結束。

### [!UICONTROL Trend] {#trend-def}

目前和上一個時段間相關量度的百分比差異（例如播放要求總數的百分比）。

### [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

在指定期間內已串流至少一次的不重複帳戶數。

### [!UICONTROL Usage] {#usage-defs}

#### [!UICONTROL Avid user] {#avid-user-def}

每月超過37個播放請求。

#### [!UICONTROL Infrequent user] {#infrequent-users-def}

每月少於9個播放請求。

#### [!UICONTROL Regular user] {#regular-user-def}

每月有9到37個播放請求。

### [!UICONTROL Usage Pattern] {#usage-patern-def}

套用至帳戶的有限類別標籤集之一，最能說明帳戶使用者在社交群組或行為方面的特性（例如，小型家庭、旅行者或通勤者、社交分享等）。

### [!UICONTROL Video category] {#video-category-def}

#### [!UICONTROL For D2C service] {#d2c-service}

視訊類別是符合視訊性質的特定標籤。 例如，來源的區域、內容型別（例如VOD或即時）、事件或特定標籤（例如團隊）。

#### [!UICONTROL For TV Everywhere] {#tv-everywhere}

視訊類別是由程式設計人員、頻道以及與串流相關聯的MVPD的組合所定義。

### [!UICONTROL Zip Code] {#zip-code-def}

美國境內地點相關的美國郵遞區號。
<!--calculated metrics-->


## TV Everywhere特定術語

### [!UICONTROL AuthZ] {#authz-def}

授權或授權要求的數目。 授權要求是程式設計師透過Adobe向MVPD要求許可權，以開始串流使用者要求內容的程式。 MVPD通常會根據與使用者的MVPD訂閱相關聯的內容許可權（例如，與內容相關聯的頻道是否在使用者的訂閱中）來授與要求。 Adobe會快取部分授權要求回應，讓Adobe能夠立即回應，而不會將要求傳遞至MVPD。

### [!UICONTROL AuthZ OK] {#authz-ok-def}

成功的授權數目。

### [!UICONTROL Channel] {#channel-def}

色版（也稱為「屬性」）是主題相關的視訊內容來源。 傳統上，表示MVPD提供的不同、數值可定址的連續視訊摘要。 此頻道直接對應到訂閱者透過其機上盒(STB)可用的可存取內容頻道。

### [!UICONTROL Industry Average Index] {#industry-avg-index-def}

在選取的時間間隔內，針對所有「程式設計人員」與MVPD的每個「風險指數」（帳戶、使用狀況、整體）計算的值。

### [!UICONTROL Isolation Mode] {#isolation-mode-def}

一種共用分析型別，帳戶評估僅限於在所選區段中直接發生在程式設計師的事件。  通常會對所有帳戶事件進行評估，而這會提供更準確的共用預估值。  某些MVPD資料的結構方式只允許隔離模式分析。

### [!UICONTROL MVPD] {#mvpd-def}

MVPD （也稱為Distributor）是Media Company視訊內容的彙總、轉售商及經銷商。

### [!UICONTROL Programmer] {#programmer-def}

Programmer （也稱為Network）是擁有和管理一個或多個管道的大型公司（公司）的子公司。

### [!UICONTROL requestorID] {#requestorid-def}

媒體公司用來識別自己或MVPD子公司的ID。  根據程式設計人員實施，這可以對應到媒體公司、程式設計人員或頻道。 傳統上，這會對應至管道。  隨著MML （三月瘋狂Live）等偽頻道的建立，以及技術驅動的移動，以解決MVPD驅動的資料限制，requestorID開始與媒體公司建立更多關聯。

### [!UICONTROL resourceID] {#resource-id-def}

一般使用者要求的內容。 傳統上，這已識別與使用者請求的內容相關聯的頻道。  系統增強功能可讓ID代表特定節目（例如具有特定分級），而ID會繼續識別關聯的頻道。


