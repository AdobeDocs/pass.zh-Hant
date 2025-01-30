---
title: 媒體權杖
description: 媒體權杖
exl-id: 7e486d2c-e078-464d-90b1-14e2cfb4d20a
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '667'
ht-degree: 0%

---

# 媒體權杖 {#media-tokens}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

媒體權杖是由Adobe Pass驗證[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)產生的Token，此授權決定旨在提供受保護內容（資源）的檢視存取權。

媒體權杖在問題發生時指定的有限和短時間範圍（預設為7分鐘）內有效，這表示在使用者端應用程式必須驗證和使用它之前的時間限制。 媒體權杖僅限於一次性使用，且絕對不可快取。

媒體權杖由以明文傳送的公開金鑰基礎結構(PKI)為基礎的已簽署字串組成。 透過以PKI為基礎的保護，權杖會使用由憑證授權單位(CA)核發給Adobe的非對稱金鑰來簽署。

媒體權杖會傳遞至程式設計師，然後程式設計師可在啟動視訊串流之前，使用媒體權杖驗證器來驗證它，以確保該資源存取的安全性。

媒體權杖驗證器是由Adobe Pass驗證所分發的程式庫，負責驗證媒體權杖的真實性。

## 媒體權杖驗證器 {#media-token-verifier}

Adobe Pass驗證建議程式設計師將媒體權杖傳送至他們自己的後端服務，整合媒體權杖驗證器程式庫，以在起始視訊資料流之前確保安全存取。 媒體權杖的存留時間(TTL)旨在解決權杖產生伺服器和驗證伺服器之間的潛在時鐘同步問題。

Adobe Pass驗證強烈建議不要剖析媒體權杖並直接擷取其資料，因為無法保證權杖格式，且未來可能會變更。 媒體權杖驗證器程式庫應該是唯一用於分析權杖內容的工具。

媒體權杖驗證器程式庫可從下列連結下載：

* https://tve.zendesk.com/hc/en-us/articles/204963159-Media-Token-Verifier-library

媒體權杖驗證器程式庫需要JDK 1.5版或更新版本，並支援使用簽章演演算法(`SHA256WithRSA`)偏好的Java加密延伸(JCE)提供者。

`mediatoken-verifier-VERSION.jar` Java封存所代表的媒體權杖驗證器程式庫包含：

* 公開金鑰Adobe。
* 權杖驗證API (`ITokenVerifier.java`)。
* 參考實作(`com.adobe.entitlement.test.EntitlementVerifierTest.java`)。
* 相依性和憑證金鑰存放區。

>[!IMPORTANT]
> 
> 包含的憑證金鑰存放區的預設密碼為`123456`。

### 方法 {#methods}

`ITokenVerifier`類別定義下列方法：

* 用來驗證媒體權杖的`isValid()`方法。 它接受單一引數，[資源識別碼](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)。 如果提供的資源識別碼為`null`，則方法只會驗證媒體權杖的真實性和有效期間。

  `isValid()`方法傳回下列其中一個狀態值：

  | VALID_TOKEN | 權杖驗證成功 |
  |----------------------|-------------------------------------------|
  | INVALID_TOKEN_FORMAT | 權杖格式無效 |
  | INVALID_SIGNATURE | 無法驗證權杖真實性 |
  | TOKEN_EXPIRED | 權杖TTL無效 |
  | INVALID_RESOURCE_ID | 權杖對指定資源無效 |
  | ERROR_UNKNOWN | 權杖尚未驗證 |

* `getResourceID()`方法用來擷取與媒體權杖關聯的資源識別碼，並將其與授權決定回應傳回的識別碼進行比較。

* 用來擷取發出媒體權杖時間的`getTimeIssued()`方法。

* 用來擷取媒體權杖的TTL的`getTimeToLive()`方法。

* 用來擷取MVPD所設定的匿名GUID的`getUserSessionGUID()`方法。

* `getMvpdId()`方法，用於擷取已驗證使用者的MVPD識別碼。

* `getProxyMvpdId()`方法，用於擷取驗證使用者的Proxy MVPD識別碼。

### 範例 {#sample}

媒體權杖驗證器封存包含參考實作(`com.adobe.entitlement.test.EntitlementVerifierTest.java`)和以測試類別叫用API的範例。 此範例(`com.adobe.entitlement.text.EntitlementVerifierTest.java`)說明媒體權杖驗證器程式庫整合到媒體伺服器中的情形。

```JAVA
package com.adobe.entitlement.test;

import com.adobe.entitlement.verifier.CryptoDataHolder;
import com.adobe.entitlement.verifier.ITokenVerifier;
import com.adobe.entitlement.verifier.ITokenVerifierFactory;
import com.adobe.entitlement.verifier.SimpleTokenPKISignatureVerifierFactory;
import com.adobe.tve.crypto.SignatureVerificationCredential; 
import java.io.InputStream; 

public class EntitlementVerifierTest { 
    String mRequestorID = null;
    String mTokenToVerify = null;
    String mPathToCertificate = null;
    String mKeystoreType = null;
    String mKeystorePasswd = null;
    String mResourceID = null;

    public static void main(String[] args) { 
        if (args == null || args.length < 2 ) {
            System.out.println("Incorrect args: Usage: EntitlementVerifierTest requestorID tokenToVerify [resourceID]");
            return;
        } 
        String requestorID = args[0];
        String tokenToVerify = args[1];
        String pathToCertificate = "media_token_keystore.jks"; // the default keystore provided in the entitlement jar 
        String keystoreType = "jks";
        String keystorePasswd = "123456"; // password for the default keystore 
        if (requestorID == null || tokenToVerify == null) {
            System.out.println("One or more arguments is null");
            return;
        } 
        System.out.println("RequestorID: " + requestorID);
        System.out.println("token: " + tokenToVerify);
        System.out.println("cert: " + pathToCertificate);
        System.out.println("keystoretype: " + keystoreType);
        System.out.println("keystore passwd: " + keystorePasswd);
        String resourceID = null;
        if (args.length > 2) {
            resourceID = args[2];
        }
        System.out.println("Resource ID: " + resourceID);
        EntitlementVerifierTest verifier = new EntitlementVerifierTest(requestorID,
            tokenToVerify, pathToCertificate, keystoreType, keystorePasswd, resourceID);
        verifier.verifyToken();
    } 

    protected EntitlementVerifierTest(String inRequestorID,
                                      String inTokenToVerify,
                                      String inPathToCertificate,
                                      String inKeystoreType,
                                      String inKeystorePasswd, String inResourceID) {
        mRequestorID = inRequestorID;
        mTokenToVerify = inTokenToVerify;
        mPathToCertificate = inPathToCertificate;
        mKeystoreType = inKeystoreType;
        mKeystorePasswd = inKeystorePasswd;
        mResourceID = inResourceID;
    } 

    protected void verifyToken() {
        // It is expected that the SignatureVerificationCredential and 
        // CryptoDataHolder could be created at Init time in a web application 
        // and be reused for all token verifications. 
        CryptoDataHolder cryptoData = createCryptoDataHolder(mPathToCertificate, mKeystoreType, mKeystorePasswd);
        ITokenVerifierFactory tokenVerifierFactory = new SimpleTokenPKISignatureVerifierFactory();
        ITokenVerifier tokenVerifier = tokenVerifierFactory.getInstance(mRequestorID, mTokenToVerify, cryptoData);
        ITokenVerifier.eReturnValue status = tokenVerifier.isValid(mResourceID);
        System.out.println("Is token Valid? : " + status.toString());
        System.out.println("Token User ID: " + tokenVerifier.getUserSessionGUID());
        System.out.println("Token was generated at: " + tokenVerifier.getTimeIssued());

        System.out.println("Token Mvpd ID: " + tokenVerifier.getMvpdId());
        System.out.println("Token Proxy Mvpd ID: " + tokenVerifier.getProxyMvpdId());
    } 
    
    protected CryptoDataHolder createCryptoDataHolder(String pathToCertificate,
                                                      String keystoreType, String keystorePasswd) {
        SignatureVerificationCredential verificationCredential =
            readShortTokenVerificationCredential(pathToCertificate, keystoreType, keystorePasswd);
        CryptoDataHolder cryptoData = new CryptoDataHolder();
        cryptoData.setCertificateInfo(verificationCredential);
        return cryptoData;
    } 
    
    protected SignatureVerificationCredential readShortTokenVerificationCredential(String keystoreFile,
                                                                                   String keystoreType,
                                                                                   String keystorePasswd) {
        SignatureVerificationCredential cred = null; 
        if (keystoreFile != null){
            try {
                // load the keystore file 
                ClassLoader loader = EntitlementVerifierTest.class.getClassLoader();
                InputStream certInputStream =  loader.getResourceAsStream(keystoreFile);
                if (certInputStream != null) {
                    cred = new SignatureVerificationCredential(certInputStream, keystorePasswd, keystoreType);          
                }
            }
            catch (Exception e) {
                System.out.println("Error creating short token server credentials: " + e.getMessage());
            }
        }
        if (cred == null) {
            System.out.println("Error creating short token server credentials");
        } 
        return cred;
    } 
}
```

## REST API V2 {#rest-api-v2}

可使用以下API擷取媒體Token：

* [使用特定mvpd擷取授權決策](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

請參閱上述API的&#x200B;**回應**&#x200B;和&#x200B;**範例**&#x200B;區段，瞭解授權決定和媒體權杖的結構。

如需有關如何及何時整合上述API的詳細資訊，請參閱以下檔案：

* [主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!IMPORTANT]
>
> 使用者端應用程式必須將傳回的`token`中的`serializedToken`值傳遞給[媒體權杖驗證器](#media-token-verifier)進行驗證。
