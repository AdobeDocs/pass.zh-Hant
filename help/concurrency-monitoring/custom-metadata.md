---
title: 自訂中繼資料
description: 自訂中繼資料
exl-id: 0cfd1158-8c6c-47c2-b838-5490ff4bf0ce
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 0%

---

# 自訂中繼資料 {#cm}

>[!NOTE]
>
> 此頁面已淘汰，因為它適用於不再建議用於新整合的舊版API

此服務可讓使用者端在查詢和決策中同時使用標準和自訂欄位。 標準欄位一律可用於任何資料流（因為它們是建立資料流或由伺服器產生的必要欄位）：

* 應用計畫
* mvpd
* accountId
* streamId
* streamStart
* 發起人


自訂欄位是在資料流初始化時傳入的任何索引鍵/值組，可作為表單或查詢字串引數。 然後，標準和自訂欄位都可用於以下情況（如需詳細資訊，請參閱以下相關API資源的實際檔案）：

* 擷取帳戶的資料流清單（/streams資源）時，要求額外的欄位（透過欄位查詢字串引數）
* 指定維度作為群組依據，以劃分帳號活動（ /activity資源）
* 根據欄位值或基數定義伺服器端原則（範例使用偽SQL以清楚說明）：
* 設定僅套用至特定欄位值的原則(例如，專用的iOS原則：其中osType為&#39;iOS&#39;)
* 限制指定欄位的相異值數量(例如不多於X個相異裝置： HAVING DISTINCT COUNT(deviceId) >= 2)
* 限制每個欄位值的作用中串流數目(例如，單一裝置型別不超過X個作用中串流：GROUP BY deviceType HAVING COUNT(streamId) >= 3)


根據傳送的這些索引鍵/值，可以建立不同的規則。 此路徑大致如下：

1. 客戶決定傳送引數群組，其值為「SPORTS」和「KIDS」。
1. 應用程式將需要執行此動作：
   * 若為體育頻道，在資料流初始化時，應用程式會傳送&#x200B;***type=SPORTS***&#x200B;作為查詢引數
   * 對於具有兒童相關內容的頻道，在資料流初始化時，應用程式會傳送&#x200B;***type=KIDS***&#x200B;作為查詢引數
1. 然後可以定義類似這樣的原則：
   * `GROUP by type HAVING COUNT(streamID) < 4) IF type=KIDS`
   * `GROUP by type HAVING COUNT(streamID) < 2) IF type=SPORTS`
1. 這基本上代表當使用者觀看運動時，他/她無法在超過1部裝置上執行此操作，但當使用者觀看兒童內容時，最多允許在3部裝置上檢視。
