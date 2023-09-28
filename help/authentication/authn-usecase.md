---
title: MVPD驗證
description: MVPD驗證
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1908'
ht-degree: 0%

---

# MVPD驗證 {#mvpd-authn}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#mvpd-authn-overview}

實際的服務提供者(SP)角色由程式設計師擔任，但Adobe Pass驗證可作為該程式設計師的SP Proxy。 使用Adobe Pass驗證作為中介，可讓MVPD和程式設計師都無須根據個別案例來自訂其權益程式。

當程式設計師從支援SAML的MVPD要求驗證時，以下步驟會使用Adobe Pass驗證呈現事件的序列。 請注意，Adobe Pass驗證存取啟用程式元件在使用者/訂閱者的使用者端上處於作用中狀態。 Access Enabler從此協助進行驗證流程的所有步驟。

1. 當使用者請求存取受保護的內容時，Access Enabler會代表程式設計師(SP)起始驗證(AuthN)。
1. SP的應用程式會向使用者顯示「MVPD選擇器」，以取得他們的付費電視提供者(MVPD)。 SP接著會將使用者的瀏覽器重新導向至所選MVPD的身分提供者(IdP)服務。  這是&quot;**程式設計師啟動的登入**「。  MVPD會將IdP的回應傳送至Adobe的SAML宣告取用者服務，並在該處進行處理。
1. 最後，存取啟用程式會將瀏覽器重新導向回SP網站，通知SP AuthN要求的狀態（成功/失敗）。

## 驗證要求 {#authn-req}

如上述步驟所示，在AuthN流程期間，MVPD必須同時接受以SAML為基礎的AuthN要求並傳送SAML AuthN回應。

此 [線上內容存取(OLCA)驗證與授權介面規格](https://www.cablelabs.com/specifications/search?query=&amp;category=&amp;subcat=&amp;doctype=&amp;content=false&amp;archives=false){target=_blanck} 提供標準AuthN請求和回應。 雖然Adobe Pass驗證不要求MVPD將其權益訊息設定在此標準上，但檢視規格可以提供AuthN交易所需關鍵屬性的深入分析。

>[!NOTE]
>
>MVPD透過Adobe Pass驗證接收的AuthN要求包含數位簽名。 不過，為了簡短起見，下列範例不會顯示簽章。 如需顯示數位簽章的範例，請參閱中的範例： [驗證回應](#authn-response) ，請於下列章節中說明。

SAML驗證請求範例：

```XML
<?xml version="1.0" encoding="UTF-8"?>
<samlp:AuthnRequest  
    AssertionConsumerServiceURL=http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer          
    Destination=http://idp.com/SSOService
    ForceAuthn="false"
    ID="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
    IsPassive="false"
    IssueInstant="2010-08-03T14:14:54.372Z"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
    Version="2.0"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
        http://saml.sp.adobe.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds=_signature_block_goes_here_
    </ds:Signature>
    <samlp:NameIDPolicy
        AllowCreate="true"
        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
        SPNameQualifier="http://saml.sp.adobe.adobe.com"/>
</samlp:AuthnRequest> 
```

下表說明驗證請求中所需的屬性和標籤，以及預設預期值。

**SAML驗證請求詳細資料**

| samlp：AuthnRequest | &lt;authnrequest> 由服務提供者核發給身分提供者。 |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AssertionConsumerServiceURL | 這是要在後續回應中使用的Adobe端點。 預設值： **http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| 目的地 | URI參照，指出此要求已傳送到的位址。 這有助於防止將請求惡意轉寄給非預期的收件者，這是某些通訊協定繫結所需的保護。 如果存在，則實際的收件者必須檢查URI參照是否識別收到訊息的位置。 如果不適用，則必須捨棄要求。 某些通訊協定連結可能需要使用此屬性。 |
| ForceAuthn | ForceAuthn屬性（如果存在，且值為true）會強制身分提供者重新建立此身分，而非依賴其可能與主體相關的現有工作階段。 |
| ID | 要求的識別碼。 另請參閱 [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} 第1.3.4節以取得詳細資料。 |
| IsPassive | Boolean值。 如果為「true」，則身分提供者和使用者代理程式本身「不得」明顯從請求者取得使用者介面的控制權，並以明顯的方式與簡報者互動。 如果未提供值，預設值為「false」。 |
| Isseminstant | 回應問題時的即時時間。 時間值會以UTC編碼，如所述 [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} 區段1.3.3。 |
| ProtocolBinding | URI參照，識別傳回 &lt;response> 訊息。 另請參閱 [SAMLBind] 以取得有關協定繫結及為其定義的URI參照的詳細資訊。 預設值： urn:oasis:名稱:tc:SAML：2.0:bindings:HTTPPOST |
| 版本 | 此請求的版本。 |
| saml：Issuer | 識別產生回應訊息的實體。 （如需此元素的詳細資訊，請參閱SAML core 2.0-os第2.2.5節） |
| ds：簽章 | XML簽章保護判斷提示的完整性並驗證其簽發者，如SAML core 2.0-os的第5節所述 |
| samlp：NameIDPolicy | 指定用來表示請求之主旨之名稱識別碼的限制條件。 |
| 允許建立 | 布林值，用來指出是否允許身分提供者在完成要求過程中建立新識別碼來代表主體。 預設： true |
| 格式 | 指定與名稱識別碼格式對應的URI參照Default： urn:oasis:名稱:tc:SAML：2.0:nameid-format:暫時Adobe建議： urn:oasis:名稱:tc:SAML：2.0:nameid-format:持續 |
| SPNameQualifier | 選擇性地指定在要求者以外的服務提供者的名稱空間中傳回（或建立）宣告主體識別碼。 預設：http://saml.sp.adobe.adobe.com |

## 驗證回應 {#authn-response}

收到並處理驗證要求後，MVPD現在必須傳送驗證回應。

**SAML驗證回應範例**

```XML
<?xml version="1.0" encoding="UTF-8"?> 
<samlp:Response Destination="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_0ac3a9dd5dae0ce05de20912af6f4f83a00ce19587"                             
                InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                IssueInstant="2010-08-17T11:17:50Z" Version="2.0"              
                xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
                xmlns:xs="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
             http://idp.com/SSOService
    </saml:Issuer>
    <samlp:Status>
       <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="pfxb0662d76-17a2-a7bd-375f-c11046a86742"
                   IssueInstant="2010-08-17T11:17:50Z"
                   Version="2.0">
        <saml:Issuer>http://idp.com/SSOService</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
          <ds:SignedInfo>
            <ds:CanonicalizationMethod
                     Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod
                     Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
            <ds:Reference URI="#pfxb0662d76-17a2-a7bd-375f-c11046a86742">
              <ds:Transforms>
                 <ds:Transform
                    Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>        
                 <ds:Transform
                            Algorithm=http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </ds:Transforms>
              <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
              <ds:DigestValue>LgaPI2ASx/fHsoq0rB15Zk+CRQ0=</ds:DigestValue>
            </ds:Reference>
          </ds:SignedInfo>
          <ds:SignatureValue>
                POw/mCKF__shortened_for_brevity__9xdktDu+iiQqmnTs/NIjV5dw==
          </ds:SignatureValue>
          <ds:KeyInfo>
            <ds:X509Data>
                <ds:X509Certificate>
                 MIIDVDCCAjygAwIBA__shortened_for_brevity_utQ==
                </ds:X509Certificate>
            </ds:X509Data>
          </ds:KeyInfo>
      </ds:Signature>
      <saml:Subject>
        <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                     SPNameQualifier="https://saml.sp.auth.adobe.com">
            _5afe9a437203354aa8480ce772acb703e6bbb8a3ad
        </saml:NameID>
        <saml:SubjectConfirmation
                     Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml:SubjectConfirmationData
                  InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                  NotOnOrAfter="2010-08-17T11:22:50Z"                                          
                  Recipient="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"/>
           </saml:SubjectConfirmation>
       </saml:Subject>
       <saml:Conditions NotBefore="2010-08-17T11:17:20Z"
                        NotOnOrAfter="2010-08-17T19:17:50Z">
           <saml:AudienceRestriction>
              <saml:Audience>https://saml.sp.auth.adobe.com</saml:Audience>
           </saml:AudienceRestriction>
       </saml:Conditions>
       <saml:AuthnStatement AuthnInstant="2010-08-17T11:17:50Z"
                   SessionIndex="_1adc7692e0fffbb1f9b944aeafce62aaa7d770cd9e">
        <saml:AuthnContext>
            <saml:AuthnContextClassRef>
                   urn:oasis:names:tc:SAML:2.0:ac:classes:Password
            </saml:AuthnContextClassRef>
        </saml:AuthnContext>
    </saml:AuthnStatement>
  </saml:Assertion>
</samlp:Response>
```


在上述範例中，AdobeSP預期會從Subject/NameId中擷取使用者ID。 AdobeSP可設定為從自訂已定義的屬性取得使用者ID；回應應包含如下元素：

```XML
<saml:AttributeStatement>
     <saml:Attribute Name="guid" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue xsi:type="xs:string">
               71C69B91-F327-F185-F29E-2CE20DC560F5
         </saml:AttributeValue>
    </saml:Attribute>
</saml:AttributeStatement>
```

**SAML驗證回應詳細資料**
| samlp：Response |由Adobe Pass驗證收到的回應。                                                                                                                                                                                                                                                                                                                                                                                                                       | ------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| |目的地 | URI參照，指出此要求已傳送到的位址。 這有助於防止將請求惡意轉寄給非預期的收件者，這是某些通訊協定繫結所需的保護。 如果存在，則實際的收件者必須檢查URI參照是否識別收到訊息的位置。 如果不適用，則必須捨棄要求。 某些通訊協定連結可能需要使用此屬性。 | | ID |要求的識別碼。 其型別為xs：ID，且必須遵循以下的1.3.4節中指定的要求 [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} 用於識別碼的唯一性。 要求中ID屬性的值與對應回應中的InResponseTo屬性值必須相符。                                                                                                                                                                                         | | InResponseTo | SAML通訊協定訊息的ID，測試實體可針對其顯示宣告。 該值必須等於在驗證請求中傳送的ID屬性中的值。 請參閱SAML core 2.0-os | | IssueInstant |發出請求的時間點。                                                                                                                                                                                                                                                                                                                                                                                                                                  | |版本 |要求的版本。                                                                                                                                                                                                                                                                                                                                                                                                                                                | | saml：Issuer |識別產生請求訊息的實體。 (如需此元素的詳細資訊，請參閱第2.2.5小節。SAML core 2.0-os ) | | samlp：Status |代表對應要求之狀態的程式碼。                                                                                                                                                                                                                                                                                                                                                                                                               | | samlp：StatusCode |代表回應對應要求時所執行活動狀態的程式碼。                                                                                                                                                                                                                                                                                                                                                                       | | saml：Assertion |此型別會指定所有判斷提示的共同基本資訊。                                                                                                                                                                                                                                                                                                                                                                                                        | | ID |此判斷提示的識別碼。                                                                                                                                                                                                                                                                                                                                                                                                                                         | |版本 |此判斷提示的版本。                                                                                                                                                                                                                                                                                                                                                                                                                                             | | IssueInstant |發出請求的時間點。                                                                                                                                                                                                                                                                                                                                                                                                                                  | | ds：簽章 | XML簽章保護判斷提示的完整性並驗證其簽發者，如SAML core 2.0-os的第5節所述 | | ds：SignedInfo | SignedInfo的結構包含標準化演演算法、簽章演演算法，以及一或多個參考。 SignedInfo元素可能包含選用的ID屬性，可讓其他簽名和物件參照該屬性。 請參閱XML簽名語法和處理 | | ds：CanonicalizationMethod | CanonicalizationMethod是必要的元素，它會在執行簽章計算之前，指定套用至SignedInfo元素的標準化演演算法。 請參閱XML簽名語法和處理 | | ds：SignatureMethod | SignatureMethod是指定用於產生和驗證簽章的演演算法的必要元素。 此演演算法會識別簽章作業中涉及的所有密碼編譯函式（例如雜湊處理、公開金鑰演演算法、MAC、填補等） 請參閱XML簽名語法和處理 | | ds：Reference |參考是可能發生一次或多次的元素。 它指定摘要演演算法和摘要值，以及選擇性地指定要簽署的物件的識別碼、物件的型別，和/或在摘要之前要套用的轉換清單。 請參閱XML簽名語法和處理 | | ds：轉換 |選用的Transforms元素包含Transform元素的有序清單；這些元素說明簽署者如何取得已擷取的資料物件。 每個轉換的輸出會作為下一個轉換的輸入。 第一個轉換的輸入是取消參考Reference元素的URI屬性的結果。 上次轉換的輸出是DigestMethod演演算法的輸入。 請參閱XML簽名語法和處理 | | ds：DigestMethod | DigestMethod是必要元素，可識別要套用至已簽署物件的摘要演演算法。 請參閱XML簽名語法和處理 | | ds：DigestValue | DigestValue是包含摘要之編碼值的元素。 摘要一律使用base64編碼。 請參閱XML簽名語法和處理 | | ds：SignatureValue | SignatureValue元素包含數位簽章的實際值；一律使用base64編碼。 請參閱XML簽名語法和處理 | | ds：KeyInfo | KeyInfo是選擇性元素，可讓收件者取得驗證簽名所需的金鑰。 請參閱XML簽名語法和處理 | | ds：X509Data | KeyInfo內的X509Data元素包含金鑰或X509憑證的一或多個識別碼。 請參閱XML簽名語法和處理 | | ds：X509憑證 |包含base64編碼的X509Certificate元素 [X509v3] 憑證 | | saml：Subject |判斷提示中陳述式的主旨。                                                                                                                                                                                                                                                                                                                                                                                                                          | | saml：NameID | &lt;nameid> 元素屬於NameIDType型別（請參閱SAML core 2.0-os中的第2.2.2小節），用於各種SAML判斷提示建構，例如 &lt;subject> 和 &lt;subjectconfirmation> 元素中，以及各種通訊協定訊息中。                                                                                                                                                                                                                                           | |格式 |代表字串型識別碼資訊分類的URI參照。                                                                                                                                                                                                                                                                                                                                                                                    | | SPNameQualifier |進一步限定具有服務提供者名稱或服務提供者附屬機構的名稱。 此屬性提供依據信賴方組成名稱的額外方法。                                                                                                                                                                                                                                                                      | | saml：SubjectConfirmation |允許確認主體的資訊。 如果提供了多個主旨確認，則滿足其中任何一個就足以確認主旨以套用宣告。                                                                                                                                                                                                                                                    | | saml：SubjectConfirmationData |要由特定確認方法使用的其他確認資訊。 例如，此元素的典型內容可能是 <!--<ds:KeyInfo>--> 在XML簽章語法和處理規格中定義的元素 | | NotOnOrAfter |無法再確認主旨的即時。                                                                                                                                                                                                                                                                                                                                                                                                            | |收件者 |一個URI，指定驗證實體可以顯示宣告的實體或位置。 例如，此屬性可能表示必須將宣告傳送至特定網路端點，以防止中介將宣告重新導向至其他位置。                                                                                                                                                                                   | | saml：Conditions | &lt;condition> 元素可作為新條件的擴充點。                                                                                                                                                                                                                                                                                                                                                                                                   | | NotBefore | NotBefore屬性指定有效間隔開始的時間。                                                                                                                                                                                                                                                                                                                                                                                  | | saml：AudienceRestriction | &lt;audiencerestriction> element指定此宣告傳送給一或多個由所識別的特定對象。 &lt;audience> 元素。                                                                                                                                                                                                                                                                                                                           | | saml：Audience |識別預期對象的URI參照。                                                                                                                                                                                                                                                                                                                                                                                                                      | | saml：AuthnStatement |驗證陳述式。                                                                                                                                                                                                                                                                                                                                                                                                                                               | |立即驗證 |指定驗證發生的時間。                                                                                                                                                                                                                                                                                                                                                                                                                 | |工作階段索引 |指定由主體識別的主體與驗證授權單位之間特定工作階段的索引。                                                                                                                                                                                                                                                                                                                                              | | saml：AuthnContext |驗證授權單位所使用的內容，包括產生此陳述式的驗證事件。                                                                                                                                                                                                                                                                                                                                                 | | saml：AuthnContextClassRef |識別驗證內容類別的URI參照，該驗證內容類別描述緊接在後面的驗證內容宣告。 |
