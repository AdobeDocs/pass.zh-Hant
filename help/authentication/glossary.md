---
title: 字彙表
description: 字彙表
exl-id: e64a94f6-7460-4aa8-8d6b-e0553ba1e4ec
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# 字彙表 {#glossary}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## AccessEnabler {#accessEnabler}

Adobe Pass驗證的使用者端元件。 Adobe Pass驗證為每個支援的平台提供AccessEnabler程式庫。

## AuthN {#authn}

用作「驗證」的簡稱，如「AuthN權杖」或「AuthN流量」。


## AuthN權杖{#authn-token}

驗證權杖，在使用者成功透過MVPD驗證後，由Adobe Pass驗證產生。 根據程式設計師的整合平台，代號會儲存在使用者的裝置或Adobe Pass驗證伺服器上。

## AuthZ {#authz}

用作「授權」的簡稱，如「AuthZ代號」或「AuthZ流量」。

## AuthZ權杖 {#authz-token}

授權權杖，在使用者獲得授權可檢視受保護的內容後，由Adobe Pass驗證產生。 AuthZ權杖儲存在Adobe Pass驗證伺服器上，用來產生[短期媒體權杖](#short-lived-token)。

## 管道ID （已棄用） {#channel_id}

資源ID的前一個辭彙。

## 無使用者端API {#clientless-api}

採用Web服務而非AccessEnabler使用者端元件的Adobe Pass驗證整合解決方案。

## 裝置ID {#device-id}

可唯一識別裝置（例如手機、平板電腦等） 在Adobe Pass Authentication中。 此ID由程式設計師的應用程式取得/提供。


## 權益流程{#entitlement_flow}

Adobe Pass驗證檔案中使用的術語，指的是使用Adobe Pass驗證註冊裝置/使用者、使用MVPD驗證使用者、為使用者授權資源以及登出使用者的整個過程。


## GUID {#guid}

請參閱[使用者ID](#user-id)。

## IdP {#idp}

識別提供者；在Adobe Pass驗證整合中，MVPD角色的內容與MVPD同義。 （客戶必須透過其付費電視提供者的登入頁面驗證其身分。）

## 媒體權杖驗證器 {#media-token-verifier}

Adobe提供的資料庫，程式設計師可使用此資料庫來驗證Adobe Pass驗證在成功完成權益流程後產生的短期媒體權杖。

## MVPD {#mvpd}

多頻道視訊節目經銷商；與「付費電視供應商」同義。

## MVPD ID {#mvpd-id}

請參閱[使用者ID](#user-id)。

## 合作夥伴 ID {#partner-id}

Adobe傳遞給MVPD的識別碼，MVPD會用它來識別代表Adobe Pass驗證請求驗證的人員。 有時用於為特定程式設計師設定其UI，有時用於所有程式設計師的設定UI相同，這取決於MVPD的需求。

## 付費電視提供者 {#pay-tv-provider}

與[MVPD](#mvpd)同義。

## 程式設計師 {#programmer}

與「內容提供者」、「帳戶」、「管道」、「服務提供者」、「品牌」等同義。

## Proxy MVPD {#proxy-mvpd}

提供其他MVPD的身分識別服務的MVPD；直接與Adobe Pass驗證整合。

## 代理的MVPD {#proxied-mvpd}

與AdobeSP沒有直接整合，但透過Proxy MVPD整合的MVPD。

## 要求者ID {#requestor-id}

在Adobe Pass驗證中唯一識別[程式設計師](#programmer) （帳戶、品牌、管道或屬性）。 此ID是在帳戶初始設定期間由程式設計師和Adobe決定。 在網頁上，要求者ID與已列入白名單的網域相關聯；將會拒絕任何使用來自外部網域的ID的呼叫。 程式設計師也會將請求者ID用於Analytics。 通常每個程式設計師只有一個要求者ID。 與請求者ID相關的另一項功能是，程式設計師必須向Adobe提供公開憑證，因為setRequestor API呼叫預期會傳送加密資料，用於驗證Adobe Pass驗證系統中的程式設計師。

## 資源ID {#resource-id}

將[程式設計員](#programmer)識別為MVPD的字串或mRSS資源。 這是程式設計師和MVPD之間所共識；Adobe Pass驗證會透過未觸及的傳遞資源ID，因此所有MVPD都必須相同。 只要MVPD知道每個ID代表的意義，程式設計師就可以使用多個資源ID。

## SessionGUID {#sessionGUID}

請參閱[使用者ID](#user-id)。

## 短期媒體Token {#short-lived-token}

此Token由Adobe Pass驗證在成功完成特定使用者的權益程式時產生。 權杖會傳遞給程式設計師，程式設計師在短期媒體權杖上使用Adobe Pass驗證權杖驗證器，來驗證權益程式的安全性。

## 智慧型裝置 {#smart-device}

Adobe Pass驗證檔案中使用的術語，指稱機上盒、遊戲主機和智慧型電視。 這些裝置具有網路功能，但無法轉譯網頁。

## SP{#sp}

服務提供者；這通常指的是Adobe Pass驗證所扮演的SP的&#x200B;*角色*，代表程式設計師與[MVPD](#mvpd)整合。

## 暫時通過 {#temp-pass}

這項功能可讓程式設計師提供暫時性的免費存取權，以支付內容。 存取權是每個請求者，適用於程式設計師指定的時段。

## TTL {#ttl}

存留時間。 這是指定的權杖有效時間長度。

## TVE {#tve}

到處都是電視。

## 使用者ID {#user-id}

唯一識別程式設計師應用程式的使用者，但源自MVPD。 針對不同的使用案例以不同的形式提供。 請參閱[瞭解程式設計師概觀中的使用者ID](/help/authentication/programmer-overview.md#user-ids)。

## 允許清單 {#whitelist}

為了與Adobe Pass驗證通訊而指定為合法的網域清單。

## XSTS權杖 {#xsts-token}

由Microsoft核發的安全性權杖，用於Xbox主控台應用程式開發，用於Xbox/Adobe Pass驗證整合。
