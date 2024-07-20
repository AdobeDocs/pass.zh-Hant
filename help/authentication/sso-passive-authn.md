---
title: 透過被動驗證的SSO
description: 透過被動驗證的SSO
exl-id: ce45899f-6e94-4bb0-a2c1-51f03bd66d8d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 0%

---

# 透過被動驗證的SSO

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。


## 簡介

本檔案的範圍是說明被動驗證流程的實作，以及這如何使用我們的標準單一登入方法。

## 使用案例

Adobe Pass驗證可啟用應用程式/網站之間的單一登入(SSO)。 使用者使用其MVPD憑證登入後，Adobe Pass驗證會產生代表MVPD驗證工作階段的安全權杖，並使用裝置ID將該權杖繫結至使用者的裝置。 Adobe Pass驗證會將權杖/裝置ID儲存在伺服器或裝置上。

只要權杖仍然有效，使用者就會直接顯示為已驗證。 這可讓使用者較少輸入其認證，同時確保交易安全。



此處詳述的業務使用案例是一項非常具體的要求：使用者必須至少針對每個造訪的網站進行驗證一次。 如此可讓MVPD套用與authN工作階段相關的商業規則，這些規則可能因網路而異。 它與目前的TVE Promise衝突，即使用者只需要登入一次，且將在Adobe Pass驗證生態系統所屬的所有網站上進行驗證。



為了維護商業規則同時保持良好的使用者體驗， MVPD不要求使用者手動提供認證資訊。 我們可以受益於先前設定的工作階段Cookie，並嘗試使用被動流程進行自動重新驗證；從使用者角度來看，他將顯示為自動登入。



為了解決這些問題，我們實作了2種不同的功能：每個網路的驗證和被動驗證支援。 MVPD將支援SAML被動authN，只在IdP上存在authN工作階段時重新驗證使用者，無論建立該工作階段的網站為何。



## 每個網路的驗證

此功能可確保MVPD會收到每個造訪網站的驗證要求一次。 此功能表示Adobe Pass驗證權杖將繫結至請求者ID，因此僅適用於請求它的網路。 因此，一旦使用者在網站「A」上進行驗證以及之後造訪網站「B」，就需要進行驗證。



請注意，由於在MVPD IdP上，使用者已經通過驗證，因此不需要提供登入資訊，而是將瀏覽器從網站「B」重新導向至MVPD IdP，然後再重新導向。 如果同一位使用者回訪，他仍會在網站「A」上通過驗證。



以下流程說明每個網路的基本驗證功能：





## 被動驗證

這樣做的目標是讓重新驗證程式在背景執行，而不需要完整的瀏覽器重新導向，也不需要顯示選擇器。 因此，從網站A移至網站B的使用者將會自動通過驗證。



從UX的觀點來看，此流程與使用一般MVPD執行的流程之間沒有差異。 使用者看到的是，在因造訪網站A而輸入認證後，它將在網站B上自動驗證。



為了讓此流程可行，MVPD將需要支援被動驗證，以便在MVPD不再有工作階段的情況下，隱藏的iframe不會在登入頁面上「卡住」。 這可透過標準「isPassive」屬性來完成。



下圖說明改善的流程和「幕後」被動驗證：





SAML要求範例
以下是被動式authN流程的SAML要求範例：


```xml
<saml2p:AuthnRequest xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol"
                     AssertionConsumerServiceURL="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                     Destination="https://mvpd_idp_url"
                     ForceAuthn="false"
                     ID="_15056686-399c-4528-b21a-4a9542cfc8ec"
                     IsPassive="true"
                     IssueInstant="2014-11-03T14:18:12.394Z"
                     ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
                     Version="2.0"
                     >
    <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth.adobe.com </saml2:Issuer>
    <saml2p:Extensions>
        <thrpty:RespondTo xmlns:thrpty="urn:oasis:names:tc:SAML:protocol:ext:third-party">https://saml.sp.auth.adobe.com</thrpty:RespondTo>
    </saml2p:Extensions>
    <saml2p:NameIDPolicy AllowCreate="true"
                         Format="urn:oasis:names:tc:SAML:2.0:nameid-format:transient"
                         SPNameQualifier="https://saml.sp.auth.adobe.com"
                         />
</saml2p:AuthnRequest>
```

## 業務規則

MVPD具有特定的SSO範圍網域限制。 例如，部分MVPD只允許部分網域（SSO適用於相同媒體公司，但不適用於跨公司）。
有些MVPD可能需要執行不同的驗證規則。 例如，MVPD在不同的網路中可能有不同的驗證TTL。 此外，MVPD可能會為某些網路啟用以住家為基礎的驗證，但不會為其他網路啟用（家長控制使用案例在此強烈呈現）。


這些業務需求應牢記，主要使用案例為不應要求使用者在成功使用其MVPD登入後再次登入。

您可以使用具有被動驗證N旗標的每個網路驗證來達成此目的。



## 已知限制

iOS — 由於iOS本機儲存的性質，SSO流程不適用於不同廠商開發的應用程式iOS。 如需iOS 8及更高版本中SSO的詳細資訊，請參閱此技術檔案。


<!--
>[!RELATEDINFORMATION]
>* Single Sign-On on iOS
>* SSO on iOS when using the Adobe Pass Authentication Access Enabler
-->
