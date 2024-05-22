---
title: Proxy MVPD Web服務
description: Proxy MVPD Web服務
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: f8cef3c41fb7132204c4fa499301c3010f62ca14
workflow-type: tm+mt
source-wordcount: '1003'
ht-degree: 0%

---

# Proxy MVPD Web服務 {#proxy-mvpd-wbservice}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。
>若要使用Proxy MVPD Web服務，您需要：
>- 請要求支援小組為您註冊的應用程式提供軟體宣告
>- 取得存取權杖依據 [動態使用者端註冊](dynamic-client-registration.md)
> 

>[!NOTE]
>
>若要使用Proxy MVPD Web服務，您需要：
>- 請要求支援小組為您註冊的應用程式提供軟體宣告
>- 取得存取權杖依據 [動態使用者端註冊](dynamic-client-registration.md)
> 

## 概觀 {#overview-proxy-mvpd-webserv}

「Proxy MVPD」是一種MVPD，除了管理其自身與Adobe Pass驗證的整合之外，還會代表一組相關聯的「Proxied MVPD」管理權益程式。 此安排對程式設計師而言是透明的。

為了實作ProxyMVPD功能，Adobe Pass驗證提供RESTful Web服務，ProxyMVPDs可透過這些服務提交和擷取ProxiedMVPDs清單。 用於此公用API的通訊協定為REST HTTP，包含下列假設：

- Proxy MVPD會使用HTTPGET方法來擷取目前整合的MVPD清單。
- Proxy MVPD會使用HTTPPOST方法來更新支援的MVPD清單。

## Proxy MVPD服務 {#proxy-mvpd-services}

- [擷取代理的MVPD](#retriev-proxied-mvpds)
- [提交代理的MVPD](#submit-proxied-mvpds)

### 擷取代理的MVPD {#retriev-proxied-mvpds}

擷取目前與已識別的Proxy MVPD整合的Proxy MVPD清單。

| 端點 | 呼叫者 | 要求引數 | 請求標頭 | HTTP方法 | HTTP回應 |
|--------------------------------------------------------------------------|-----------|-----------------------|---------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| &lt;fqdn>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | 授權（必要） | GET | <ul><li> 200 （確定） — 已成功處理要求，且回應包含XML格式的ProxiedMVPD清單</li><li>401 （未獲授權） — 指出下列其中一項：<ul><li>使用者端必須要求新的access_token</li><li>請求來自不在允許清單中的IP位址</li><li>權杖無效</li></ul></li><li>403 （禁止） — 指出提供的引數不支援操作，或Proxy MVPD未設定為Proxy或遺失</li><li>405 （不允許使用方法） — 使用GET或POST以外的HTTP方法。 HTTP方法通常不受支援，或是此特定端點不支援。</li><li>500 （內部伺服器錯誤） — 要求程式期間在伺服器端引發錯誤。</li></ul> |

Curl範例：

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds"`


XML回應範例：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```

### 提交代理的MVPD {#submit-proxied-mvpds}

推動與已識別的Proxy MVPD整合的MVPD陣列。

| 端點 | 呼叫者 | 要求引數 | 請求標頭 | HTTP方法 | HTTP回應 |
|:------------------------------------------------------------------------:|:---------:|-----------------------|:---------------------------------------------------:|:-----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;fqdn>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Authorization （必要） proxied-mvpds （必要） | POST | <ul><li>201 （已建立） — 已成功處理推播</li><li>400 （錯誤請求） — 伺服器不知道如何處理請求：<ul><li>傳入的XML不符合此規格中發佈的結構描述</li><li>代理的mvpds沒有唯一的ID</li><li>已推送的requestorIds不存在400回應代碼的其他Servlet容器原因</li></ul><li>401 （未獲授權） — 指出下列其中一項：<ul><li>使用者端必須要求新的access_token</li><li>請求來自不在允許清單中的IP位址</li><li>權杖無效</li></ul></li><li>403 （禁止） — 指出提供的引數不支援操作，或Proxy MVPD未設定為Proxy或遺失</li><li>405 （不允許使用方法） — 使用GET或POST以外的HTTP方法。 HTTP方法通常不受支援，或是此特定端點不支援。</li><li>500 （內部伺服器錯誤） — 要求程式期間在伺服器端引發錯誤。</li></ul> |

Curl範例：

`curl -X POST -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



XML範例：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```


### 張貼頻率 {#posting-frequency}

Adobe Pass驗證建議，ProxyMVPDs只有在上次推播發生變更時，才應推播其ProxiedMVPDs清單。

### 刪除代理的MVPD {#delete-proxied-freqency}

如果ProxyMVPD推播具有空白ProxiedMVPDs清單的XML記錄，該空白清單會像任何清單一樣儲存在我們的系統中，因此會有效地刪除先前的清單。



## XSD格式 {#xsd-format}

Adobe已定義下列可接受的格式，以便向我們的公用Web服務發佈/擷取代理的MVPD：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns:pxm="http://tve.adobe.com/data/proxiedmvpd"
           targetNamespace="http://tve.adobe.com/data/proxiedmvpd"
           elementFormDefault="qualified"
           version="1.0">
    <xs:complexType name="iframeSize">
        <xs:all>
            <xs:element name="iframeHeight" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeWidth" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
        </xs:all>
    </xs:complexType>
    <xs:complexType name="requestorIds">
        <xs:annotation>
            <xs:documentation>List of requestors/programmers integrated with the proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:sequence>
            <xs:element name="requestorId" type="xs:string" minOccurs="1" maxOccurs="unbounded" nillable="false">
                <xs:annotation>
                    <xs:documentation>The requestor/programmer identifier recognized by Adobe</xs:documentation>
                </xs:annotation>
            </xs:element>
        </xs:sequence>
    </xs:complexType>
    <xs:complexType name="proxiedMvpd">
        <xs:all>
            <xs:element name="id" minOccurs="1" maxOccurs="1" nillable="false">
                <xs:annotation>
                    <xs:documentation>The id must conform to the regular expression: ([a-zA-Z0-9]+((\-)|[_])*)</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:simpleContent>
                        <xs:extension base="xs:string">
                            <xs:attribute name="ProviderID">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:minLength value="1"/>
                                        <xs:maxLength value="128"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:attribute>
                        </xs:extension>
                    </xs:simpleContent>
                </xs:complexType>
            </xs:element>
            <xs:element name="displayName" type="xs:string" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="logoURL" type="xs:anyURI" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeSize" type="pxm:iframeSize" minOccurs="0" maxOccurs="1"/>
            <xs:element name="requestorIds" type="pxm:requestorIds" minOccurs="0" maxOccurs="1"/>
        </xs:all>
    </xs:complexType>
    <xs:element name="proxiedMvpds">
        <xs:annotation>
            <xs:documentation>List of Proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:complexType>
            <xs:sequence>
                <xs:element name="proxiedMvpd" type="pxm:proxiedMvpd" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

**有關元素的附註：**

-   `id` （必要） — 代理的MVPD ID必須是與MVPD名稱相關的字串，且使用下列任一字元（因為程式設計師可能會看到它做為追蹤用途）： — 任何英數字元、底線(「_」)和連字型大小(「 — 」)。
- idID必須符合下列規則運算式：
`(a-zA-Z0-9((-)|_)*)`

    因此，它必須至少有一個字元，以字母開頭，並以任何字母、數字、破折號或底線繼續。

-   `iframeSize` （選用） - iframeSize元素是選用專案，如果MVPD驗證頁面應該在iFrame中，它會定義iFrame的大小。 否則，如果iframeSize元素不存在，則會在完整的瀏覽器重新導向頁面中進行驗證。
-   `requestorIds` （選用） - requestorIds值將由Adobe提供。 一個要求是，代理的MVPD應至少與一個requestorId整合。 如果代理的MVPD元素上不存在「requestorIds」標籤，則該代理的MVPD將與整合在Proxy MVPD下的所有可用請求者整合。
-   `ProviderID` （選用） — 當ID元素上出現ProviderID屬性時，ProviderID的值會在SAML驗證要求上傳送給Proxy MVPD，做為代理的MVPD / SubMVPD ID （而非ID值）。 在此情況下，ID的值將只會用於「程式設計人員」頁面上顯示的MVPD選擇器，而且會由Adobe Pass驗證在內部使用。 ProviderID屬性的長度必須介於1到128個字元之間。

## 安全性 {#security}

請求必須符合下列規則，才能視為有效：

 — 請求標頭必須包含來自的安全性Oauth2存取權杖 [動態使用者端註冊](dynamic-client-registration.md).
 — 要求必須來自允許的特定IP位址。
 — 要求必須透過SSL通訊協定傳送。

請求標頭中任何未在上方列出的引數都會被忽略。

Curl範例：

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/proxiedMvpds"`

## Adobe Pass驗證環境的Proxy MVPD Web服務端點 {#proxy-mvpd-wevserv-endpoints}

- **生產網址：** https://mgmt.auth.adobe.com/control/v3/proxiedMvpds - **暫存URL：** https://mgmt.auth-staging.adobe.com/control/v3/proxiedMvpds - **生產前URL：** https://mgmt-prequal.auth.adobe.com/control/v3/proxiedMvpds - **預先測試URL：** https://mgmt-prequal.auth-staging.adobe.com/control/v3/proxiedMvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
