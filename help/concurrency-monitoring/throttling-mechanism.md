---
title: 節流機制
description: 節流機制
source-git-commit: cbb45cae576332e2b63027992c597b834210988d
workflow-type: tm+mt
source-wordcount: '624'
ht-degree: 1%

---


# 節流機制 {#throttling-mechanism}

## 簡介 {#introduction}

Adobe身為資料處理者，必須採取適當措施，確保客戶的使用者能公平使用資源，且服務不會充斥著不必要的API請求。 為此，我們已設定節流機制。
一個「並行監視」應用程式可供多個使用者使用，而一個使用者可以有多個工作階段。 因此，服務將設定特定時間間隔內每個使用者/工作階段接受的呼叫數限制。
達到限制後，請求將標示為特定的回應狀態（HTTP 429太多請求）。 在收到「429太多請求」回應之後完成的任何後續呼叫，應至少在一分鐘的冷卻期間內完成，以確保其將獲得有效的業務回應。

## 機制概觀 {#mechanism-overview}

此機制會決定特定時間間隔內每個「並行監視」端點可接受的最大呼叫數。
一旦達到這個最大通話數量，我們的服務將會回應「429太多請求」。 429回應「過期」標頭包含下一次呼叫被視為有效或節流過期的時間戳記。 目前，節流會在第一個429回應發生一分鐘後過期。

使用節流設定的端點包括：
1. 建立新工作階段：POST/sessions/{idp}/{subject}
2. 心率呼叫：POST/sessions/{idp}/{subject}/{sessionId}
3. 終止工作階段：DELETE/sessions/{idp}/{subject}/{sessionId}

節流是在兩個層級上設定：
1. 工作階段：相同獨特 {sessionId} 引數已傳入中 `Heartbeat` 呼叫和 `Terminate a session` 呼叫。
2. 使用者：相同獨特 {subject} 引數已傳入中 `Create a new session` 呼叫。

工作階段層級節流限制設為1分鐘內200個要求。\
使用者層級節流限制設為1分鐘內200個請求。\
這些限制（工作階段層級限制和使用者層級限制）皆可設定，我們將會更新這些限制，以防止透過有效的整合方案達到這些限制。 為此，我們建議您聯絡支援團隊。

**工作階段層級節流的情況：**

| 時間 | 要求傳送至CM | 要求數目 | 從CM收到的回應 | 說明 |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 第二10 | POST/sessions/idp1/subject1/session1 | 50 | 所有來電都會收到「202已接受」 | 從限制消耗了50個呼叫 |
| 第二50 | POST/sessions/idp1/subject1/session1 | 151 | 150個來電接收「202已接受」，1個來電接收「429請求太多」 | 從限制消耗了200個呼叫，1個呼叫將收到429個回應 |
| 第二61 | DELETE/sessions/idp1/subject1/session1 | 1 | 1個呼叫收到「429請求太多」 | 在限制內沒有可用的呼叫 |
| 第二70 | DELETE/sessions/idp1/subject1/session1 | 1 | 1個通話接收「202已接受」 | 限制設為200個可用的呼叫，因為從第10秒起已過60秒 |

**使用者層級節流的情況：**

| 時間 | 要求傳送至CM | 要求數目 | 從CM收到的回應 | 說明 |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 第二10 | POST/sessions/idp1/subject1 | 50 | 50個來電接收「202已接受」 | 從限制消耗了50個呼叫 |
| 第二50 | POST/sessions/idp1/subject1 | 151 | 150個來電接收「202已接受」，1個來電接收「429請求太多」 | 從限制消耗了200個呼叫，1個呼叫將收到429個回應 |
| 第二61 | POST/sessions/idp1/subject1 | 1 | 1個呼叫收到「429請求太多」 | 在限制內沒有可用的呼叫 |
| 第二70 | POST/sessions/idp1/subject1 | 1 | 1個通話接收「202已接受」 | 限制設為200個可用的呼叫，因為從第10秒起已過60秒 |

**429回應範例：**

```
HTTP/2 429
date: Thu, 15 Feb 2024 07:54:20 GMT
content-length: 0
vary: Origin
vary: Access-Control-Request-Method
vary: Access-Control-Request-Headers
cache-control: no-store
expires: Thu, 15 Feb 2024 07:54:41 GMT
strict-transport-security: max-age=31536000 ; includeSubDomains
x-xss-protection: 1; mode=block
x-frame-options: DENY
x-content-type-options: nosniff
```

## 客戶整合建議 {#customer-integration-recommendations}

若正確實施，客戶將不會收到「429太多請求」回應。
不過，Adobe仍建議每位客戶使用上方所述的技術詳細資訊，妥善處理「429太多請求」回應。 處理回應時，應使用「Expires」標頭來決定何時傳送下一個有效請求。