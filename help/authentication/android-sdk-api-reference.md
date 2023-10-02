---
title: Android SDK API參考
description: Android SDK API參考
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: 854698397d9d14c1bfddcc10eecc61c7e3c32b71
workflow-type: tm+mt
source-wordcount: '4526'
ht-degree: 0%

---

# Android SDK API參考 {#android-sdk-api-reference}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 簡介 {#intro}

本檔案詳細說明適用於Adobe Pass驗證的Android SDK所公開的方法和回呼，Adobe Pass驗證1.7版及更新版本均支援這些方法和回呼。 這裡說明的方法和回呼函式在AccessEnabler.h和EntitlementDelegate.h標頭檔案中定義。

請參閱 [https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) 以取得最新的Android AccessEnabler SDK。


**注意：** Adobe Pass驗證團隊鼓勵您僅使用Adobe Pass驗證 *公共* API：

- 有可用的公用API *經過全面測試* 所有支援的使用者端型別。 對於任何公用功能，我們確定每個使用者端型別都有相關方法的對應版本。</span>
- 公用API需要儘可能穩定，以支援回溯相容性，並確保合作夥伴整合不會中斷。 但是，對於 *非*-public API，我們保留在未來任何時候變更其簽名的權利。 如果您遇到無法透過目前公用Adobe Pass驗證API呼叫組合支援的特定流程，最好的方法就是讓我們知道。 根據您的需求，我們可以修改公用API，並提供未來穩定的解決方案。

## Android API {#api}

- [getInstance](#getInstance)
- [setOptions](#setOptions)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setselectedprovider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- 預先授權
- [checkAuthorization](#checkAuthZ)
- [getAuthorization](#getAuthZ)
- [setToken](#setToken)
- [tokenRequestFailed](#tokenRequestFailed)
- [登出](#logout)
- [getselectedprovider](#getSelectedProvider)
- [selectedprovider](#selectedProvider)
- [getMetadata](#getMetadata)
- [setmetadataStatus](#setMetadaStatus)
- [getVersion](#getVersion)

### Factory.getInstance {#getInstance}

**說明：** 例項化Access Enabler物件。 每個應用程式執行個體應該有單一Access Enabler執行個體。

| API呼叫：建構函式 |
| --- |
| **公用靜態** AccessEnabler **getInstance**（內容appContext、字串softwareStatement、字串redirectUrl）<br>        **擲回** AccessEnablerException <br><br>**公用靜態** AccessEnabler getInstance(Context appContext， String env_url， String softwareStatement， String redirectUrl)<br>**擲回** AccessEnablerException |

**可用性：** v3.1.2+

**引數：**

- *appContext*：Android應用程式內容。
- env\_url：若要使用Adobe測試環境進行測試，env\_url可設為「sp.auth-staging.adobe.com」

**已棄用：**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**說明：** 建立程式設計師的身分。 每個程式設計師在為Adobe Pass驗證系統註冊Adobe後都會獲得一個唯一的ID。 處理SSO和遠端權杖時，驗證狀態會在應用程式於背景時變更，當應用程式進入前景時，可再次呼叫setRequestor以便與系統狀態同步（如果SSO已啟用，會擷取遠端權杖，如果同時發生登出，則會刪除本機權杖）。

伺服器回應包含MVPD清單，以及附加至程式設計師身分的一些設定資訊。 伺服器回應由Access Enabler程式碼內部使用。 只有作業的狀態（即SUCCESS/FAIL）會透過setRequestorComplete()回呼顯示給應用程式。

如果 *url* 不會使用引數，因此產生的網路呼叫會鎖定預設服務提供者URL：Adobe版本/生產環境。

若提供的值用於 *url* 引數，則所產生的網路呼叫會鎖定 *url* 引數。 所有設定請求都在不同的執行緒中同時觸發。 第一個回應者在編譯MVPD清單時優先。 對於清單中的每個MVPD，「存取啟用程式」會記住關聯服務提供者的URL。 所有後續的軟體權利檔案要求都會導向在設定階段與目標MVPD配對之服務提供者相關聯的URL。

| API呼叫：要求者設定 |
| --- |
| ```public void setRequestor(String requestorId)``` |

**可用性：** v3.0+

| API呼叫：要求者設定 |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |
**可用性：** v3.0+


**引數：**

- *要求者ID*：與程式設計師相關聯的唯一ID。 首次向Adobe Pass驗證服務註冊時，請將Adobe指派的唯一ID傳遞至您的網站。

- *signedRequestorID*：以數位方式簽署您的私密金鑰的請求者ID復本。 <!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

- *url*：選用引數；預設會使用Adobe服務提供者(http://sp.auth.adobe.com/)。 此陣列可讓您為Adobe提供的驗證和授權服務指定端點（不同的執行個體可能會用於偵錯）。 您可以使用此專案來指定多個Adobe Pass驗證服務提供者執行個體。 這樣做時，MVPD清單會由來自所有服務提供者的端點組成。 每個MVPD都與最快的服務提供者相關聯；也就是先回應且支援該MVPD的提供者。

**已觸發回呼：** `setRequestorComplete()`

已棄用：

    public void setRequestor(String requestorId， String signedRequestorId)
    
    public void setRequestor (String requestorId， String signedRequestorId， ArrayList&lt;string> url)

[返回Android API...](#api)

### setRequestorComplete {#setRequestorComplete}

**說明：** 由Access Enabler觸發的回呼，會通知您的應用程式設定階段已完成。 這是應用程式可以開始發出權益要求的訊號。 在設定階段完成之前，應用程式無法發出任何軟體權利檔案請求。

| 回呼：要求者設定完成 |
| --- |
| java public void setRequestorComplete(int status) |

**可用性：** v1.0+

**引數：**

- *狀態*：可以取下列其中一個值：
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`  — 設定階段已成功完成
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR`  — 設定階段失敗
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS`  — 設定階段已成功完成
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR`  — 設定階段失敗

**觸發者：** `setRequestor()`

[返回Android API...](#api)

### setOptions {#setOptions}

**說明：** 設定全域SDK選項。 它接受 **對應\&lt;string string=&quot;&quot;>** 作為引數。 來自對應的值會連同SDK發出的每個網路呼叫一併傳遞至伺服器。

這些值將會傳遞至伺服器，不受目前流量（驗證/授權）的影響。 如果您想要變更值，可以隨時呼叫此方法。

| API呼叫： setOptions |
| --- |
| public void setOptions(HashMap&lt;string string=&quot;&quot;> options) |

**可用性：** v1.9.2+

**引數：**

- *選項*：地圖&lt;string string=&quot;&quot;> 包含全域SDK選項。 目前提供下列選項：
   - **applicationProfile**  — 它可用來根據此值進行伺服器設定。
   - **ap_vi** -Experience CloudID (visitorID)。 此值稍後可用於進階分析報表。
   - **ap_ai**  — 廣告ID
   - **device_info**  — 使用者端資訊，如下所述： [傳遞使用者端資訊裝置連線和應用程式](/help/authentication/passing-client-information-device-connection-and-application.md).

[回到頂端……](#apis)


### checkAuthentication {#checkAuthN}

**說明：** 檢查驗證狀態。 其做法是在本機權杖儲存空間中搜尋有效的驗證權杖。 此方法不會執行任何網路呼叫，我們建議您在主要執行緒上呼叫它。 應用程式會使用它來查詢使用者的驗證狀態並相應地更新UI （即更新登入/登出UI）。 驗證狀態會透過 [*setAuthenticationStatus()*](#setAuthNStatus) 回撥。

如果MVPD支援「每個請求者的驗證」功能，則多個驗證權杖可以儲存在裝置上。  如需有關此功能的詳細資訊，請參閱 [快取准則](#$caching) 部分（在Android技術概覽中）。

| API呼叫：檢查驗證狀態 |
| --- |
| public void checkAuthentication() |

**可用性：** v1.0+

**引數：** 無

**已觸發回呼：** `setAuthenticationStatus()`

[返回Android API...](#api)


### getAuthentication {#getAuthN}

**說明：** 啟動完整驗證工作流程。 首先檢查驗證狀態。 如果尚未驗證，則會啟動驗證流程狀態機器：

- 如果上次驗證嘗試成功，則會略過MVPD選取階段，而且 [*navigateToUrl()*](#navigagteToUrl) 會觸發回呼。 應用程式會使用此回呼來例項化WebView控制項，該控制項會以MVPD的登入頁面顯示使用者。
- 如果上次驗證嘗試不成功，或使用者明確登出，則 [*displayProviderDialog()*](#displayProviderDialog) 會觸發回呼。 您的應用程式會使用此回呼來顯示MVPD選取UI。 此外，您的應用程式也需要透過將使用者的MVPD選擇通知給Access Enabler程式庫，以繼續驗證流程。 [setSelectedProvider()](#setSelectedProvider) 方法。

由於在MVPD登入頁面上驗證使用者的認證，您的應用程式必須監督使用者在MVPD登入頁面上驗證時發生的多個重新導向操作。 輸入正確的認證時，WebView控制項會重新導向至由定義的自訂URL。 *AccessEnabler.ADOBEPASS\_REDIRECT\_URL* 常數。 此URL不打算由WebView載入。 應用程式必須攔截此URL，並將此事件解譯為登入階段完成的訊號。 然後，它應該將控制權移交給Access Enabler，以完成驗證流程(透過呼叫 *getAuthenticationToken()* 方法)。

如果MVPD支援「每個請求者的驗證」功能，則多個驗證權杖可以儲存在裝置上（每個程式設計師一個）。  如需有關此功能的詳細資訊，請參閱 [快取准則](#$caching) 部分（在Android技術概覽中）。

最後，驗證狀態會透過 *setAuthenticationStatus()* 回撥。



| API呼叫：起始驗證流程 |
| --- |
| 公開void getAuthentication() |

**可用性：** v1.0+

| API呼叫：起始驗證流程 |
| --- |
| public void getAuthentication(boolean forceAuthN， Map&lt;string object=&quot;&quot;> genericData) |

**可用性：** v1.8+

**引數：**

- *forceAuthn*：指定是否應啟動驗證流程的標幟，無論使用者是否已驗證。
- *資料*：包含要傳送至Pay-TV Pass服務的機碼值組的對應。 Adobe可使用此資料來啟用未來的功能，而不需變更SDK。

**已觸發回呼：** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[返回Android API...](#api)

### displayProviderDialog {#displayProviderDialog}

**說明** 由Access Enabler觸發的回呼，用於通知應用程式需要例項化適當的UI元素，以允許使用者選取想要的MVPD。 回呼會提供MVPD物件清單，內含其他資訊，可協助您正確建立選取專案UI面板（例如指向MVPD標誌的URL、好記的顯示名稱等）

一旦使用者選取了想要的MVPD，上層應用程式就需呼叫，以恢複驗證流程 *setSelectedProvider()* ，並向其傳遞與使用者選取專案相對應的MVPD ID。

>[!NOTE]
>
> 中止驗證流程
> </br></br>
> 請注意，這是使用者能夠按下「上一步」按鈕的階段，這等同於中止驗證流程。 在這種情況下，您的應用程式必須呼叫 `setSelectedProvider()` 方法，傳遞 *null* 做為引數，讓Access Enabler有機會重設其驗證狀態機器。

| 回呼：顯示MVPD選取專案UI |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**可用性：** v1.0+

**引數**：

- *mvpds*：包含MVPD相關資訊的MVPD物件清單，應用程式可將其用來建置MVPD選取UI元素。

**觸發者：** `getAuthentication(), getAuthorization()`

[返回Android API...](#api)


### setselectedprovider {#setSelectedProvider}

**說明：** 您的應用程式會呼叫此方法，將使用者的MVPD選取專案通知存取啟用程式。 應用程式可以使用此方法來選取或變更用於驗證的服務提供者。

如果選取的MVPD是TempPass MVPD，它會自動透過該MVPD進行驗證，之後不需要呼叫getAuthentication()。

請注意，這不適用於促銷暫時傳遞，因為getAuthentication()方法有額外的引數。

傳遞時 *null* Access Enabler以引數形式假設使用者已取消驗證流程（亦即按下「上一步」按鈕），並透過重設驗證狀態機器及呼叫 *setAuthenticationStatus()* 回呼與 `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` 錯誤碼。

| API呼叫：設定目前選取的提供者 |
| --- |
| public void setSelectedProvider(String mvpdId) |

**可用性：** v1.0+

**引數：** 無

**已觸發回呼：** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[返回Android API...](#api)


### navigateToUrl {#navigagteToUrl}

**已棄用：** 從Android SDK 3.0開始，只有在裝置上沒有Chrome自訂分頁的情況下，才會使用navigateToUrl

**說明：** 「存取啟用程式」所觸發的回呼，會通知應用程式使用者必須顯示MVPD登入頁面才能輸入其認證。 Access Enabler會以引數形式傳遞MVPD登入頁面的URL。 您的應用程式必須將WebView控制項例項化，並將其導向此URL。 此外，應用程式必須監控WebView控制項載入的URL，並攔截以定義的自訂URL為目標的重新導向操作。 `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)` 常數。 發生此事件時，應用程式必須關閉或隱藏WebView控制項，並透過呼叫 *getAuthenticationToken()* 方法。 Access Enabler會從後端伺服器擷取驗證Token，並將其儲存在本機的權杖儲存體中，藉此完成驗證流程。

>[!WARNING]
>
> **中止驗證流程**  <br>請注意，這是使用者能夠按下「上一步」按鈕的階段，這等同於中止驗證流程。 在這種情況下，您的應用程式必須呼叫 _setSelectedProvider()_ 方法傳遞 _null_ 做為引數，並讓Access Enabler有機會重設其驗證狀態機器。

| 回呼：顯示MVPD登入頁面 |
| --- |
| 公開void navigateToUrl（字串url） |

**可用性：** v1.0+

**引數：**

- *url*：指向MVPD登入頁面的URL

**觸發者：** `getAuthentication(), setSelectedProvider()`

[返回Android API...](#api)


### getAuthenticationToken {#getAuthNToken}

**已棄用：** 從Android SDK 3.0版開始，由於Chrome自訂標籤用於驗證，因此應用程式將不再使用此方法。

**說明：** 從後端伺服器要求驗證權杖，以完成驗證流程。 只有在WebView控制項裝載MVPD登入頁面時，應用程式才會呼叫此方法，以回應以下動作所定義的自訂URL： `AccessEnabler.ADOBEPASS_REDIRECT_URL` 常數。

| API呼叫：擷取驗證Token |
| --- |
| 公開void getAuthenticationToken() |

**可用性：** v1.0+

**引數：**

- *Cookie*：在目標網域上設定的Cookie （請參閱SDK中的示範應用程式以取得參考實作）。

**已觸發回呼：** `setAuthenticationStatus()`， `sendTrackingData()`

[返回Android API...](#api)


### setAuthenticationStatus {#setAuthNStatus}

**說明：** 由Access Enabler觸發的回呼，通知應用程式驗證流程的狀態。 在許多地方，由於使用者的互動或其他無法預料的情況（例如網路連線問題等），此流程可能會失敗。 此回呼會通知應用程式驗證流程的成功/失敗狀態，同時會在需要時提供有關失敗原因的其他資訊。

| 回呼：報告驗證流程的狀態 |
| --- |
| public void setAuthenticationStatus(int status， String errorCode) |

**可用性：** v1.0+

**引數：**

- *狀態*：可以取下列其中一個值：
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`  — 驗證流程已成功完成
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR`  — 驗證流程失敗
- *程式碼*：失敗原因。 如果 *狀態* 是 `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`，然後 *程式碼* 是空字串(即由 `AccessEnablerConstants.USER_AUTHENTICATED` 常數)。 如果失敗，此引數可以採用下列其中一個值：
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR`  — 使用者未驗證。 回應 *checkAuthentication()* 當本機Token快取中沒有有效的驗證Token時，方法呼叫。
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - AccessEnabler在上層應用程式通過後，已重設驗證狀態機器 *null* 至 `setSelectedProvider()` 以中止驗證流程。  使用者可能已取消驗證流程（亦即按下「上一步」按鈕）。
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR`  — 由於網路無法使用或使用者明確取消驗證流程等原因，驗證流程失敗。

**觸發者：** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[返回Android API...](#api)


### checkPreauthorizedResources {#checkPreauth}

>**已棄用：** 從Android SDK 3.6開始，預先授權API正在取代checkPreauthorizedResources，提供擴充的錯誤碼。

**說明：** 應用程式會使用此方法來判斷使用者是否已獲授權檢視特定的受保護資源。 此方法的主要用途是擷取用於裝飾UI的資訊（例如，使用鎖定和解鎖圖示指示存取狀態）。

| API呼叫：設定目前選取的提供者 |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**可用性：** v1.3+

**引數：** 此 `resources` parameter是應檢查其授權的資源陣列。 清單中的每個元素應為代表資源ID的字串。 資源ID的限制與 `getAuthorization()` 呼叫，也就是說，這應該是程式設計師與MVPD或媒體RSS片段之間建立的議定值。

**已觸發回呼：** `preauthorizedResources()`

[返回Android API...](#api)


### checkPreauthorizedResources {#checkPreauth2}

**已棄用：** 從Android SDK 3.6開始，預先授權API正在取代checkPreauthorizedResources，提供擴充的錯誤碼。

**說明：** 應用程式會使用此方法來判斷使用者是否已獲授權檢視特定的受保護資源。 此方法的主要用途是擷取用於裝飾UI的資訊（例如，使用鎖定和解鎖圖示指示存取狀態）。

| API呼叫：設定目前選取的提供者 |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**可用性：** v3.1+

**引數：** 此 `resources` parameter是應檢查其授權的資源陣列。 清單中的每個元素應為代表資源ID的字串。 資源ID的限制與 `getAuthorization()` 呼叫，也就是說，這應該是程式設計師與MVPD或媒體RSS片段之間建立的議定值。

此 `cache` 引數會指定是否可使用快取的預先授權回應。 根據預設，快取為true，SDK會傳回先前快取的回應（若有）。

**已觸發回呼：** `preauthorizedResources()`

[返回Android API...](#api)

### preauthorizedResources {#preauthResources}

**已棄用：** 從Android SDK 3.6開始，預先授權API正在取代checkPreauthorizedResources，提供擴充的錯誤碼。 將不會在新API上呼叫preauthorizedResources回呼。


**說明：** checkPreauthorizedResources()觸發的回呼。 提供使用者已獲授權可檢視的資源清單。

| API呼叫：設定目前選取的提供者 |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**可用性：** v1.3+

**引數：** 此 `resources` parameter是使用者已獲授權可檢視的資源陣列。

**觸發者：** `checkPreauthorizedResources()`

[返回Android API...](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**說明：** 應用程式使用此方法來檢查授權狀態。 首先檢查驗證狀態。 如果未驗證， *setTokenRequestFailed()* 會觸發回呼，而方法會結束。 如果使用者已通過驗證，它也會觸發授權流程。 欲知詳情，請參閱 *getAuthorization()* 方法。

| API呼叫：檢查授權狀態 |
| --- |
| public void checkAuthorization(String resourceId) |

**可用性：** v1.0+

| API呼叫：檢查授權狀態 |
| --- |
| public void checkAuthorization(String resourceId， Map&lt;string object=&quot;&quot;> genericData) |

**可用性：** v1.8+

**引數：**

- *resourceId*：使用者要求授權的資源ID。
- *資料*：包含要傳送至Pay-TV Pass服務的機碼值組的對應。 Adobe可使用此資料來啟用未來的功能，而不需變更SDK。

**已觸發回呼：** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[返回Android API...](#api)


### <span id="getAuthZ"></span>getAuthorization

**說明：** 應用程式會使用此方法來起始授權流程。 如果使用者尚未驗證，它也會起始驗證流程。 如果使用者已驗證，存取啟用程式會繼續發出授權權杖（如果本機權杖快取中不存在有效的授權權杖）和短期媒體權杖的請求。 取得簡短媒體權杖後，授權流程即視為完成。 此 *setToken()* 系統會觸發回呼，並將簡短媒體權杖作為引數傳遞至應用程式。 如果由於任何原因，授權失敗， *tokenRequestFailed()* 系統會觸發回呼，並提供錯誤代碼和詳細資訊。

| API呼叫：起始授權流程 |
| --- |
| public void getAuthorization(String resourceId) |

**可用性：** v1.0+

| API呼叫：起始授權流程 |
| --- |
| 公開void getAuthorization(String resourceId， Map&lt;string object=&quot;&quot;> genericData) |

**可用性：** v1.8+

**引數：**

- *resourceId*：使用者要求授權的資源ID。
- *資料*：包含要傳送至Pay-TV Pass服務的機碼值組的對應。 Adobe可使用此資料來啟用未來的功能，而不需變更SDK。

**已觸發回呼：** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **已觸發其他回呼**  <br> 此方法也可以觸發下列回呼（如果同時啟動驗證流程）： *setAuthenticationStatus()*， *displayProviderDialog()*， *navigateToUrl()*

**注意：請儘量使用checkAuthorization()而非getAuthorization()。 getAuthorization()方法將會啟動完整的驗證流程（如果使用者未驗證），這可能會導致程式設計師端的複雜實作。**

[返回Android API...](#api)


### setToken {#setToken}

**說明：** 由Access Enabler觸發的回呼，通知您的應用程式授權流程已成功完成。 短期媒體代號也會作為引數傳遞。

| 回呼：授權流程已成功完成 |
| --- |
| 公用void setToken(String token， String resourceId) |

**可用性：** v1.0+

**引數：**

- *token*：短期媒體權杖
- *resourceId*：已獲得授權的資源

**觸發者：** `checkAuthorization()`， `getAuthorization()`


[返回Android API...](#api)

### tokenRequestFailed {#tokenRequestFailed}

**說明：** 由Access Enabler觸發的回呼，通知上層應用程式授權流程失敗。

| 回呼：授權流程失敗 |
| --- |
| 公用void tokenRequestFailed(String resourceId， <br>        字串errorCode， String errorDescription) |

**可用性：** v1.0+

**引數：**

- *resourceId*：已獲得授權的資源
- *errorCode*：與失敗案例相關的錯誤代碼。 可能的值：
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR`  — 使用者無法授權特定資源
- *errorDescription*：有關失敗情況的其他詳細資料。 如果此描述性字串因任何原因而無法使用，Adobe Pass驗證會傳送空白字串 **(「」)**.

  MVPD可使用此字串來傳遞自訂錯誤訊息或銷售相關訊息。 例如，如果拒絕訂閱者的資源授權，MVPD會傳送訊息，例如：「您目前沒有封裝中此通道的存取權。 如果您想要升級您的套件，請按這裡。」 此訊息會由Adobe Pass驗證透過此回呼傳送給程式設計師，程式設計師可以選擇顯示或忽略此訊息。 Adobe Pass驗證也可以使用此引數來提供可能導致錯誤的狀況通知。 例如，「與提供者的授權服務通訊時發生網路錯誤。」

**觸發者：** `checkAuthorization(), getAuthorization()`

[返回Android API...](#api)

### 登出 {#logout}

**說明：** 使用此方法來起始登出流程。 登出是一系列HTTP重新導向作業的結果，因為使用者需要同時從Adobe Pass驗證伺服器和MVPD伺服器登出。 因此，此流程無法使用Access Enabler程式庫發出的簡單HTTP要求完成。 SDK會使用Chrome自訂分頁來執行HTTP重新導向操作。 此流程將對使用者可見，並在完成後關閉

| API呼叫：起始登出流程 |
| --- |
| public void logout() |

**可用性：** v1.0+

**引數：** 無

**已觸發回呼：**

- `navigateToUrl()` 適用於3.0之前的SDK版本
- `setAuthenticationStatus()` 適用於SDK版本> 3.0


[返回Android API...](#api)


### getselectedprovider {#getSelectedProvider}

**說明：** 使用此方法來判斷目前選取的提供者。

| API呼叫：決定目前選取的MVPD |
| --- |
| 公開void getSelectedProvider() |

**可用性：** v1.0+

**引數：** 無

**已觸發回呼：** `selectedProvider()`

[返回Android API...](#api)


### <span id="selectedProvider"></span>selectedprovider

**說明：** Access Enabler所觸發的回呼，將目前所選MVPD的相關資訊傳送給應用程式。

| 回呼：目前所選MVPD的相關資訊 |
| --- |
| public void selectedProvider(Mvpd mvpd) |


**可用性：** v1.0+

**引數：**

- *mvpd*：包含目前所選MVPD相關資訊的物件

**觸發者：** `getSelectedProvider()`

[返回Android API...](#api)


### getMetadata {#getMetadata}

**說明：** 使用此方法來擷取Access Enabler程式庫公開為中繼資料的資訊。 應用程式可提供複合MetadataKey物件，以存取此資訊。

| API呼叫：查詢AccessEnabler的中繼資料 |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**可用性：** v1.0+

程式設計師可以使用兩種中繼資料型別：

- 靜態中繼資料（驗證權杖TTL、授權權杖TTL和裝置ID）
- 使用者中繼資料（使用者特定資訊，例如使用者ID和郵遞區號；在驗證和/或授權流程中從MVPD傳遞至使用者裝置）

**引數：**

- *metadataKey*：封裝索引鍵和args變數的資料結構，其含義如下：
   - 如果索引鍵為 `METADATA_KEY_USER_META` 和引數包含名稱為=的SerializableNameValuePair物件 `METADATA_ARG_USER_META` 而值= `[metadata_name]`，則會針對使用者中繼資料進行查詢。 目前可用的使用者中繼資料型別清單：
      - `zip`  — 郵遞區號

      - `householdID`  — 家庭識別碼。 如果MVPD不支援附屬帳戶，這將會與 `userID`.

      - `maxRating`  — 使用者的父母分級上限

      - `userID`  — 使用者識別碼。 如果MVPD支援附屬帳戶，且使用者不是主帳戶， `userID` 將不同於 `householdID`.

      - `channelID`  — 使用者有權檢視的管道清單
   - 如果索引鍵為 `METADATA_KEY_DEVICE_ID` 接著會執行查詢以獲取目前的裝置id。 請注意，此功能預設為停用，程式設計師應聯絡Adobe以取得有關啟用和費用的資訊。
   - 如果索引鍵為 `METADATA_KEY_TTL_AUTHZ` 和引數包含名稱為=的SerializableNameValuePair物件 `METADATA_ARG_RESOURCE_ID` 而值= `[resource_id]`，則會進行查詢以取得與指定資源關聯的授權權杖的到期時間。
   - 如果索引鍵為 `METADATA_KEY_TTL_AUTHN` 然後進行查詢以取得驗證權杖到期時間。



>[!NOTE]
>
>對於SDK 3.4.0，常數為： `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` 可從com.adobe.adobepass.accessenabler.api.profile.UserProfileService取得。



>[!NOTE]
>
>程式設計師實際可用的使用者中繼資料取決於MVPD提供的內容。  此清單將進一步展開，因為新的中繼資料已推出並新增至Adobe Pass驗證系統。

**已觸發回呼：** [`setMetadataStatus()`](#setMetadaStatus)

**更多資訊：** [使用者中繼資料](/help/authentication/user-metadata-feature.md)

[返回Android API...](#api)

### setmetadataStatus {#setMetadaStatus}

**說明：** 由Access Enabler觸發的回呼，透過傳遞請求的中繼資料 *getMetadata()* 呼叫。

| 回撥：中繼資料擷取請求的結果 |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**可用性：** v1.0+

**引數：**

- *key*：此MetadataKey物件包含要求中繼資料值的索引鍵和相關聯的引數（如需參考實作，請參閱示範應用程式）。
- *結果*：包含請求中繼資料的複合物件。 物件包含下列欄位：
   - *simpleResult*：代表要求驗證TTL、授權TTL或裝置ID時中繼資料值的字串。 如果針對使用者中繼資料提出要求，則此值為Null。

   - *userMetadataResult*：包含JSON使用者中繼資料裝載Java表示法的物件。\
     例如：

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
```

會轉譯為Java，如下所示：

```java
          Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
```

**使用者中繼資料物件的實際結構類似於以下內容：**

```json
          {
              updated: 1334243471,
              encrypted: ["encryptedProp"],
              data: {
                  zip: ["12345", "34567"],
                  maxRating: { 
                      "MPAA": "PG-13",
                      "VCHIP": "TV-Y", 
                      "URL": "http://exam.pl/e/manage/ratings"
                  },
                  householdID: "3456",
                  userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
                  channelID: ["channel-1", "channel-2"]
              }
          }
```

請求簡單中繼資料（驗證TTL、授權TTL或裝置ID）時，此值為空。

- *encrypted*：指定是否加密擷取的中繼資料的布林值。 此引數僅對使用者中繼資料請求而言很重要，對始終未加密接收的靜態中繼資料（例如：驗證TTL）而言沒有意義。 如果此引數設為True，則由程式設計師決定是否使用白名單私密金鑰（與在中簽署請求者ID所用的相同私密金鑰）執行RSA解密，以取得未加密的使用者中繼資料值。 [`setRequestor`](#setRequestor) 呼叫)。

**觸發者：** [`getMetadata()`](#getMetadata)

**更多資訊：** [使用者中繼資料](/help/authentication/user-metadata-feature.md)


[返回Android API...](#api)


### getVersion {#getVersion}

**說明：** 此方法可用來擷取AccessEnabler程式庫的版本。

| API呼叫：取得AccessEnabler版本 |
| --- |
| ```public static String getVersion()``` |


[返回Android API...](#api)

</br>

## 追蹤事件 {#tracking}

Access Enabler會觸發其他回呼，而此回呼不一定與權益流程相關。 實作事件追蹤回呼函式命名為 *sendTrackingData()* 是選用專案，但此專案可讓應用程式追蹤特定事件並編譯統計資料，例如成功/失敗的驗證/授權嘗試次數。 以下是 *sendTrackingData()* 回呼：


### sendTrackingData {#sendTrackingData}

**說明：** Access Enabler向應用程式發出各種事件（例如驗證/授權流程完成/失敗）的訊號所觸發的回呼。 sendTrackingData()也會報告裝置型別、Access Enabler使用者端型別和作業系統。

>[!WARNING]
>
> 裝置型別和作業系統衍生自使用公用Java程式庫([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils))和使用者代理字串。 請注意，此資訊僅以粗略的方式提供，以將運作量度劃分為裝置類別，但該Adobe對於錯誤結果概不負責。 請據以使用新功能。


- 裝置型別的可能值：
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`


- Access Enabler使用者端型別的可能值：
   - `flash`
   - `html5`
   - `ios`
   - `android`

</br>

| 回呼：追蹤事件 |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**可用性：** v1.0+

**引數：**

- *事件*：正在追蹤的事件。 追蹤事件型別共有三種：
   - **authorizationDetection：** 每當授權權杖請求傳回(事件型別為 `EVENT_AUTHZ_DETECTION`)
   - **authenticationdetection：** 當驗證檢查發生時(事件型別為 `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection：** 當使用者在MVPD選取表單中選取MVPD時(事件型別為 `EVENT_MVPD_SELECTION`)
- *資料*：與已報告事件相關聯的其他資料。 此資料會以值清單的形式呈現。

以下為解譯 *資料*
陣列：

- 針對事件型別 *`EVENT_AUTHN_DETECTION`：*
   - **0**  — 權杖要求是否成功(true/false)，如果以上為true：
   - **1** - MVPD ID字串
   - **2** - GUID （md5雜湊）
   - **3**  — 權杖已位於快取中(true/false)
   - **4**  — 裝置型別
   - **5**  — 存取啟用程式使用者端型別
   - **6**  — 作業系統型別

- 針對事件型別 `EVENT_AUTHZ_DETECTION`
   - **0**  — 權杖要求是否成功(true/false)，如果成功：
   - **1** - MVPD ID
   - **2** - GUID （md5雜湊）
   - **3**  — 權杖已位於快取中(true/false)
   - **4**  — 錯誤
   - **5**  — 詳細資料
   - **6**  — 裝置型別
   - **7**  — 存取啟用程式使用者端型別
   - **8**  — 作業系統型別

- 針對事件型別 `EVENT_MVPD_SELECTION`
   - **0**  — 目前所選MVPD的識別碼
   - **1**  — 裝置型別
   - **2**  — 存取啟用程式使用者端型別
   - **3**  — 作業系統型別

**觸發者：** `checkAuthentication()`， `getAuthentication()`， `checkAuthorization()`， `getAuthorization()`， `setSelectedProvider()`

[返回Android API...](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
