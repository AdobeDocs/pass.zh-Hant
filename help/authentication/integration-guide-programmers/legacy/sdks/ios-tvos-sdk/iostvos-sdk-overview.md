---
title: iOS/tvOS SDK概觀
description: iOS/tvOS SDK概觀
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '3754'
ht-degree: 0%

---

# （舊版） iOS/tvOS SDK概觀 {#iostvos-sdk-overview}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

</br>


## 簡介 {#intro}

iOS AccessEnabler是Objective C iOS/tvOS資料庫，可讓行動應用程式使用Adobe Pass驗證來進行TV Everywhere的權益服務。 實作包含定義軟體權利檔案API的&#x200B;*AccessEnabler*&#x200B;介面，以及描述程式庫觸發之回撥的&#x200B;*EntitlementDelegate*&#x200B;和&#x200B;*[EntitlementStatus](#ios%20entitlement%20status)*&#x200B;通訊協定。 介面與通訊協定在同一個一般名稱下稱為：AccessEnabler程式庫。

## iOS和tvOS需求 {#reqs}

如需iOS和tvOS平台以及Adobe Pass驗證的目前技術需求，請參閱[平台/裝置/工具需求](#ios)，並參閱SDK下載內容中的發行說明。 在本頁面的其餘部分中，您會看到註解適用於特定SDK版本及更新版本的變更的區段。 例如，以下是有關1.7.5 SDK的合法附註：

## 瞭解原生使用者端工作流程 {#flows}

原生使用者端工作流程通常與瀏覽器式Adobe Pass驗證使用者端的工作流程相同或非常類似。 不過，有一些例外，如下所述。

- [初始化後工作流程](#post-init)
- [通用初始驗證工作流程](#generic)
- [登出工作流程](#logout)


### 初始化後工作流程 {#post-init}

AccessEnabler支援的所有權益工作流程都假設您先前已呼叫[`setRequestor()`](#setReq)來建立您的身分識別。 您進行此呼叫時，通常是在應用程式的初始化/設定階段，只提供一次請求者ID。


使用iOS原生使用者端時，在初次呼叫[`setRequestor()`](#setReq)後，您可以選擇如何繼續：

- 您可以立即開始發出權益呼叫，並視需要允許以靜默方式將其排入佇列。

- 您可以實作[`setRequestor()`](#setReq)回呼，以接收[`setRequestorComplete()`](#setReqComplete)成功/失敗的確認。

- 您可以同時執行上述兩項操作。

您可以選擇讓應用程式等候[`setRequestor()`](#setReq)成功通知，或依賴AccessEnabler的呼叫佇列機制。 由於所有後續的授權和驗證要求都需要要求者ID和相關聯的組態資訊，在初始化完成之前，[`setRequestor()`](#setReq)方法會有效地封鎖所有驗證和授權API呼叫。



### 通用初始驗證工作流程 {#generic}

此工作流程的用途是使用其MVPD登入使用者。 成功登入後，後端伺服器會向使用者發出驗證權杖。 雖然驗證通常是作為授權流程的一部分完成的，但以下說明如何單獨進行驗證，並且不包括任何授權步驟。

請注意，雖然此工作流程對於原生使用者端與一般瀏覽器式驗證工作流程不同，但對於原生使用者端和瀏覽器式使用者端，步驟1至5是相同的。

1. 您的應用程式會呼叫AccessEnabler的`getAuthentication() `API方法來啟動驗證工作流程，此方法會檢查有效的快取驗證Token。
1. 如果使用者目前已經驗證，AccessEnabler會呼叫您的[`setAuthenticationStatus()`](#setAuthNStatus)回呼函式，傳遞表示成功的驗證狀態，並結束流程。
1. 如果使用者目前未驗證，AccessEnabler會判斷使用者的上次驗證嘗試在指定的MVPD中是否成功，以繼續驗證流程。 如果已快取MVPD ID且`canAuthenticate`旗標為true或使用[`setSelectedProvider()`](#setSelProv)選取了MVPD，則不會使用MVPD選取對話方塊提示使用者。 驗證流程會繼續使用MVPD的快取值(即上次成功驗證期間使用的MVPD)。 系統會呼叫後端伺服器的網路，並將使用者重新導向至MVPD登入頁面（下方的步驟6）。
1. 如果未快取任何MVPD ID，而且未使用[`setSelectedProvider()`](#setSelProv)選取任何MVPD，或`canAuthenticate`標幟設定為false，則會呼叫[`displayProviderDialog()`](#dispProvDialog)回呼。 此回呼會指示您的應用程式建立UI，向使用者顯示可供選擇的MVPD清單。 提供了一系列MVPD物件，其中包含您建置MVPD選擇器所需的資訊。 每個MVPD物件都說明了一個MVPD實體，並包含MVPD的ID （例如XFINITY、AT\&amp;T等）和可以找到MVPD標誌的URL等資訊。
1. 選取特定MVPD後，您的應用程式必須通知AccessEnabler使用者所做的選擇。 一旦使用者選取想要的MVPD，您就會透過呼叫[`setSelectedProvider()`](#setSelProv)方法通知AccessEnabler使用者選取範圍。
1. iOS AccessEnabler會呼叫您的`navigateToUrl:`回呼或`navigateToUrl:useSVC:`回呼，以將使用者重新導向至MVPD登入頁面。 透過觸發其中一個，AccessEnabler會向您的應用程式發出要求，要求建立`UIWebView/WKWebView or SFSafariViewController`控制器並載入回呼`url`引數中提供的URL。 這是後端伺服器上驗證端點的URL。 對於tvOS AccessEnabler，會使用[引數呼叫](#status_callback_implementation)status()`statusDictionary`回呼，並立即開始輪詢第二個熒幕驗證。 `statusDictionary`包含需要用於第二個熒幕驗證的`registration code`。
1. 若是iOS AccessEnabler，使用者會登陸MVPD的登入頁面，透過應用程式`UIWebView/WKWebView or SFSafariViewController `控制器的媒體輸入其認證。 請注意，在此傳輸期間會發生數個重新導向作業，且您的應用程式必須監視控制器在多重重新導向作業期間載入的URL。
1. 若是iOS AccessEnabler，當`UIWebView/WKWebView or SFSafariViewController`控制器載入特定自訂URL時，您的應用程式必須關閉控制器並呼叫AccessEnabler的`handleExternalURL:url `API方法。 請注意，這個特定自訂URL實際上無效，控制器並非打算實際載入此URL。 應用程式必須將其解譯為驗證流程已完成，且關閉`UIWebView/WKWebView or SFSafariViewController`控制器是安全的訊號。 如果您的應用程式需要使用`SFSafariViewController `控制器，則特定自訂URL是由`application's custom scheme`所定義（例如： `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`），否則此特定自訂URL是由`ADOBEPASS_REDIRECT_URL`常數（即`adobepass://ios.app`）所定義。
1. 在您的應用程式關閉`UIWebView/WKWebView or SFSafariViewController`控制器並呼叫AccessEnabler的`handleExternalURL:url `API方法後，AccessEnabler就會從後端伺服器擷取驗證權杖，並通知您的應用程式驗證流程已完成。 AccessEnabler以狀態碼1呼叫[`setAuthenticationStatus()`](#setAuthNStatus)回呼，表示成功。 如果在執行這些步驟期間發生錯誤，[`setAuthenticationStatus()`](#setAuthNStatus)回呼會以狀態碼0觸發，表示驗證失敗以及對應的錯誤碼。


>[!WARNING]
>
> 在AccessEnabler將控制權交還給應用程式的步驟中（亦即顯示提供者選擇對話方塊時，或顯示UIWebView/WKWebView或SFSafariViewController時），使用者有機會取消驗證流程。 在這些情況下，您的應用程式會負責通知AccessEnabler此事件，並呼叫[`setSelectedProvider()`](#setSelProv) API方法，將null傳遞為引數。 這可讓AccessEnabler有機會清理其內部狀態並重設驗證流程。 在tvOS上，您可以使用相同的方法取消驗證輪詢。


### 登出工作流程 {#logout}

對於原生使用者端，登出的處理與上述驗證程式類似。

1. 您的應用程式會呼叫AccessEnabler的`logout() `API方法來啟動登出工作流程。 登出是一系列HTTP重新導向作業的結果，因為使用者需要同時從Adobe Pass驗證伺服器和MVPD伺服器登出。 因為此流程無法以AccessEnabler程式庫發出的簡單HTTP要求完成，所以必須將`UIWebView/WKWebView or SFSafariViewController`控制器具現化，才能遵循HTTP重新導向作業。

1. 採用類似於驗證流程的模式。 iOS AccessEnabler觸發`navigateToUrl:`回呼或`navigateToUrl:useSVC:`來建立`UIWebView/WKWebView or SFSafariViewController`控制器，並載入回呼的`url`引數中提供的URL。 這是後端伺服器上登出端點的URL。 對於tvOS AccessEnabler，不會呼叫`navigateToUrl:`回呼或`navigateToUrl:useSVC:`回呼。

1. 執行數個重新導向時，您的應用程式必須監視`UIWebView/WKWebView or SFSafariViewController `控制器的活動，並偵測載入特定自訂URL的時間。 請注意，這個特定自訂URL實際上無效，控制器並非打算實際載入此URL。 應用程式必須將其解譯為登出流程已完成，且關閉控制器是安全的訊號。 當控制器載入這個特定的自訂URL時，您的應用程式必須關閉控制器並呼叫AccessEnabler的`handleExternalURL:url `API方法。 如果您的應用程式需要使用`SFSafariViewController `控制器，則特定自訂URL是由`application's custom scheme` （例如`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`）所定義，否則此特定自訂URL是由` ADOBEPASS_REDIRECT_URL  `常數（即`adobepass://ios.app`）所定義。

1. 最後，AccessEnabler會以狀態碼0呼叫[`setAuthenticationStatus()`](#setAuthNStatus)回呼，表示登出流程成功。

登出流程與驗證流程不同，因為使用者不需要以任何方式與`UIWebView/WKWebView or SFSafariViewController`控制器互動。 因此，Adobe建議您在登出過程中隱藏控制項。

## Token {#tokens}

- [定義和使用情況](#definitions)
- [快取准則](#caching)
- [持續性](#persistence)
- [格式](#format)
- [裝置繫結](#device_binding)


### 定義和使用情況 {#definitions}

Adobe Pass驗證權利解決方案的核心是在成功完成驗證和授權工作流程後，Adobe Pass驗證產生的特定資料片段（權杖）。 這些權杖會儲存在使用者端的iOS裝置上。



權杖的生命週期有限；到期時，需要透過重新初始化驗證和/或授權工作流程重新發行權杖。



在權益工作流程期間會核發三種型別的權杖：

- **驗證Token：**&#x200B;使用者驗證工作流程的最終結果將會是AccessEnabler可用來代表使用者進行授權查詢的驗證GUID。 此驗證GUID將具有關聯的存留時間(TTL)值，該值可能與使用者的驗證工作階段本身不同。 透過將驗證GUID繫結至起始驗證請求的裝置，將產生驗證權杖。
- **授權Token：**&#x200B;授予由唯一resourceID識別的特定受保護資源的存取權。 它由授權方核發的授權授與與原始資源ID組成。 此資訊會繫結至起始請求的裝置。
- **短效媒體權杖：** AccessEnabler會傳回短效媒體權杖，授與特定資源之託管應用程式的存取權。 此Token是根據先前為該特定資源取得的授權Token而產生。 此外，此Token不會與裝置繫結，而且關聯的生命週期會大幅縮短（預設值： 5分鐘）。

在成功的驗證和授權後，Adobe Pass驗證將核發驗證、授權和短期媒體權杖。 這些權杖應在使用者裝置上快取，並用於其相關生命週期的持續時間。



### 快取准則 {#caching}

- 驗證Token
- 授權Token
- 短期媒體Token


#### 驗證Token

- **AccessEnabler 1.7：**&#x200B;此SDK推出新的權杖儲存方法，可啟用多個程式設計人員 — MVPD貯體，因此可啟用多個驗證權杖。 現在，「每位請求者的驗證」案例和一般的驗證流程都會使用相同的儲存配置。 兩者之間唯一的差異在於執行驗證的方式：「每個請求者的驗證」包含一項新的改進（被動驗證），使AccessEnabler能夠基於儲存體中的驗證權杖存在（針對不同的程式設計人員）執行後端通道驗證。 使用者只需驗證一次，此工作階段將用於取得其他應用程式中的驗證權杖。 此後端管道流程會在[`setRequestor()`](#setReq)呼叫期間發生，且對程式設計師來說大部分是透明的。 **然而，這裡有一個重要的需求：程式設計師必須從主要使用者介面執行緒呼叫setRequestor()。**
- **AccessEnabler 1.6和更舊版本：**&#x200B;驗證權杖在裝置上的快取方式取決於與目前MVPD相關聯的「**每個請求者的驗證」**&#x200B;旗標：

<!-- end list -->

1. 如果「每位請求者驗證」功能已停用，則單一驗證Token會儲存在本機全域剪貼簿中；此Token會與整合至目前MVPD的所有應用程式共用。
1. 如果「每個請求者的驗證」功能已啟用，那麼權杖會明確關聯到執行驗證流程的「程式設計師」（權杖不會儲存在全域作業範圍中，而是儲存在只能看到該程式設計師應用程式的私人檔案中）。 更具體來說，將會停用不同應用程式之間的單一登入(SSO)；使用者在切換至新應用程式時，需要明確執行驗證流程(前提是第二個應用程式的程式設計師已整合至目前的MVPD，而且本機快取中沒有該程式設計師的驗證權杖)。



#### 授權Token

在任何指定時刻，AccessEnabler只會快取每個RESOURCE的一個授權權杖。 可以快取多個授權權杖，但它們與不同資源相關聯。 每當核發新的授權權杖，且&#x200B;*相同資源*&#x200B;的舊授權權杖已存在時，新權杖就會覆寫現有的快取值。



#### 媒體權杖

不應快取短暫的媒體權杖。 每次呼叫授權API時，應該從伺服器擷取媒體權杖，因為它僅限於一次性使用。



### 持續性 {#persistence}

Token必須持續存在於相同應用程式的連續執行中。 這表示取得驗證和授權權杖且使用者關閉應用程式後，當使用者重新開啟應用程式時，應用程式可使用相同的權杖。 此外，這些代號最好在多個應用程式中持續儲存。 換言之，當使用者使用一個應用程式與特定的身分提供者登入（成功取得驗證和授權權杖）後，相同的權杖可以透過不同的應用程式使用，而且當使用者透過相同的身分提供者登入時，不再提示使用者輸入認證。 這種無縫的驗證/授權工作流程讓Adobe Pass驗證解決方案成為無所不在的真正電視節目
實作。



## iOS

iOS AccessEnabler程式庫會將權杖資料儲存在名為&#x200B;*貼上板*&#x200B;的「類似剪貼簿」資料結構中，以解決跨應用程式資料共用問題。 此系統層級共用資源提供可啟用所需永久權杖使用案例的主要元件：

- **支援結構化儲存裝置** — 貼上板不只是一種簡單的線性緩衝記憶體結構。 它提供類似字典的儲存機制，允許根據使用者指定的索引鍵值索引資料。
- **使用基礎檔案系統支援資料持續性** — 貼上板結構的內容可以標示為持續性，在這種情況下，資料會儲存在裝置的內部記憶體中。



將特定權杖放入權杖快取中後，AccessEnabler程式庫將在不同時間檢查其有效性。  有效的權杖定義為：

- 權杖的TTL尚未過期。
- 權杖的簽發者包含在允許的身分提供者清單中。



## tvOS

由於作業範圍在tvOS上無法使用，因此tvOS AccessEnabler程式庫會使用NsUserDefaults作為儲存選項。 這解決了持續驗證和授權權杖的問題，但儲存的資訊無法在不同的應用程式之間共用。



**iOS 10作業範圍變更 —**&#x200B;從舊版iOS升級時，作業範圍將會被清除。 這表示所有應用程式都需要重新驗證。



**iOS 7作業範圍變更 —**&#x200B;由於作業範圍在iOS 7上的功能有所變更，在iOS 7上執行的應用程式之間的交叉SSO將會受到限制。 具有相同`<Bundle Seed ID>` （也稱為`<Team ID>`）的應用程式將共用權杖，這表示來自相同程式設計人員X的應用程式A1和A2將共用權杖，而應用程式A1 （程式設計人員X）和應用程式A3 （程式設計人員Y）將不會共用權杖。

- 如果2個應用程式之間的套件組合種子ID/團隊ID是由相同的布建設定檔產生，則它們是相同的。 如需詳細資訊，請前往此連結：
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- 無論使用哪種Adobe Pass驗證SDK，iOS 7都會出現這種「跨SSO」限制。

請閱讀此技術說明以瞭解在iOS 7和更新版本上設定SSO的詳細資訊（此技術說明適用於Access Enabler v1.8和更新版本）： <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### 權杖儲存(AccessEnabler 1.7)

從AccessEnabler 1.7開始，權杖儲存可支援多個程式設計人員 — MVPD組合，依賴可容納多個驗證權杖的多層巢狀對應結構。 此新儲存裝置不會以任何方式影響AccessEnabler公用API，而且不需要程式設計師端進行變更。 以下是範例，
說明了這項新功能：

1. 開啟App1 （由Programmer1開發）。
1. 使用MVPD1 （與程式設計工具1整合）進行驗證。
1. 暫停/關閉目前的應用程式，然後開啟App2 （由Programmer2開發）。
1. 我們假設Programmer2未與MVPD2整合，因此「不會」在App2中驗證使用者。
1. 在App2中使用MVPD2 （已與Programmer2整合）進行驗證。
1. 切換回App1；使用者仍將透過程式設計員1進行驗證。

在舊版AccessEnabler中，步驟6會將使用者呈現為未驗證，因為權杖儲存先前僅支援一個驗證權杖。



從某個程式設計人員/MVPD工作階段登出後，將會清除整個基礎儲存空間，包括裝置上的所有其他程式設計人員/MVPD驗證權杖。 另一方面，取消驗證流程（叫用[`setSelectedProvider(null)`](#setSelProv)）將不會清除基礎儲存體，但只會影響目前的程式設計人員/MVPD驗證嘗試(透過清除目前程式設計人員的MVPD)。



### Token Importer (AccessEnabler 1.7)

AccessEnabler 1.7中包含的另一個儲存相關功能可讓您從舊儲存區域匯入驗證權杖。 此「Token Importer」有助於實現連續AccessEnabler版本之間的相容性，即使儲存版本升級時也能維持SSO狀態。 匯入工具會在[`setRequestor()`](#setReq)流程期間執行，並在以下兩個案例中執行（假設目前存放區中沒有目前程式設計師的有效驗證Token）：

- 首次安裝由特定程式設計師開發的1.7應用程式
- 升級路徑至使用新儲存裝置的未來AccessEnabler

匯入作業對程式設計師而言是透明的，不需要在使用者端應用程式中進行任何程式碼變更。



### 權杖清除程式(AccessEnabler 1.7.5)

從AccessEnabler 1.7.5開始，此服務會在[`setRequestor()`](#setReq)`. `上執行。開發此服務是因為將iOS 7從WiFi MAC位址切換至IDFA以進行追蹤。 清理程式會確保目前的儲存空間僅包含有效的驗證權杖(根據裝置ID而言有效，先前是使用MAC位址計算，在iOS7之前)。 Token清理程式會移除所有無效的Token。



在iOS 6上使用AccessEnabler 1.7.5應用程式，然後使用者更新至iOS 7時，「權杖清除器」服務最有用。 發生此情況時，在iOS 6上取得的所有驗證權杖現在都將無效(因為裝置ID演演算法從使用MAC位址變更為IDFA)。 清理程式會清除所有無效的權杖，然後使用者會進行未驗證。



如果沒有「權杖清除程式」移除無效的權杖，AccessEnabler將無法取得授權，因為存在無效的驗證權杖。 這對於一般使用者來說很難進行偵錯，因為授權錯誤訊息可能很隱秘，且在判斷導致問題的原因時不太實用。



### 格式 {#format}

- [AuthN權杖](#authn_token)
- [AuthZ權杖](#authz_token)
- [短媒體Token](#short_token)
- [裝置繫結](#device_binding)


請注意，此處包含AuthN和AuthZ權杖的格式僅供背景資訊使用。 Adobe Pass驗證可隨時變更這些權杖的結構。 程式設計師不需要知道AuthN和AuthZ權杖的確切結構即可實作其應用程式，因為AuthN和AuthZ權杖不會在本機裝置上公開。 Short Media權杖&#x200B;*是*&#x200B;公開給程式設計師的應用程式。



#### 驗證Token {#authn_token}

以下清單顯示驗證Token的格式：

```
  <signatureInfo>base64(...)<signatureInfo>
  <simpleAuthenticationToken>
      <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
      <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
      <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
      <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
      <simpleTokenMsoID>Adobe</simpleTokenMsoID>
      <simpleTokenDeviceID>
          <simpleTokenFingerprint>
              HASH(true device identification info)
          </simpleTokenFingerprint>
      </simpleTokenDeviceID>   
  </simpleAuthenticationToken>
```


#### 授權Token {#authz_token}

以下清單顯示授權權杖的格式：

```
  <signatureInfo>base64(...)<signatureInfo>
  <simpleAuthorizationToken>
      <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
      <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
      <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
      <simpleTokenMsoID>Adobe</simpleTokenMsoID>
      <simpleTokenDeviceID>
          <simpleTokenFingerprint>
              HASH(true device identification info)
          </simpleTokenFingerprint>
      </simpleTokenDeviceID>
  </simpleAuthorizationToken>
```


#### 短媒體Token {#short_token}

以下清單顯示短媒體權杖的格式。 此Token會公開給程式設計師的應用程式。 在成功的軟體權利檔案程式結束時，它會傳遞給程式設計師的應用程式：

```
  <signatureInfo>signature<signatureInfo>
  <shortAuthorizationToken>
    <sessionGUID>session_guid</sessionGUID>
    <requestorID>requestor_id</requestorID>
    <resourceID>resource_id</resourceID>
    <ttl>ttl_in_ms</ttl>
    <issueTime>issue_time</issueTime>
    <mvpdId>mvpd_id</mvpdId>
    <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
  </shortAuthorizationToken>
```


### 裝置繫結 {#device_binding}

在上述XML清單中，記下標題為`simpleTokenFingerprint`的標籤。 此標籤的用途是儲存原生裝置ID個人化資訊。 AccessEnabler程式庫可取得這類個人化資訊，並在軟體權利檔案呼叫期間提供給Adobe Pass驗證服務。 此服務將使用此資訊並將其嵌入實際權杖，因此可以將權杖有效地繫結到特定裝置。 這的最終目標是要讓權杖不可跨裝置轉讓。



由於這顯然是安全性相關功能，因此從安全性觀點來看，此資訊本身就是「敏感」的。 因此，需要保護這些資訊不被竄改和竊聽。 透過透過HTTPS通訊協定傳送驗證/授權請求，即可解決竊聽問題。 處理竄改保護的方法為以數位方式簽署裝置識別資訊。 AccessEnabler程式庫會根據裝置所提供的資訊計算裝置ID，然後將裝置ID「以清除形式」傳送至Adobe Pass驗證伺服器，作為請求引數。 Adobe Pass驗證伺服器會使用Adobe的私密金鑰以數位方式簽署裝置ID，並將其新增至傳回至AccessEnabler的驗證權杖。 因此，裝置ID會與驗證Token繫結。 在授權流程期間，AccessEnabler會再次以清除格式傳送裝置ID以及驗證權杖。 驗證程式的失敗將自動導致驗證/授權工作流程失敗。 Adobe Pass驗證伺服器會將私密金鑰套用至裝置ID，並將其與驗證權杖中的值比較。 如果兩者不符，該權益流程就會失敗。



**注意AccessEnabler 1.7.5中的裝置繫結：**&#x200B;從AccessEnabler 1.7.5開始，裝置ID的計算方式會有所變更，以提供iOS裝置的個人化資訊。 這項變更反映了iOS 7的變更：從iOS 7開始，Apple不再提供WiFi MAC位址作為追蹤選項，而改用其廣告商識別碼(IDFA)。 由於在iOS 7上執行的應用程式之個人化資訊以IDFA為基礎，而該資訊內嵌於權益流程權杖中，這表示此變更可能會對使用者體驗造成許多不同的影響。 不同的可能效果取決於使用者要升級的iOS版本，以及程式設計師要升級的AccessEnabler版本。 如需此變更的詳細資訊，請參閱AccessEnabler SDK 1.7.5隨附的發行說明。

<!--
## Related Information {#related}


- [iOS/tvOS Integration Cookbook](#)
- [iOS/tvOS API Reference](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [SSO on iOS when using the Adobe Pass Authentication Access
  Enabler](#)
-->
