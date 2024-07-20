---
title: Adobe Pass Concurrency Monitoring 2.5.0發行說明
description: Adobe Pass Concurrency Monitoring 2.5.0發行說明
exl-id: da392b18-a2aa-4f51-a75f-2c5b65b2b073
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---

# Adobe Pass Concurrency Monitoring 2.5.0發行說明 {#cm-250}

此頁面說明此版本的新功能、變更和已知問題：

發行日期： 2016年4月21日

## 新功能 {#new-features}

Concurrency Monitoring API v2.0現於生產環境中提供。

V2版本可統一心率和查詢呼叫，並大幅簡化同時使用這兩個API時的實作。



### 工作階段生命週期 {#session-lifecycle}

* 用於建立、保持連線及終止工作階段的唯寫API — 亦即，不再有查詢，工作階段初始及心率呼叫時會傳回決定
* 心率間隔現在由伺服器透過在工作階段初始呼叫和保持連線呼叫上設定的Expires標題驅動。 這可讓伺服器端設定心率間隔，並在應用程式中設定一個較不硬式的值。
* 快樂路徑（使用者和應用程式的合規行為）是「鋪設」的2XX狀態代碼

### 錯誤處理 {#error-handling}

* 拒絕決定、缺少中繼資料或應用程式錯誤行為會標示為4XX回應（衝突、錯誤請求、未授權、已消失等）

* 每當有意義時，回應都會包括：

   * 相關的建議 — 失敗的詳細說明，提示使用者。

   * 義務 — 應用程式必須採取的強制動作(例如：重新整理中繼資料、從Adobe Pass登出)。

### 中繼資料 {#metadata}

* 標準而非自訂中繼資料 — 這可讓API識別是否傳送了錯誤的中繼資料或是否遺失規則所需的中繼資料。
* 現在，每個應用程式的必要屬性是由API呼叫提供，且應由使用者端快取。
附註

>[!NOTE]
>
>V1版本仍可繼續使用。 如果客戶需要在心率呼叫之外使用查詢，則可以使用V1版本。




### 錯誤修正 {#bug-fixes}

不適用

### 已知問題 {#known-issues}

不適用
