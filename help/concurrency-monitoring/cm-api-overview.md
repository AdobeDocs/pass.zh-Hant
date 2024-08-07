---
title: API總覽
description: 並行監視的API總覽
exl-id: eb232926-9c68-4874-b76d-4c458d059f0d
source-git-commit: dd370b231acc08ea0544c0dedaa1bdb0683e378f
workflow-type: tm+mt
source-wordcount: '1556'
ht-degree: 0%

---

# API總覽 {#api-overview}

檢視[線上API檔案](http://docs.adobeptime.io/cm-api-v2/)以取得詳細資料。

## 用途和先決條件 {#purpose-prerequisites}

本檔案可協助應用程式開發人員在實作與並行監視的整合時，使用我們的Swagger API規格。 強烈建議讀者先瞭解服務定義的概念，再遵循本指引。 為了能瞭解這點，必須概述[產品檔案](/help/concurrency-monitoring/cm-home.md)和[Swagger API規格](http://docs.adobeptime.io/cm-api-v2/)。


## 簡介 {#api-overview-intro}

在開發過程中，Swagger公開檔案代表瞭解和測試API流程的參考指引。 這是一個絕佳的起點，以便擁有實作方法，並熟悉真實世界應用程式在不同使用者互動情境下的行為方式。

在[Zendesk](mailto:tve-support@adobe.com)中提交票證，以便在並行監視中註冊您的公司和應用程式。 Adobe會將應用程式ID指派給每個實體。 在本指南中，我們將使用具有識別碼&#x200B;**demo-app**&#x200B;和&#x200B;**demo-app-2**&#x200B;的兩個參考應用程式，它們將在租使用者Adobe之下。


## 使用案例 {#api-use-case}

使用Swagger測試流量的第一步是在頁面的右上角輸入應用程式ID，如下所示：

![](assets/setting-app-id.png)

之後，我們按下&#x200B;**瀏覽**&#x200B;以設定將在對REST API進行的所有呼叫的Authorization標頭中使用的ID。  每個API呼叫都預期應用程式ID會透過HTTP基本驗證傳入。 使用者名稱是應用程式ID，而密碼為空白。


### 第一個應用程式 {#first-app-use-cases}

ID為&#x200B;**demo-app**&#x200B;的應用程式已由Adobe團隊指派一項原則，其中一項規則將同時串流數量限製為3。 會根據在Zendesk中提交的請求，將原則指派給特定應用程式。


#### 正在擷取中繼資料 {#retrieve-metadata-use-case}

我們進行的第一個呼叫是針對中繼資料資源，以便在工作階段初始化期間取得需要作為表單資料傳遞的中繼資料屬性清單。 此中繼資料將用於評估指派給此應用程式的原則。

![](assets/retrieving-metadata.png)

按下[試用]後，對於ID為&#x200B;**demo-app**&#x200B;的應用程式，我們會取得下列結果：

![](assets/empty-metadata-call.png)

我們從回應本文欄位中看到，中繼資料屬性清單是空的。 這表示設計所需的屬性足以評估指派給此應用程式的3個串流原則。 另請參閱[標準中繼資料欄位檔案](/help/concurrency-monitoring/standard-metadata-attributes.md)。 進行此呼叫後，我們可以繼續並在「工作階段」REST資源上建立新的工作階段。


#### 工作階段初始化 {#session-initial}

工作階段初始化呼叫是在取得執行呼叫所需的所有必要資訊後，由應用程式完成的。

![](assets/session-init.png)

第一次呼叫時不需要提供任何終止程式碼，因為我們沒有任何其他作用中的資料流。 而且沒有中繼資料屬性，因為擷取中繼資料呼叫未傳回任何屬性。

**subject**&#x200B;和&#x200B;**idp**&#x200B;引數是必要引數，它們將被指定為URI路徑變數。 您可以從Adobe Pass驗證呼叫&#x200B;**mvpd**&#x200B;和&#x200B;**upstreamUserID**&#x200B;中繼資料欄位，以取得&#x200B;**主旨**&#x200B;和&#x200B;**idp**&#x200B;引數。 另請參閱中繼資料API的[總覽](https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/user-metadat/user-metadata-feature.html?lang=en#)。 在此範例中，我們會提供「12345」值作為主旨，以及「adobe」值作為idp。


![](assets/session-init-params-frstapp.png)

進行工作階段初始化呼叫。 您會收到下列回應：


![](assets/session-init-result-first-app.png)


我們需要的所有資料都包含在回應標題中。 **Location**&#x200B;標頭代表新建立的工作階段識別碼，**Date**&#x200B;和&#x200B;**Expires**&#x200B;標頭代表用來排程應用程式進行下一個活動訊號以保持工作階段運作的值。

#### 心率 {#heartbeat}

進行心率呼叫。 提供在工作階段初始化呼叫中取得的&#x200B;**工作階段ID**，以及使用的&#x200B;**主體**&#x200B;和&#x200B;**idp**&#x200B;引數。

![](assets/heartbeat.png)


如果工作階段仍然有效（尚未過期或已手動刪除），您將會收到成功的結果：

![](assets/heartbeat-succesfull-result.png)

如同第一種情況，我們將使用&#x200B;**Date**&#x200B;和&#x200B;**Expires**&#x200B;標頭來排程此特定工作階段的其他活動訊號。 如果工作階段不再有效，此呼叫將會失敗，並產生410 GONE HTTP狀態代碼。

您可以使用Swagger UI中的「保持資料流作用中」選項，在特定工作階段上執行自動心率，這可幫助您測試規則，而不必擔心及時執行工作階段心率所需的樣板。 此按鈕會與「Swagger心率」標籤中的「試用」按鈕並列。 若要針對所有建立的工作階段設定自動心率，您必須在網頁瀏覽器分頁中開啟個別Swagger UI，讓工作階段分別排程每個工作階段。

![](assets/keep-stream-alive.png)

#### 工作階段結束 {#session-termination}

根據貴公司的業務案例，例如當使用者停止觀看視訊時，可能需使用並行監視來終止特定工作階段。 這可透過在工作階段資源上進行DELETE呼叫來完成。

![](assets/delete-session.png)

對呼叫使用和作業階段心率相同的引數。 回應HTTP狀態碼為：

* 成功回應為202 ACCEPTED
* 如果工作階段已停止，則為410 GONE。

#### 取得所有執行中的串流 {#get-all-running-streams}

此端點為其所有應用程式上的特定租使用者提供所有目前執行中的工作階段。 使用呼叫的&#x200B;**主旨**&#x200B;和&#x200B;**idp**&#x200B;引數：

![](assets/get-all-running-streams-parameters.png)

當您進行呼叫時，您會收到下列回應：

![](assets/get-all-running-streams-success.png)

請注意&#x200B;**Expires**標頭。 這是第一個工作階段到期的時間，除非傳送心率。 OtherStreams的值為0，因為沒有其他資料流在其他租使用者的應用程式上針對此使用者執行。
中繼資料欄位將會填入工作階段開始時所傳送的所有中繼資料。 我們不篩選它，您會收到您傳送的所有內容。
如果您進行呼叫時沒有特定使用者的執行中工作階段，您將會收到此回應：

![](assets/get-all-running-streams-empty.png)

另請注意，在此情況下，**Expires**&#x200B;標頭不存在。

#### 破壞原則 {#breaking-policy-app-first}


為了模擬指派給應用程式的3個資料流原則中斷時的行為，我們需要3次呼叫工作階段初始化。 為了讓原則生效，呼叫必須在其中一個工作階段因缺少心率而過期之前完成。 我們將會看到這些呼叫全部成功，但如果我們進行第4次呼叫，則會失敗並出現下列錯誤：

![](assets/breaking-policy-frstapp.png)


我們會收到409 CONFLICT回應，以及裝載中的評估結果物件。 閱讀[Swagger API規格](http://docs.adobeptime.io/cm-api-v2/#evaluation-result)中評估結果的完整說明。

應用程式可使用評估結果中的資訊，在停止視訊時向使用者顯示特定訊息，並在需要時採取進一步的動作。 一個使用案例是停止其他現有的串流，以啟動新的串流。 這是使用特定衝突屬性的&#x200B;**衝突**&#x200B;欄位中的&#x200B;**terminationCode**&#x200B;值來完成的。 此值將在新工作階段初始化的呼叫中作為X-Terminate HTTP標頭提供。

![](assets/session-init-termination-code.png)

在工作階段初始化時提供一或多個終止代碼時，呼叫將會成功，並產生新的工作階段。 然後，如果我們嘗試對其中一個已從遠端停止的工作階段發出心率，我們會收到410 GONE回應，其中包含說明工作階段已從遠端終止的評估結果裝載，例如範例中的：

![](assets/remote-termination.png)

### 第二個應用程式 {#second-application}

我們將使用的另一個範例應用程式是ID為&#x200B;**demo-app-2**&#x200B;的應用程式。 此原則已指派給一項原則，其中一項規則會將通道可用的串流數量限製為最多2個。   您必須提供管道變數，才能評估此原則。

#### 正在擷取中繼資料 {#retrieving-metadata}

在頁面的右上角設定新的應用程式ID，並呼叫中繼資料資源。 您會收到下列回應：

![](assets/non-empty-metadata-secndapp.png)

這次，回應本文不再是空白清單，就像第一個應用程式的範例一樣。 Concurrency Monitoring Service現在在回應主體中指出工作階段初始化需要&#x200B;**管道**&#x200B;中繼資料，才能評估原則。

如果您不提供&#x200B;**channel**&#x200B;引數的值而進行呼叫，將會得到：

* 回應代碼 — 400個錯誤請求
* 回應內文 — 在&#x200B;**義務**&#x200B;欄位中說明工作階段初始化要求中預期要執行的動作，以便作業成功的評估結果裝載。

![](assets/metadata-request-secndapp.png)


#### 工作階段初始化 {#session-init}

為所需的中繼資料金鑰指派值，並將其設定為工作階段初始化請求中的表單引數，如下所示：

![](assets/session-init-params-secndapp.png)

現在呼叫將成功，並產生新的工作階段。


#### 破壞原則 {#breaking-policy-second-app}

為了中斷指派給此應用程式的原則中所包含的規則，我們需要使用相同的管道值進行2次呼叫。 如同第一個範例，第二個呼叫必須在第一個產生的工作階段仍然有效時完成。

![](assets/breaking-policy-secnd-app.png)

如果我們每次建立新工作階段時，都使用不同的管道中繼資料值，則所有呼叫都會成功，因為臨界值2會個別設定每個值的範圍。

就像第一個範例一樣，我們可以使用終止程式碼來遠端停止衝突的資料流，或者我們可以假設沒有心率在資料流上運作，而等待其中一個資料流過期。
