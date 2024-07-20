---
title: 動態使用者端註冊API
description: 動態使用者端註冊API
exl-id: 06a76c71-bb19-4115-84bc-3d86ebcb60f3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '904'
ht-degree: 0%

---

# 動態使用者端註冊API {#dynamic-client-registration-api}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

目前，Adobe Pass驗證識別及註冊應用程式的方式有兩種：

* 以瀏覽器為基礎的使用者端已透過允許的[網域清單](/help/authentication/programmer-overview.md)登入
* 原生應用程式使用者端(例如iOS和Android應用程式)會透過已簽署的請求者機制進行註冊。

Adobe Pass驗證會建議註冊應用程式的新機制。 此機制於以下段落中說明。

## 應用程式註冊機制 {#appRegistrationMechanism}

### 技術原因 {#reasons}

Adobe Pass驗證中的驗證機制依賴工作階段Cookie，但由於[Android Chrome自訂標籤](https://developer.chrome.com/multidevice/android/customtabs){target=_blank}和[Apple Safari檢視控制器](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}，此目標無法再達成。

由於存在這些限制，Adobe會為所有使用者端引入新的註冊機制。 它以OAuth 2.0 RFC為基礎，包含
，共執行下列步驟：

1. 從TVE擷取軟體陳述式
儀表板
1. 取得使用者端認證
1. 取得存取權杖

### 從TVE儀表板擷取軟體陳述式 {#softwareStatement}

對於您發行的每個應用程式，都需要取得軟體宣告。 應用程式建立後，所有軟體陳述式都會透過TVE Dashboard提供。 軟體陳述式應該與使用者裝置上的應用程式一起部署。

>[!IMPORTANT]
>
>使用軟體陳述式時，將不再需要已簽署的請求者ID機制。

如需有關如何建立軟體陳述式的詳細資訊，請造訪[TVE儀表板中的使用者端註冊](/help/authentication/dynamic-client-registration.md)。

### 取得使用者端認證 {#clientCredentials}

從TVE Dashboard擷取軟體陳述式後，您需要向Adobe Pass授權伺服器註冊應用程式。 執行/register呼叫並擷取您的唯一使用者端識別碼，以執行此操作。

**要求**

| HTTP呼叫 |                    |
|-----------|--------------------|
| 路徑 | /o/client/register |
| 方法 | POST |

| 欄位 |                                                                           |           |
|--------------------|---------------------------------------------------------------------------|-----------|
| software_statement | 在TVE Dashboard中建立的軟體宣告。 | 強制 |
| redirect_uri | 應用程式用來完成驗證流程的URI。 | 可選 |

| 請求標頭 |                                                                                |           |
|-----------------|--------------------------------------------------------------------------------|-----------|
| Content-Type | application/json | 強制 |
| X-Device-Info | 傳遞裝置和連線資訊中定義的裝置資訊 | 強制 |
| User-Agent | 使用者代理 | 強制 |

**回應**

| 回應標頭 |                  |           |
|------------------|------------------|-----------|
| Content-Type | application/json | 強制 |

| 回應欄位 |                 |                            |
|---------------------|-----------------|----------------------------|
| client_id | 字串 | 強制 |
| client_secret | 字串 | 強制 |
| client_id_issued_at | 長 | 強制 |
| redirect_uris | 字串清單 | 強制 |
| grant_types | 字串清單<br/> **接受值**<br/> `client_credentials`：由不安全的使用者端使用，例如Android SDK。 | 強制 |
| 錯誤 | **接受的值**<ul><li>invalid_request</li><li>invalid_redirect_uri</li><li>invalid_software_statement</li><li>unapproved_software_statement</li></ul> | 錯誤流程中的必要專案 |


#### 錯誤回應 {#error-response}

發生錯誤時，註冊伺服器會以HTTP 400 （錯誤請求）狀態代碼回應，並在回應中包含下列引數：

| 狀態代碼 | 回應內文 | 說明 |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;： &quot;invalid_request&quot;} | 請求缺少必要的引數、包含不支援的引數值、重複引數或格式錯誤。 |
| HTTP 400 | {&quot;error&quot;： &quot;invalid_redirect_uri&quot;} | 根據此使用者端已註冊的應用程式，不允許對其使用redirect_uri。 |
| HTTP 400 | {&quot;error&quot;： &quot;invalid_software_statement&quot;} | 軟體陳述式無效。 |
| HTTP 400 | {&quot;error&quot;： &quot;unapproved_software_statement&quot;} | 在設定中找不到software_id。 |

#### 使用者端認證範例 {#client-credentials-example}

**要求：**

```HTTPS
POST /o/client/register HTTP/1.1
    User-Agent: Android
    X-Device-Info: ew0KICAibW9kZWwiOiAiVFYiLA0KICAidmVuZG9yIjogIkFwcGxlIiwNCiAgIm1hbnVmYWN0dXJlciI6ICJBcHBsZSIsDQogICJvc05hbWUiOiAidHZPUyIsDQogICJvc1ZlbmRvciI6ICJBcHBsZSIsDQogICJvc1ZlcnNpb24iOiAiMTAuMiIsDQogICJicm93c2VyVmVuZG9yIjogIkFwcGxlIiwNCiAgImJyb3dzZXJOYW1lIjogIlNhZmFyaSINCn0
    Content-Type: application/json
    Accept: application/json
    Host: sp.auth.adobe.com
 {
        "software_statement": "eyJhbGciOiJSUzI1NiJ9.
    eyJzb2Z0d2FyZV9pZCI6IjROUkIxLTBYWkFCWkk5RTYtNVNNM1IiLCJjbGll
    bnRfbmFtZSI6IkV4YW1wbGUgU3RhdGVtZW50LWJhc2VkIENsaWVudCIsImNs
    aWVudF91cmkiOiJodHRwczovL2NsaWVudC5leGFtcGxlLm5ldC8ifQ.
    GHfL4QNIrQwL18BSRdE595T9jbzqa06R9BT8w409x9oIcKaZo_mt15riEXHa
    zdISUvDIZhtiyNrSHQ8K4TvqWxH6uJgcmoodZdPwmWRIEYbQDLqPNxREtYn0
    5X3AR7ia4FRjQ2ojZjk5fJqJdQ-JcfxyhK-P8BAWBd6I2LLA77IG32xtbhxY
    fHX7VhuU5ProJO8uvu3Ayv4XRhLZJY4yKfmyjiiKiPNe-Ia4SMy_d_QSWxsk
    U5XIQl5Sa2YRPMbDRXttm2TfnZM1xx70DoYi8g6czz-CPGRi4SW_S2RKHIJf
    IjoI3zTJ0Y2oe0_EJAiXbL6OyF9S5tKxDXV8JIndSA",
  "redirect_uri": "adobepass://com.programmer"  }
```

**回應：**

```HTTPS
HTTP/1.1 201 Created
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
    
{
 "client_id": "s6BhdRkqt3",
 "client_secret": "t7AkePiru4",
 "client_id_issued_at": 2893256800,
 "redirect_uris": [
   "app://com.programmer.adobe#sdasdsadas"],
 "grant_types": ["client_credentials"]
}
```

**錯誤回應：**

```HTTPS
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

### 取得存取Token {#accessToken}

擷取應用程式的唯一使用者端識別碼（使用者端ID和使用者端密碼）後，您需要取得存取權杖。 存取權杖是必要的OAuth 2.0權杖，用於呼叫Adobe Pass驗證API。

>[!NOTE]
>
>目前，存取權杖有24小時的存留時間。

**要求**


| **HTTP呼叫** | |
| --- | --- |
| 路徑 | `/o/client/token` |
| 方法 | POST |

| **要求引數** | |
| --- | --- |
| `grant_type` | 在使用者端註冊程式中接收。<br/> **接受的值**<br/>`client_credentials`：用於不安全的使用者端，例如Android SDK。 |
| `client_id` | 在使用者端註冊程式中取得的使用者端識別碼。 |
| `client_secret` | 在使用者端註冊程式中取得的使用者端識別碼。 |

**回應**

| 回應欄位 | | |
| --- | --- | --- |
| `access_token` | 您用來呼叫Adobe Pass API的存取權杖值 | 強制 |
| `expires_in` | access_token過期前的秒數 | 強制 |
| `token_type` | 語彙基元&#x200B;**持有者**&#x200B;的型別 | 強制 |
| `created_at` | 權杖的問題時間 | 強制 |
| **回應標頭** | | |
| `Content-Type` | application/json | 強制 |

**錯誤回應**

發生錯誤時，授權伺服器會以HTTP 400 （錯誤請求）狀態代碼回應，並在回應中包含下列引數：

| 狀態代碼 | 回應內文 | 說明 |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;： &quot;invalid_request&quot;} | 請求缺少必要的引數、包含不受支援的引數值（授予型別除外）、重複引數、包含多個認證、使用多個機制來驗證使用者端，或格式錯誤。 |
| HTTP 400 | {&quot;error&quot;： &quot;invalid_client&quot;} | 使用者端驗證失敗，因為使用者端不明。 SDK必須再次向授權伺服器註冊。 |
| HTTP 400 | {&quot;error&quot;： &quot;unauthorized_client&quot;} | 已驗證的使用者端無權使用此授權授予型別。 |

#### 取得存取Token範例： {#obt-access-token}

**要求：**

```HTTPS
POST o/client/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
    
grant_type=client_credentials&client_id=AAA&client_secret=SSS
```

**回應：**

```JSON
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"bearer",
  "expires_in":3600,
  "created_at":123456789
}
```

**錯誤回應：**

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

## 執行驗證要求 {#autheticationRequests}

使用存取權杖執行Adobe Pass [驗證API呼叫](/help/authentication/initiate-authentication.md)。 為此，需要以下列方式之一將存取權杖新增到API請求中：

* 將新的查詢引數新增到請求中。 該新引數稱為&#x200B;**access_token**。

* 透過將新的HTTP標頭新增到請求：授權：持有人。 建議您使用HTTP標頭，因為查詢字串傾向於顯示在伺服器記錄中。

發生錯誤時，可能會傳回下列錯誤回應：

| 錯誤回應 |     |                                                                                                        |
|-----------------|-----|--------------------------------------------------------------------------------------------------------|
| invalid_request | 400 | 要求的格式錯誤。 |
| invalid_client | 403 | 不再允許使用者端ID執行要求。 應產生新的使用者端認證。 |
| access_denied | 401 | access_token無效（過期或無效）。 使用者端必須要求新的access_token。 |

### 執行驗證要求範例：

**正在傳送存取權杖做為要求引數：**

```HTTPS
GET adobe-services/config?access_token=<access_token>&requestor_id=... HTTP/1.1
    
Host: sp.auth.adobe.com
```

**正在傳送存取權杖做為HTTP標頭：**

```HTTPS
POST adobe-services/sessionDevice?device_id=platformDeviceId HTTP/1.1
    
Authorization: Bearer <access_token>
    
Host: sp.auth.adobe.com
```

**錯誤回應做為回應內文：**

```HTTPS
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error":"invalid_client" }
```

**錯誤回應為URL引數：**

```HTTPS
HTTP/1.1 302 Found
    
Location: adobepass://com.programmer.adobe?error=access_denied
```
