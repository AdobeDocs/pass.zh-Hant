---
title: 節流機制
description: 瞭解Adobe Pass驗證中使用的節流機制。 在此頁面中探索此機制的概觀。
source-git-commit: 4f81f39427d87e4274c27d8f1b4bd1eb366d9abb
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 0%

---


# 節流機制 {#throttling-mechanism}

所有Pass Authentication客戶都需要能夠根據其指示和商業案例，為其每位使用者存取Pass Authentication API。

Pass Authentication引進節流機制，以確保在客戶的使用者之間公平分配資源。

此機制之所以重要，原因如下：

- 多租使用者架構的其中一項黃金原則是，一個使用者的行為不應影響其他人。
- 對於API，速率限制很重要，因為與API整合時很容易出錯。 由於API是以程式設計方式使用，因此不小心傳送過多請求相對容易。

## 機制概觀 {#mechanism-overview}

### 使用者端實作

通過驗證可提供與API互動的准則和SDK，但無法控制客戶的使用方式。 有些實作可能只進行初級實作，可能會讓服務充滿不必要的API要求，無論是否意外，可能代表其他使用者遇到速度緩慢或容量問題。

服務本身應該能夠處理任何合理的容量。 但無論服務的擴充性或效能如何，總會有限制。 因此，服務必須針對特定時間間隔內接受的呼叫數設定限制。

### 節流簡介

通過驗證是以使用者身分識別和Token貯體比率限制演演算法為基礎，使用預先定義的值來控制每個使用者裝置對我們API的存取。

### 裝置識別機制

所提出的節流機制會利用「X-Forwarded-For」標頭，個別使用已識別的裝置。 限制將以相同方式套用至每個裝置。

### 必要的更新

伺服器對伺服器實作必須使用「X-Forwarded-For」標頭機制轉送其使用者端的IP位址。

您可以找到更多有關如何傳遞X-Forwarded-For標頭的詳細資料 [此處](rest-api-cookbook-servertoserver.md).

### 實際限制和端點

目前，預設限制允許每秒最多1個請求，初始高載為3個請求（所識別使用者端第一次互動允許一次性使用，這應允許初始化成功完成）。 這應該不會影響我們所有客戶的任何一般業務案例。

節流機制將在下列端點啟用：

- /o/client/register
- /o/client/token
- /o/client/scopes
- /o/client/validate
- /api/v2/
- /api/v1/tokens/usermetadata
- /api/v1/tokens/authn
- /api/v1/tokens/authz
- /api/v1/tokens/media
- /api/v1/config/
- /api/v1/checkauthn
- /api/v1/logout
- /api/v1/authorize
- /api/v1/preauthorize
- /api/v1/mediatoken
- /api/v1/authenticate/freepreview
- /api/v1/authenticate/
- /api/v1/。+/profile-requests/.+
- /api/v1/identities
- /reggie/v1/。+/regcode
- /reggie/v1/。+/regcode/.+

### SDK實作消除混淆

由於使用Adobe Pass驗證所提供SDK的使用者端不會與任何端點明確互動，本節將介紹已知函式、遇到節流回應時的行為方式以及應採取的動作。

#### setRequestor

使用達到節流閥限制時 `setRequestor` 函式中，SDK會傳回CFG429錯誤代碼，透過 `errorHandler` 回撥。

#### getAuthorization

使用達到節流閥限制時 `getAuthorization` 函式中，SDK會傳回Z100錯誤代碼，透過 `errorHandler` 回撥。

#### checkPreauthorizedResources

使用達到節流閥限制時 `checkPreauthorizedResources` 函式中，SDK會傳回P100錯誤代碼，透過 `errorHandler` 回撥。

#### getMetadata

使用達到節流閥限制時 `getMetadata` 函式中，SDK會透過傳回空白回應 `setMetadataStatus` 回撥。

如需各個特定實作的詳細資訊，請參閱特定的SDK檔案。

- [JavaScript SDK API參考](javascript-sdk-api-reference.md)
- [Android SDK API參考](android-sdk-api-reference.md)
- [iOS/tvOS API參考](iostvos-sdk-api-reference.md)

### API回應變更和回應

當我們確定已違反限制時，我們會以特定回應狀態（HTTP 429太多請求）標示此請求，指示您已在時間間隔內使用指派給使用者裝置（IP位址）的所有Token。

前429個回應經過一秒後，節流就會過期。 每個收到429回應的應用程式都必須等候至少1秒，才能產生新要求。

所有客戶應用程式都應適當地處理「429太多請求」回應。

以下是429回應訊息的範例：

```
HTTP/2 429
date: Tue, 20 Feb 2024 11:21:53 GMT
content-type: text/html
content-length: 166
set-cookie: AWSALB=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/
set-cookie: AWSALBCORS=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/; SameSite=None; Secure
server: openresty
access-control-allow-credentials: true
access-control-allow-methods: POST,GET,OPTIONS,DELETE
access-control-allow-headers: ap_11,ap_42,ap_z,ap_19,ap_21,ap_23,authorization,content-type,pass_sfp,AP-Session-Identifier,AP-Device-Identifier,AP-SDK-Identifier,X-Device-Info
access-control-expose-headers: pass_sfp,Authzf-Error-Code,Authzf-Sub-Error-Code,Authzf-Error-Details
p3p: CP="NOI DSP COR CURa ADMa DEVa OUR BUS IND UNI COM NAV STA"

<html>
<head><title>429 Too Many Requests</title></head>
<body>
<center><h1>429 Too Many Requests</h1></center>
<hr><center>openresty</center>
</body>
</html>
```

## 影響和必要的變更

### 傳遞X-Forwarded-For標頭

使用自訂實作（包括伺服器對伺服器實作）與Pass Authentication API互動的客戶，應確保他們可以擷取其使用者IP位址並正確轉寄，進一步使用X-Forwarded-For標頭以傳遞Authentication API。

另請參閱 [此處](rest-api-cookbook-servertoserver.md) 以取得更多詳細資料。

### 回應新的回應代碼

使用自訂實作（包括伺服器對伺服器實作）與Pass Authentication API互動的客戶，在收到429太多請求後發出的任何後續呼叫都應確保包含至少1秒的等待時間。 此等待期可確保有機會變更此機制並獲得有效的業務回應。

## 節流的案例範例

| 自第一次要求以來的時間 | 已收到回應 | 說明 |
|--------------------------|-----------------------------------|----------------------------------------------------------------------------------------------------------|
| 第二個0 | 來電接收成功狀態代碼 | 1個呼叫已從限制耗用 |
| 第二個0.3 | 來電接收成功狀態代碼 | 從限制耗用1個呼叫，以及標籤為高載的1個呼叫 |
| 第二個0.6 | 來電接收成功狀態代碼 | 從限制耗用1個呼叫，以及標籤為高載的2個呼叫 |
| 第二個0.9 | 來電接收成功狀態代碼 | 從限制耗用1個呼叫，以及標籤為高載的3個呼叫 |
| 第二個1.2 | 來電接收成功狀態代碼 | 已從限制耗用2個呼叫，以及標籤為高載的3個呼叫 |
| 第二個1.4 | 來電接收429狀態碼 | 從限制消耗了2個呼叫，以及標籤為高載的3個呼叫和1個呼叫接收「429請求過多」 |
| 第二個1.6 | 來電接收429狀態碼 | 從限制消耗了2個呼叫，以及標籤為高載的3個呼叫和2個呼叫接收「429請求過多」 |
| 第二個1.8 | 來電接收429狀態碼 | 從限制消耗了2個呼叫，以及標籤為高載的3個呼叫和3個呼叫接收「429請求過多」 |
| 第二個2.1 | 來電接收成功狀態代碼 | 從限制消耗了3個呼叫，標籤為高載的3個呼叫和3個呼叫接收「429請求太多」 |
