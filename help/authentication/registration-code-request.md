---
title: 註冊頁面
description: 註冊頁面
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# 註冊頁面 {#registration-page}

## REST API端點 {#clientless-endpoints}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!NOTE]
>
> REST API實作已由[節流機制](/help/authentication/throttling-mechanism.md)限制

&lt;REGGIE_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## 說明 {#create-reg-code-svc}

傳回隨機產生的註冊代碼和登入頁面URI。

| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode</br>例如：</br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | 串流應用程式</br>或</br>程式設計師服務 | 1.要求者</br>    （路徑元件）</br>2。  deviceId （雜湊）   </br>    （必要）</br>3。  device_info/X-Device-Info （必要）</br>4。  mvpd （選用）</br>5。  ttl （選擇性）</br>6。  _deviceType_</br> 7。  _deviceUser_ （已棄用）</br>8。  _appId_ （已棄用） | POST | 包含註冊代碼的XML或JSON，以及失敗時的資訊或錯誤詳細資料。 請參閱下面的結構描述和範例。 | 201 |

{style="table-layout:auto"}

| 輸入引數 | 說明 |
| --- | --- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| deviceId | 裝置識別碼位元組。 |
| device_info/</br>X-Device-Info | 串流裝置資訊。</br>**注意**：這可以作為URL引數傳遞device_info，但由於此引數的潛在大小以及GETURL長度的限制，它應該作為X-Device-Info傳遞到http標頭。 </br>檢視[傳遞裝置和連線資訊](/help/authentication/passing-client-information-device-connection-and-application.md)中的完整詳細資料。 |
| mvpd | 此作業適用的MVPD ID。 |
| ttl | 此regcode應在秒記憶體留多久。</br>**注意**： ttl允許的最大值為36000秒（10小時）。 較高的值會導致400 HTTP回應（錯誤請求）。 如果`ttl`留空，Adobe Pass驗證會設定30分鐘的預設值。 |
| _deviceType_ | 裝置型別（例如Roku、PC）。</br>若此引數設定正確，ESM提供的量度在使用Clienless時，會針對每個裝置型別](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type)進行[劃分，因此可以執行不同型別的分析，例如Roku、AppleTV和Xbox。</br>檢視，[在傳遞量度中使用無使用者端裝置型別引數的好處&#x200B;](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**注意**： device_info將會取代此引數。 |
| _deviceUser_ | 裝置使用者識別碼。 |
| _appId_ | 應用程式id/名稱。 </br>**注意**： device_info會取代此引數。 |

{style="table-layout:auto"}


>[!CAUTION]
>
>**串流裝置IP位址**
></br>
>對於使用者端對伺服器實作，串流裝置IP位址會與此呼叫一併隱含傳送。  對於伺服器對伺服器實作，其中發出&#x200B;**regcode**&#x200B;呼叫是程式設計人員服務，而不是串流裝置，以下標頭是傳遞串流裝置IP位址的必要專案：
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>其中`<streaming\_device\_ip>`是串流裝置的公用IP位址。
></br></br>
>範例：</br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1</br>X-Forwarded-For:203.45.101.20
>```
>
</br>

### 回應XML結構描述 {#xml-schema}


#### 註冊代碼XSD {#registration-code-xsd}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="model.mvc.reggie.pass.adobe.com"
            targetNamespace="model.mvc.reggie.pass.adobe.com"
            attributeFormDefault="unqualified"
            elementFormDefault="unqualified">
        <xs:element name="regcode">
            <xs:complexType>
                <xs:all>
                    <xs:element name="id" type="xs:string" />
                    <xs:element name="code" type="xs:string" />
                    <xs:element name="requestor" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="mvpd" type="xs:string" minOccurs="1" maxOccurs="1"/
                    <xs:element name="generated" type="xs:long" />
                    <xs:element name="expires" type="xs:long" />
                    <xs:element name="info" type="infoType" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
        <xs:complexType name="infoType">
            <xs:all>
                <xs:element name="deviceId" type="xs:base64Binary" minOccurs="1" maxOccurs="1"/>
                <xs:element name="deviceType" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="deviceUser" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appId" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appVersion" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="registrationURL" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
            </xs:all>
        </xs:complexType>
    </xs:schema>
```

</br>

| 元素名稱 | 說明 |
| --------------- | ------------------------------------------------------------------------------------ |
| id | 註冊代碼服務產生的UUID |
| 程式碼 | 註冊代碼服務產生的註冊代碼 |
| 要求者 | 要求者ID |
| mvpd | Mvpd ID |
| 已產生 | 註冊代碼建立時間戳記（自1970年1月1日GMT起以毫秒為單位） |
| 過期 | 註冊代碼過期的時間戳記（自1970年1月1日以來以毫秒為單位GMT） |
| deviceId | 不重複裝置ID （或XSTS權杖） |
| deviceType | 裝置型別 |
| deviceuser | 使用者已登入裝置 |
| appId | 應用程式ID |
| appVersion | 應用程式版本 |
| 註冊URL | 要顯示給一般使用者的登入網頁應用程式URL |

{style="table-layout:auto"}


</br>

### 錯誤訊息XSD  {#error-message}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="rest.pass.adobe.com"
               targetNamespace="rest.pass.adobe.com"
               attributeFormDefault="unqualified"
               elementFormDefault="unqualified">
        <xs:element name="error">
            <xs:complexType>
                <xs:all>
                    <xs:element name="status" type="xs:int" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="message" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="details" type="xs:string" minOccurs="0" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
    </xs:schema>
```


### 範例回應 {#sample-response}

**XML：**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <ns2:regcode xmlns:ns2="model.mvc.reggie.pass.adobe.com">
        <id>678f9fea-a1cafec8-1ff0-4a26-8564-f6cd020acf13</id>
        <code>TJJCFK</code>
        <requestor>sampleRequestorId</requestor>
        <mvpd>sampleMvpdId</mvpd>
        <generated>1348039846647</generated>
        <expires>1348043446647</expires>
        <info>
            <deviceId>dGhpc0lkQUR1bW15RGV2aWNlSWQ=</deviceId>
            <deviceType>xbox</deviceType>
            <deviceUser>JD</deviceUser>
            <appId>2345</appId>
            <appVersion>2.0</appVersion>
            <registrationURL>http://loginwebapp.com</registrationURL>
        </info>
    </ns2:regcode>
```

**JSON：**

```JSON
    {
        "id": "678f9fea-9d364b29-246c-488f-b97e-298566d1b9e0",
        "code": "D4BDU2W",
        "requestor": "sampleRequestorId",
        "mvpd": "sampleMvpdId",
        "generated": 1348039555877,
        "expires": 1348043155877,
        "info": {
            "deviceId": "dGhpc0l.kQUR1bW15RGV2.aWNlSWQ=",
            "deviceType": "xboxOne",
            "deviceUser": "JD",
            "appId": "2345",
            "appVersion": "2.0",
            "registrationURL": "http://loginwebapp.com"
        }
    }
```
