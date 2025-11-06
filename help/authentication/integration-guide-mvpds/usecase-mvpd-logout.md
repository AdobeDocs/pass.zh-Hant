---
title: MVPD登出
description: MVPD登出
exl-id: a2b57d02-9688-48e3-beff-1012cd361d0c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 0%

---

# MVPD登出

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

登出使用案例可由傳送至IdP的SAML登出要求或是由呼叫的自訂登出端點實作。  以下請求和回應範例提供SAML登出實作的範例。

## 登出請求範例 {#sample-logout-request}

```XML
<?xml version="1.0" encoding="UTF-8"?>
<samlp:LogoutRequest
        Destination="https://idpcom/slo" ID="_b18de1a2-5bc2-4866-88e3-97a9cbb663ca"
        IssueInstant="2010-08-18T07:18:22.479Z"
        Reason="urn:oasis:names:tc:SAML:2.0:logout:user"
        Version="2.0"
        xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
        xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer>https://saml.sp.auth.adobe.com</saml:Issuer>
    <saml:NameID
        Format="urn:oasis:names:tc:SAML:2.0:nameid format:transient">
            DkCwM54kzVs5coXH0R0IFhPSA9a
    </saml:NameID>
    <samlp:SessionIndex>L4EQmlLCIS-5hHr71z1VfOCEYWk</samlp:SessionIndex>
</samlp:LogoutRequest>
```

## 登出回應範例 {#sample-logout-response}

```xml
<?xml version="1.0" encoding="UTF-8"?>
<samlp:LogoutResponse
        Destination="https://sp.auth-staging.adobe.com/sp/saml/LogoutServiceHTTPRedirectResponse"
        ID="XpsdJNdPp7PEnNxfeBubP.w4e3r"
        InResponseTo="_b18de1a2-5bc2-4866-88e3-97a9cbb663ca"
        IssueInstant="2010-08-18T07:18:24.969Z" Version="2.0"
        xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer  
        xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">https://idp.com/slo
    </saml:Issuer>
    <samlp:Status>
        <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
</samlp:LogoutResponse>
```

<!--
>[!RELATEDINFORMATION]
>* [Content Metadata Exchange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [Preflight Authorization](/help/authentication/mvpd-preflight-authz.md)
>* [MVPD Integration Features](/help/authentication/mvpd-integr-features.md)
-->
