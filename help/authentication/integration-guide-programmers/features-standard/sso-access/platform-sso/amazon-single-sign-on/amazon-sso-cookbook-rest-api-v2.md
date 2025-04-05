---
title: Amazon SSO逐步指南(REST API V2)
description: Amazon SSO逐步指南(REST API V2)
exl-id: 63e4fa63-8ca3-40eb-b49a-84dd75c2ca1d
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 0%

---

# Amazon SSO逐步指南(REST API V2) {#amazon-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

Adobe Pass Authentication REST API V2支援在FireOS上執行之使用者端應用程式的一般使用者使用平台單一登入(SSO)。

此檔案可作為現有[REST API V2總覽](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)的延伸，提供高階檢視和說明如何使用平台識別流程實作[單一登入的檔案](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)。

## 使用平台身分流程的Amazon單一登入 {#cookbook}

Adobe Pass驗證會與Amazon合作，為電視訂閱者改善登入使用者體驗，並促進跨所有電視應用程式的單一登入(SSO)。

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
   * 串流應用程式在執行階段可能遇到下列類別`com.amazon.ottssotokenlib.SSOEnabler`的`ClassNotFoundException`。

* 缺少上述API應傳回的SSO權杖（平台身分）裝載。
   * 串流應用程式可能會聯絡Amazon和Adobe代表進行調查。

### 工作流程 {#workflow}

針對Amazon驗證REST API V2端點發出的所有HTTP請求中，都必須有Adobe Pass SSO權杖（平台身分）裝載：

```
/api/v2/*
```

Adobe Pass驗證REST API V2支援下列方法來接收SSO權杖（平台身分）裝載，此為裝置範圍或平台範圍的識別碼：

* 作為標頭，名稱為： `Adobe-Subject-Token`

>[!IMPORTANT]
> 
> 如需`Adobe-Subject-Token`標頭的詳細資訊，請參閱[Adobe-Subject-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)檔案。

#### 範例

**以標頭傳送**

```HTTPS
GET /api/v2/{serviceProvider}/sessions HTTP/1.1 
Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

>[!IMPORTANT]
>
> 如果`Adobe-Subject-Token`標頭值遺失或無效，則Adobe Pass驗證將處理請求而不考慮單一登入。
