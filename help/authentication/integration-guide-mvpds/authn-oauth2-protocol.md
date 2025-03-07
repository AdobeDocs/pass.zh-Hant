---
title: 使用OAuth 2.0通訊協定進行驗證
description: 使用OAuth 2.0通訊協定進行驗證
exl-id: 0c1f04fe-51dc-4b4d-88e7-66e8f4609e02
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1088'
ht-degree: 0%

---

# 使用OAuth 2.0通訊協定進行驗證

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

雖然SAML仍然是美國MVPD和一般企業用於驗證的主要通訊協定，但改用OAuth 2.0作為主要驗證通訊協定的趨勢已很明顯。 OAuth 2.0通訊協定(https://tools.ietf.org/html/rfc6749)主要是為消費者網站開發，並很快被Facebook、Google和Twitter等網際網路巨頭採用。

OAuth 2.0非常成功，這促使企業慢慢升級其基礎架構以支援。



## 移至OAuth 2.0的優勢 {#adv-oauth2}

從高層面來看，OAuth 2.0通訊協定提供與SAML通訊協定相同的功能，但有一些重要區別。

其中之一是重新整理權杖流程可用於在幕後重新整理驗證的方式。 這可讓IdP （在此情況下為MVPD）在仍允許良好使用者體驗的同時維持控制，因為使用者因安全性考量而不再需要經常登入。

通訊協定也提供更大的彈性，現在服務提供者可以使用權杖存取其他API以取得額外資訊。 這進而為TVE使用案例產生「聊天」通訊協定，但可讓您擁有複雜工作流程所需的彈性。





## 切換至OAuth 2.0的需求 {#oauth-req}

若要支援使用OAuth 2.0的驗證，MVPD必須符合下列先決條件：

首先，MVPD必須確定它支援[授權代碼授予](https://oauthlib.readthedocs.io/en/latest/oauth2/grants/authcode.html)流程。

確認支援流程後，MVPD必須提供我們下列資訊：

* 驗證端點
   * 端點將提供授權代碼，稍後將用來交換重新整理和存取Token
* /token端點
   * 這將提供重新整理權杖和存取權杖
   * 重新整理權杖需要穩定（每次我們請求新的存取權杖時，它不能變更）
   * MVPD需要為每個重新整理權杖允許數個使用中的存取權杖
   * 此端點也會以重新整理權杖交換存取權杖
* 我們需要使用者設定檔&#x200B;**的**&#x200B;端點
   * 此端點會提供使用者ID，此ID必須對帳戶唯一，且不得包含任何個人識別資訊
* **/登出**&#x200B;端點（選擇性）
   * Adobe Pass驗證將重新導向至此端點，並向MVPD提供重新導向後端URI；在此端點上，MVPD可以清除使用者端電腦上的Cookie，或套用任何想要的邏輯以進行登出
* 強烈建議支援獲授權的使用者端（不會觸發使用者授權頁面的使用者端應用程式）
* 我們也需要：
   * 整合設定的&#x200B;**clientID**&#x200B;和&#x200B;**使用者端密碼**
   * 重新整理權杖和存取權杖的&#x200B;**存留時間** (TTL)值
   * 我們可以為MVPD提供授權回呼和登出回呼URI。 此外，如有需要，我們可為MVPD提供要列入防火牆設定中白名單的IP清單。


## 驗證流程 {#authn-flow}

在驗證流程中，Adobe Pass驗證會使用在設定中選取的通訊協定與MVPD通訊。 OAuth 2.0流程如下圖所示：



![在組態中選取的通訊協定上與MVPD通訊的Adobe驗證中顯示驗證流程的圖表。](../assets/authn-flow.png)

**圖1： OAuth 2.0驗證流程**



## 驗證要求與回應 {#authn-req-response}

簡而言之，支援OAuth 2.0通訊協定的MVPD的驗證流程遵循下列步驟：

1. 一般使用者會導覽至程式設計師的網站，並選取使用其MVPD憑證登入
1. 安裝在程式設計師端的AccessEnabler，會以HTTP要求的形式將驗證要求傳送至Adobe Pass驗證端點，而Adobe Pass驗證端點會重新導向至MVPD授權端點。
1. MVPD授權端點傳送授權代碼到Adobe Pass驗證端點
1. Adobe Pass驗證會使用收到的授權代碼，從MVPD的權杖端點要求重新整理權杖和存取權杖
1. 擷取使用者資訊和中繼資料的呼叫可傳送到使用者設定檔端點，以防使用者資訊未包含在權杖中
1. 驗證權杖會傳遞給一般使用者，他們現在可以成功瀏覽程式設計師網站

   >[!NOTE]
   >
   >重新整理權杖用於在目前的存取權杖無效或過期後，取得新的存取權杖。


>[!IMPORTANT]
>
>重新整理權杖在交換存取權杖時不得變更。

此限制來自不允許伺服器更新AuthNToken （對於OAuth 2.0通訊協定，也包含重新整理權杖）的使用者端流程。

典型的授權流程會執行儲存在AuthNToken中的重新整理權杖交換，以取得存取權杖，該權杖隨後會用來以首先通過驗證的使用者名稱執行授權呼叫。 如果授權伺服器(MVPD)要變更重新整理權杖並使舊權杖失效，我們將無法更新有效的AuthNToken。 因此，MVPD需要支援穩定的重新整理權杖，才能為其設定OAuth 2.0整合。


## 從SAML移轉至OAuth 2.0 {#saml-auth2-migr}

從SAML移轉至OAuth 2.0的整合作業將由Adobe和MVPD執行。 程式設計人員不需要進行任何技術變更，但程式設計人員可能會想要在MVPD登入頁面上檢查/測試品牌結合。 從MVPD的觀點來看，需要Oauth 2.0需求中要求的端點和其他資訊。

為了&#x200B;**保留SSO**，已擁有透過SAML取得的驗證權杖的使用者仍會被視為已驗證，其要求會透過舊的SAML整合進行路由。

從技術角度來看：

1. Adobe將會啟用程式設計師與MVPD之間的OAuth 2.0整合，而不會刪除SAML整合。
1. 啟用後，所有新使用者都將使用OAuth 2.0流程。
1. 已驗證的使用者，若已擁有包含SAML主體ID的本機AuthN權杖，將會由Adobe透過SAML整合自動路由。
1. 對於步驟編號3的使用者，一旦其SAML產生的AuthN權杖到期後，Adobe會將他們視為新使用者，其行為與步驟編號2的使用者類似。
1. Adobe將檢閱使用模式，以判斷可以安全停用SAML整合的時機。
