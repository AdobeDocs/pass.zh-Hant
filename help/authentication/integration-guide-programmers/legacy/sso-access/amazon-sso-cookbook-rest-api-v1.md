---
title: Amazon SSO逐步指南(REST API V1)
description: Amazon SSO逐步指南(REST API V1)
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# （舊版） Amazon SSO逐步指南(REST API V1) {#amazon-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

Adobe Pass Authentication REST API V1支援在FireOS上執行之使用者端應用程式的一般使用者之平台單一登入(SSO)。

此檔案可作為提供高階檢視的現有[REST API V1總覽](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)的延伸。

## 使用平台身分流程的Amazon單一登入 {#cookbook}

### 先決條件 {#prerequisites}

繼續使用平台身分識別流程進行Amazon單一登入之前，請確定符合下列先決條件。

#### 整合Amazon SSO SDK {#integrate-amazon-sso-sdk}

串流應用程式必須將單一登入(SSO)的[Amazon SSO SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar)資料庫整合至其組建。

* 將最新的Amazon SSO SDK程式庫下載並複製到與應用程式目錄平行的`/SSOEnabler`資料夾中。

* 更新資訊清單和Gradle檔案，以使用Amazon SSO SDK資料庫。

  **資訊清單：**

  ```JAVA
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false">
  ```

  **Gradle：**

  在存放庫下：

  ```JAVA
  flatDir {
      dirs '../SSOEnabler'
  }
  ```

  在相依性下：

  ```JAVA
  provided fileTree(include: ['ottSSOTokenStub.jar'], dir: '../SSOEnabler')
  ```

#### 使用Amazon SSO SDK {#use-amazon-sso-sdk}

串流應用程式必須使用Amazon SSO SDK來取得SSO權杖（平台身分）裝載。

Amazon SSO SDK提供同步和非同步API來取得SSO權杖（平台身分）裝載。

串流應用程式可以根據其架構選擇兩個選項之一。

##### 非同步API

* 取得`SSOEnabler`執行個體並設定`SSOEnablerCallback`：

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```

  這可以在串流應用程式初始化期間完成。

  ```JAVA
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

  SSO權杖成功回應套件組合將包含：
   * SSO權杖作為具有索引鍵「SSOToken」的`string`。

  <br/>

  SSO權杖失敗回應套件組合將包含：
   * 含有索引鍵「ErrorCode」的`int`錯誤碼。
   * `string`的錯誤描述，含索引鍵「ErrorDescription」。

  <br/>

* 取得SSO權杖：

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

  此API將在初始化期間透過回呼集提供回應。

##### 同步API

* 取得`SSOEnabler`執行個體：

  ```JAVA
  SSOEnabler ssoEnabler = SSOEnabler.getInstance(context);
  ```

* 取得SSO權杖：

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

  此API將封鎖呼叫者對話串，並以結果套件回應。 由於這是同步呼叫，請勿在主執行緒中使用它。

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

  此API會設定同步呼叫的逾時值。 預設逾時值為1分鐘。

#### Amazon SSO後援 {#fallback-amazon-sso}

串流應用程式必須處理從Amazon SSO流程到一般驗證流程的遞補案例。

確認串流應用程式正在處理：

* 缺少應在Amazon裝置上執行的Amazon隨附應用程式。
   * 串流應用程式在執行階段可能遇到下列類別`ClassNotFoundException`的`com.amazon.ottssotokenlib.SSOEnabler`。

* 缺少上述API應傳回的SSO權杖（平台身分）裝載。
   * 串流應用程式可能會聯絡Amazon和Adobe代表進行調查。

### 工作流程 {#workflow}

針對Amazon驗證端點發出的所有HTTP請求中，都必須有Adobe Pass SSO權杖（平台身分）裝載：

```
/adobe-services/*
/reggie/*
/api/*
```

>[!IMPORTANT]
> 
> 串流應用程式可能會略過在`/authenticate`呼叫上傳送Amazon SSO權杖（平台身分）裝載，因為它是在`/regcode`呼叫上提供的。

Adobe Pass驗證支援以下方法來接收SSO權杖（平台身分）裝載，該裝載為裝置範圍或平台範圍的識別碼：

* 作為標頭，名稱為： `Adobe-Subject-Token`
* 作為查詢引數，名稱為： `ast`
* 作為名為`ast`的張貼引數

>[!IMPORTANT]
>
> 如果以查詢引數的形式傳送，則整個URL可能會變得很長並被拒絕。
>
> 如果以查詢/張貼引數的形式傳送，則在產生請求簽名時必須包含此引數。

#### 範例

**以標頭傳送**

```HTTPS
GET /api/v1/config/{requestorId} HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**正在以查詢引數的形式傳送**

```HTTPS
GET /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com
```

**正在以張貼引數的形式傳送**

```HTTPS
POST /api/v1/config/{requestorId}?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA HTTP/1.1
Host: sp.auth.adobe.com 
Content-Type: multipart/form-data;
```

>[!IMPORTANT]
>
> 如果`Adobe-Subject-Token`標頭或`ast`引數值遺失或無效，則Adobe Pass驗證將處理要求而不考慮單一登入。
