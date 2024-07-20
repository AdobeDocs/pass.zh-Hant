---
title: 使用無使用者端API逐步指南的Amazon FireOS SSO
description: 使用無使用者端API逐步指南的Amazon FireOS SSO
exl-id: 4c65eae7-81c1-4926-9202-a36fd13af6ec
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '755'
ht-degree: 0%

---

# 使用無使用者端API逐步指南的Amazon FireOS SSO {#amazon-fireos-sso-using-clientless-api-cookbook}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>

## 簡介 {#Introduction}

本檔案提供使用無使用者端API實作Amazon SSO版Adobe Pass驗證流程的說明。 本檔案的第一部分著重於此架構的Amazon版本特性，適合許多熟悉且有實施經驗的合作夥伴使用。

本檔案的第二部分說明實施Adobe Pass驗證無使用者端API的主要步驟。

如需無使用者端解決方案運作方式的廣泛技術概覽，請參閱[REST API概覽](/help/authentication/rest-api-overview.md)。 如需整體架構和首次實作的相關支援，Adobe是慣用的聯絡方式。

## Amazon無使用者端SSO {#AMZ-Clientless-SSO}

### 高階架構 {#High-Level-Arch}

Amazon無使用者端SSO實作相當簡單，而且大致上等同於一般的Adobe Primetime驗證無使用者端API。

您將需要使用Amazon SDK來擷取個人化裝載，並在呼叫Adobe無使用者端API時使用。

如果可辨識裝載且符合已驗證的工作階段，則無使用者端API會使用您工作階段的Token立即傳回。

### 如何建置應用程式以使用Amazon SDK {#Build-entries}

* 將最新的[Amazon Stub SDK](https://tve.zendesk.com/hc/en-us/article_attachments/360064368131/ottSSOTokenLib_v1.jar)下載並複製到與應用程式目錄平行的/SSOEnabler資料夾
* 更新資訊清單/Gradle檔案以使用程式庫：

  **將下列行加入您的資訊清單檔案：**

  ```Java
  <uses-library android:name="com.amazon.ottssotokenlib" android:required="false"/\>
  ```

  **Gradle檔案專案：**

  在存放庫下：

  ```java
  flatDir {
       dirs '../SSOEnabler'
  }
  ```

  在相依性下方，新增：

  ```Java
  provided fileTree(include: \['ottSSOTokenStub.jar'\], dir: '../SSOEnabler')
  ```


* 處理缺少Amazon隨附應用程式的問題：

  雖然不太可能，但若您的應用程式執行中的Amazon裝置上沒有隨附，您應該會在下列類別的執行階段遇到ClassNotFoundException： `com.amazon.ottssotokenlib.SSOEnabler`。

  如果發生此情況，您只需要略過裝載步驟，並退回一般PrimeTime流程即可。 SSO將不會啟用，但一般驗證流程將正常進行。

</br>

### 如何使用Amazon SDK取得Amazon SSO裝載 {#UseAmazonSSO}

在您的應用程式初始化期間，取得SSOEnabler的執行個體。 根據您的應用程式架構，您應決定是同步實施還是非同步實施。

如果由於任何原因，API呼叫沒有傳回裝載，請使用一般的非SSO流程，並聯絡您的Amazon和Adobe合作夥伴以調查。

**非同步API**

* 取得SSO啟用程式執行個體：

  ```Java
  ssoEnabler = SSOEnabler.getInstance(context);
  SSOEnablerCallback ssoEnablerCallback = new SSOEnablerCallbackImpl();
  ssoEnabler.setSSOTokenCallback(ssoEnablerCallback);
  ```


* 設定回撥

  ```java
  public static abstract class SSOEnablerCallback
  {
          public abstract void getSSOTokenSuccess(Bundle result);
          public abstract void getSSOTokenFailure(Bundle result);
  }
  ```

   * 成功回應套件組合將包含：
      * SSO權杖為具有索引鍵「SSOToken」的字串
   * 失敗回應套件組合將包含：
      * 錯誤碼為int加上索引鍵「ErrorCode」
      * 錯誤說明（含索引鍵「ErrorDescription」的字串）


* 取得SSO權杖

  ```JAVA
  Bundle getSSOTokenAsync(Void);
  ```

* 此API將透過初始期間設定的回呼提供回應。

  **Ex**。 使用在初始化期間建立的單一例項呼叫：

  ```JAVA
  ssoEnabler.getSSOTokenAsync().
  ```


**同步API**

* 取得SSO啟用程式執行個體並設定回呼

  ```JAVA
  ssoEnabler = SSOEnabler.getInstance(context);</span>
  ```

* 取得SSO權杖

  ```JAVA
  Bundle getSSOTokenSync(Void);
  ```

   * 此API將封鎖呼叫者對話串，並以結果套件回應。 由於這是同步呼叫，請勿在主執行緒上使用它。

  ```JAVA
  void setSSOTokenTimeout(long);
  ```

   * 值（毫秒）。 如果設定，覆寫同步API的預設逾時值1分鐘。


### Adobe Pass無使用者端API更新以使用動態使用者端註冊 {#clientlessdcr}

如果這是您的第一個實作，請參閱&#x200B;**無使用者端技術概覽**，並聯絡Adobe以備您需要支援時使用。

Adobe無使用者端API需要應用程式使用動態使用者端註冊，才能呼叫Adobe伺服器。

* 若要在應用程式中使用Dynamic Client Registration，請依照[Dynamic Client Registration Management中的指示來註冊應用程式](/help/authentication/dynamic-client-registration-management.md)。

* 若要實作Dynamic Client Registration API以對Adobe Pass伺服器執行驗證和授權要求，請依照[Dynamic Client Registration API](/help/authentication/dynamic-client-registration-api.md)中的指示操作。

### Adobe Pass無使用者端API更新以使用Amazon SSO {#clientlesssso}

向Amazon驗證端點提出的請求中，必須存在從Amazon SDK取得的Adobe Pass SSO裝載：

```
      /adobe-services/*
      /reggie/*
      /api/*
```


所有Adobe Pass驗證端點都支援下列方法來接收裝置範圍識別碼或平台範圍識別碼(出現在Amazon SSO裝載中)：

* 作為標頭：&quot;Adobe-Subject-Token&quot;
* 作為查詢引數：&quot;ast&quot;
* 作為貼文引數：&quot;ast&quot;


>[!NOTE]
>
>如果以查詢/貼文引數傳送裝置範圍識別碼或平台範圍識別碼，則產生請求簽名時必須包含此識別碼。

>[!NOTE]
>
>使用查詢引數&quot;ast&quot;，整個URL可能會變得非常長且遭到拒絕。 在/authenticate呼叫上，可以略過此引數，因為它是在/regcode呼叫上提供的

**範例：**

**以自訂標頭傳送**

```HTTPS
GET /adobe-services/config/requestor HTTP/1.1 Host: sp-preprod.auth.adobe.com

Adobe-Subject-Token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA
```

**正在以查詢引數的形式傳送**

```HTTPS
GET /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.JlBFhNhNCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com
```


**以張貼引數傳送**


```HTTPS
POST /adobe-services/config/requestor?ast=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJyb2t1IiwiaWF0IjoxNTExMzY4ODAyLCJleHAiOjE1NDI5MDQ4MDIsImF1ZCI6ImFkb2JlIiwic3ViIjoiNWZjYzMwODctYWJmZi00OGU4LWJhZTgtODQzODViZTFkMzQwIiwiZGlkIjoiY2FmZjQ1ZDAtM2NhMy00MDg3LWI2MjMtNjFkZjNhMmNlOWM4In0.Jl\_BFhN\_h\_NCJCDXLwBjy5tt3PtPcqbMKEIGZ6sr2NA

HTTP/1.1
Host: sp.auth.adobe.com Content-Type: multipart/form-data;
boundary=---- WebKitFormBoundary7MA4YWxkTrZu0gW
```

>[!NOTE]
>
>如果Amazon SSO不存在或無效，Adobe Pass驗證將會忽略屬性，且會像SSO不存在一樣執行呼叫。
