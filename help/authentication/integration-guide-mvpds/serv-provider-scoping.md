---
title: 服務提供者範圍
description: 服務提供者範圍
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# 服務提供者範圍 {#service-provoider-scoping}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

Adobe Pass驗證與MVPD整合的預設實作是以&#x200B;**OLCA規格**&#x200B;為基礎。 OLCA規格（6.5，主體識別碼）的「驗證需求」區段指出，您可以指示服務提供者(SP)的「主體」識別碼範圍。 （主體識別碼是MVPD傳回SP的模糊化使用者ID。）  在Adobe Pass驗證整合中，MVPD必須啟用SP驗證要求的範圍。

由於Adobe Pass驗證扮演了程式設計師的SP角色，因此必須實作自訂，以啟用驗證請求的SP範圍設定。  必須這樣做，MVPD才能識別傳遞給MVPD的身分提供者(IdP)的SAML宣告中的網路品牌。  範圍設定可透過下一節中說明的兩種方式之一來實施。

## 服務提供者範圍 {#service-provider-scoping}

Adobe Pass驗證支援以下兩種方式來啟用驗證請求的SP領域設定：

* **SAML簽發者方法。**&#x200B;在此方法中，「要求者ID」會附加至SAML驗證要求中的SAML簽發者字串。

* **自訂範圍屬性方法。**&#x200B;在此方法中，「要求者ID」會明確包含在SAML驗證要求中，作為自訂「範圍」屬性。

>[!NOTE]
>
>「請求者ID」是Adobe Pass驗證用來指稱程式設計師網路品牌的方式（例如：「CNN」是Turner網路的品牌之一）。

### SAML簽發者方法 {#saml-issuer-approach}

此方法在SAML驗證請求中使用SAML `<Issuer>`元素，如以下程式碼片段所示：

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### 自訂範圍設定屬性方法 {#custom-scoping-property-approach}

此方法會使用名為「Scoping」的自訂屬性，如以下SAML驗證請求程式碼片段所示：

```xml
...
<samlp:Scoping xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <samlp:RequesterID xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">requestorID</samlp:RequesterID>
</samlp:Scoping>
...
```

<!--
>[!RELATEDINFORMATION]
>* [MVPD Authentication](/help/authentication/authn-usecase.md)
>* **OLCA Specification**
-->
