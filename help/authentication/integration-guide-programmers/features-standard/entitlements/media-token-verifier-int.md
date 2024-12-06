---
title: 整合媒體權杖驗證器
description: 整合媒體權杖驗證器
exl-id: 1688889a-2e30-4d66-96ff-1ddf4b287f68
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '887'
ht-degree: 0%

---

# 整合媒體權杖驗證器

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 關於媒體權杖驗證器 {#about-media-token-verifier}

授權成功時，Adobe Pass驗證會建立長效授權(AuthZ)權杖。  AuthZ權杖會傳遞至使用者端或儲存在伺服器端，視使用者端的平台而定。  （請參閱[瞭解代號](/help/authentication/kickstart/programmer-overview.md#understanding-tokens)，瞭解代號如何儲存在不同的使用者端系統上，以及其他詳細資料。）


AuthZ權杖會授權網站的使用者檢視指定資源。  其一般存留時間(TTL)為6至24小時，過了這段時間，Token就會過期。 **為了實際檢視存取權，Adobe Pass驗證會使用AuthZ權杖產生您取得的短期媒體權杖，並傳遞至您的媒體伺服器**。 這些短期媒體權杖的TTL非常短（通常為幾分鐘）。


在AccessEnabler整合中，您會透過`setToken()`回呼取得短暫的媒體權杖。 對於無使用者端API整合，您需使用`<SP_FQDN>/api/v1/tokens/media` API呼叫取得短暫的Media Token。 權杖是以純文字傳送的字串，由Adobe簽署，使用基於PKI （公開金鑰基礎架構）的權杖保護。 透過這種PKI式保護，權杖會使用非對稱金鑰簽署，由憑證授權單位核發給Adobe。


因為使用者端沒有驗證權杖，所以惡意使用者可能會使用工具來插入假的`setToken()`呼叫。 因此，在考量使用者是否獲得授權時，**無法**&#x200B;僅依賴已觸發`setToken()`的事實。 您必須驗證短暫的Token本身是否合法。 執行驗證的工具是媒體權杖驗證器程式庫。


>[!TIP]
>
>您必須將傳回的權杖字串的整個長度傳遞至媒體權杖驗證器，以進行驗證。

## 使用媒體權杖驗證器驗證短期權杖 {#validate-short-livedttokens}

我們建議程式設計師將權杖傳送至使用媒體權杖驗證器程式庫的Web服務，以便在實際開始視訊資料流之前驗證權杖。 短期媒體權杖的極短TTL定義為足夠長，以便在產生權杖的伺服器和驗證權杖的伺服器之間發生時鐘同步問題，但不再發生。



[媒體權杖驗證器程式庫](https://adobeprimetime.zendesk.com/auth/v2/login/signin?return_to=https%3A%2F%2Ftve.zendesk.com%2Fhc%2Fen-us%2Farticles%2F204963159-Media-Token-Verifier-library&amp;theme=hc&amp;locale=en-us&amp;brand_id=343429&amp;auth_origin=343429%2Cfalse%2Ctrue){target=_blank}可用於Adobe Pass驗證夥伴。



媒體權杖驗證器程式庫包含在Java封存`mediatoken-verifier-VERSION.jar`中。 程式庫會定義：

* 權杖驗證API （`ITokenVerifier`介面），含JavaDoc檔案
* 用來驗證權杖是否確實來自Adobe的Adobe公開金鑰
* 一個參考實作(`com.adobe.entitlement.test.EntitlementVerifierTest.java`)，顯示如何使用驗證器API，以及如何使用程式庫中包含的Adobe公開金鑰來驗證其來源


封存包含所有相依性和憑證金鑰存放區。 所含憑證金鑰庫的預設密碼為「123456」。

* 驗證程式庫需要JDK 1.5版或更新版本。
* 使用您偏好的JCE提供者進行簽名演演算法「SHA256WithRSA」。


**驗證器程式庫必須是用來分析權杖內容的唯一方法。 程式設計師不應該剖析權杖並自行擷取資料，因為權杖格式無法保證，且未來可能會有所變更。**&#x200B;只有驗證器API才能保證正常運作。 直接剖析字串可能會暫時運作，但會導致日後格式可能變更時的問題。 驗證器API會從Token擷取資訊，例如：

* 權杖是否有效（`isValid()`方法）？
* 繫結至權杖（`getResourceID()`方法）的資源識別碼；這可以與`setToken()`函式回呼的其他引數做比較（而且應該相符）。 如果不符，可能表示有欺詐行為。
* 簽發權杖的時間（`getTimeIssued()`方法）。
* TTL （`getTimeToLive()`方法）。
* 從MVPD （`getUserSessionGUID()`方法）收到的匿名驗證GUID。
* 散發者的ID，用於驗證使用者，如果是這種情況，則為散發者提供驗證的proxy-MVPD。

## 使用驗證器API {#using-verifier-api}

`ITokenVerifier`類別定義您用來驗證指定資源之權杖真實性的方法。 使用`ITokenVerifier`方法來分析回應`setToken()`要求所收到的權杖。


`isValid()`方法是驗證Token的主要方法。 它需要一個引數（資源ID）。 如果您傳遞Null資源ID，此方法只會驗證權杖真實性和有效期間。


`isValid()`方法傳回下列其中一個狀態值：



| VALID_TOKEN | 所有驗證都成功 |
|--------------------|-----------------------------------------|
| INVALID_TOKEN_FORMAT | 權杖格式無效 |
| INVALID_SIGNATURE | 無法驗證權杖真實性 |
| TOKEN_EXPIRED | 權杖TTL無效 |
| INVALID_RESOURCE_ID | 權杖對指定資源無效 |
| ERROR_UNKNOWN | 權杖尚未驗證 |

其他方法可提供特定權杖的資源ID、核發時間和存留時間的特定存取權。

* 使用`getResourceID()`擷取與權杖關聯的資源ID，並將其與從setToken()請求傳回的ID進行比較。
* 使用`getTimeIssued()`擷取簽發權杖的時間。
* 使用`getTimeToLive()`擷取TTL。
* 使用`getUserSessionGUID()`擷取MVPD所設定的匿名化GUID。
* 使用`getMvpdId()`擷取驗證使用者的MVPD識別碼。
* 使用`getProxyMvpdId()`擷取驗證使用者的Proxy MVPD識別碼。

## 程式碼範例 {#sample-code}

媒體權杖驗證器封存包含參考實作(`com.adobe.entitlement.test.EntitlementVerifierTest.java`)和以測試類別叫用API的範例。 此範例(`com.adobe.entitlement.text.EntitlementVerifierTest.java`)說明權杖驗證程式庫與媒體伺服器的整合。


```Java
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
