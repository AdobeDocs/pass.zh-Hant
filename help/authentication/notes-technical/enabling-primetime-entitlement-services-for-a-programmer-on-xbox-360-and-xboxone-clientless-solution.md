---
title: 在Xbox 360和XboxOne無使用者端上啟用程式設計師的Adobe Pass軟體權利檔案服務
description: 在Xbox 360和XboxOne無使用者端上啟用程式設計師的Adobe Pass軟體權利檔案服務
exl-id: ff7254de-9ea4-4c27-a186-d1c2eea12222
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# 在Xbox 360和XboxOne無使用者端上啟用程式設計師的Adobe Pass軟體權利檔案服務 {#enabling-primetime-entitlement-services-for-a-programer-on-xbox-360-and-xboxone-clientless}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。




1. 程式設計師會建立Zendesk票證，藉由提供下列資訊來啟用Adobe Pass驗證無使用者端解決方案的Xbox 360/One：

   1. 平台：例如Xbox 360、Xbox One

   1. 請求者ID：例如netgeo、CNN等。

1. Adobe將會建立X509憑證，並在其結尾設定私密金鑰和密碼。

1. Adobe會在票證中或透過電子郵件向程式設計師提供公開憑證（X509憑證）。

1. 接著，程式設計師需要在GDNP入口網站，為註冊至Microsoft的應用程式安裝該公開憑證。

1. 然後程式設計師會分別向Microsoft Xbox Live服務請求XboxOne或360的JWT （Java Web權杖）或STS權杖，該服務將使用步驟3提供的X509公開憑證進行加密。

1. 這些是包含Xbox裝置不重複deviceId的Token。 使用「x」引數在授權標頭中包含權杖（JWT或STS），如下所示：

   1. 對於Xbox 360，在傳送到Adobe Pass付費電視驗證之前，XSTS權杖必須是Base64編碼。
   1. 對於Xbox One，JWT已正確編碼，因此不應進行額外編碼。

1. 來自Xbox裝置的所有API呼叫都應該包含在x引數中帶有上述權杖的授權標頭。



>[!NOTE]
>
>尤其是Xbox有一些與數位簽署相關的獨特要求。 XBox主控台的裝置ID包含在XSTS權杖中。  對於Xbox 360，這是加密的SAML宣告；對於Xbox One，這是加密的JWT。 XBox主控台應用程式會將整個XSTS權杖傳送給Adobe Pass付費電視驗證。 Adobe Pass pay-TV驗證會使用其公開金鑰來解密Token、剖析Token，然後從中擷取deviceId。

>[!NOTE]
>
>由於XSTS權杖的長度很大，因此XBox主控台具有技術限制：它無法將權杖作為HTTPGET引數傳送到Adobe Pass付費電視驗證API。 為了處理這個問題，Adobe Pass付費電視驗證允許在呼叫API時傳送XSTS權杖作為HTTP標頭「Authorization」的一部分。 XSTS權杖必須使用從Adobe Pass付費電視驗證核發給程式設計師的X.509憑證中的公開金鑰加密。 Adobe Pass付費電視驗證會儲存關聯的私密金鑰，並使用它來解密XSTS權杖並從中擷取deviceId。
