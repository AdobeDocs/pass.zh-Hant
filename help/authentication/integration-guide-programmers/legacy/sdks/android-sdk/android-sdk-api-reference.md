---
title: Android SDK API參考
description: Android SDK API參考
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '4560'
ht-degree: 0%

---

# （舊版） Android SDK API參考 {#android-sdk-api-reference}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

## 簡介 {#intro}

本檔案詳細說明適用於Adobe Pass驗證的Android SDK所公開的方法和回呼，Adobe Pass驗證1.7版及更新版本均支援這些方法和回呼。 這裡說明的方法和回呼函式在AccessEnabler.h和EntitlementDelegate.h標頭檔案中定義。

如需最新的Android AccessEnabler SDK，請參閱[https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library)。


**注意：** Adobe Pass驗證團隊鼓勵您只使用Adobe Pass驗證&#x200B;*公用* API：

- 公開API可在&#x200B;*上使用，並在所有支援的使用者端型別上經過完整測試*。 對於任何公用功能，我們確定每個使用者端型別都有相關方法的對應版本。</span>
- 公用API需要儘可能穩定，以支援回溯相容性，並確保合作夥伴整合不會中斷。 不過，對於&#x200B;*非*&#x200B;公用API，我們保留在未來任何時候變更其簽章的權利。 如果您遇到無法透過目前公用Adobe Pass驗證API呼叫組合支援的特定流程，最好的方法就是讓我們知道。 根據您的需求，我們可以修改公用API，並提供未來穩定的解決方案。

## ANDROID API {#api}

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

**描述：**&#x200B;將存取啟用程式物件具現化。 每個應用程式執行個體應該有單一Access Enabler執行個體。

| API呼叫：建構函式 |
| --- |
| **公用靜態** AccessEnabler **getInstance**（內容appContext， String softwareStatement， String redirectUrl）<br>        **擲回** AccessEnablerException <br><br>**公用靜態** AccessEnabler getInstance(Context appContext， String env_url， String softwareStatement， String redirectUrl)<br>**擲回** AccessEnablerException |

**可用性：** v3.1.2+

**引數：**

- *appContext*： Android應用程式內容。
- env\_url：若要使用Adobe測試環境進行測試，env\_url可設為「sp.auth-staging.adobe.com」

**已棄用：**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**描述：**&#x200B;建立程式設計師的身分。 每個程式設計師在為Adobe Pass驗證系統註冊Adobe後都會獲得一個唯一的ID。 處理SSO和遠端權杖時，驗證狀態會在應用程式於背景時變更，當應用程式進入前景時，可再次呼叫setRequestor以便與系統狀態同步（如果SSO已啟用，會擷取遠端權杖，如果同時發生登出，則會刪除本機權杖）。

伺服器回應包含MVPD清單，以及附加至程式設計師身分的一些設定資訊。 伺服器回應由Access Enabler程式碼內部使用。 只有作業的狀態（即SUCCESS/FAIL）會透過setRequestorComplete()回呼顯示給應用程式。

如果未使用&#x200B;*url*&#x200B;引數，則產生的網路呼叫會鎖定預設服務提供者URL：Adobe發行/生產環境。

如果提供&#x200B;*url*&#x200B;引數的值，則產生的網路呼叫會鎖定&#x200B;*url*&#x200B;引數中提供的所有URL。 所有設定請求都在不同的執行緒中同時觸發。 第一個回應者在編譯MVPD清單時優先。 對於清單中的每個MVPD，「存取啟用程式」會記住關聯服務提供者的URL。 所有後續的軟體權利檔案請求都會導向在設定階段與目標MVPD配對之服務提供者相關聯的URL。

| API呼叫：要求者設定 |
| --- |
| ```public void setRequestor(String requestorId)``` |

**可用性：** v3.0+

| API呼叫：要求者設定 |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**可用性：** v3.0+


**引數：**

- *requestorID*：與程式設計師相關聯的唯一識別碼。 首次向Adobe Pass驗證服務註冊時，請將Adobe指派的唯一ID傳遞至您的網站。

- *signedRequestorID*：以您的私密金鑰數位簽署的請求者ID復本。<!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->。

- *url*：選用引數；預設會使用Adobe服務提供者(http://sp.auth.adobe.com/)。 此陣列可讓您為Adobe提供的驗證和授權服務指定端點（不同的執行個體可能會用於偵錯）。 您可以使用此專案來指定多個Adobe Pass驗證服務提供者執行個體。 若這麼做，MVPD清單將由所有服務提供者的端點組成。 每個MVPD都與最快的服務提供者相關聯；也就是說，第一個回應並支援該MVPD的提供者。

已觸發&#x200B;**回呼：** `setRequestorComplete()`

已棄用：

    public void setRequestor(String requestorId， String signedRequestorId)
    
    public void setRequestor (String requestorId， String signedRequestorId， ArrayList&lt;String> url)

[返回Android API...](#api)

### setRequestorComplete {#setRequestorComplete}

**描述：**&#x200B;由Access Enabler觸發的回呼，通知您的應用程式設定階段已完成。 這是應用程式可以開始發出權益要求的訊號。 在設定階段完成之前，應用程式無法發出任何軟體權利檔案請求。

| 回呼：要求者設定完成 |
| --- |
| java public void setRequestorComplete(int status) |

**可用性：** v1.0+

**引數：**

- *狀態*：可以使用下列其中一個值：
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` — 設定階段已順利完成
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` — 設定階段失敗
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` — 設定階段已順利完成
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` — 設定階段失敗

**觸發者：** `setRequestor()`

[返回Android API...](#api)

### setOptions {#setOptions}

**描述：**&#x200B;設定全域SDK選項。 它接受&#x200B;**Map\&lt;String， String\>**&#x200B;做為引數。 來自對應的值會連同SDK發出的每個網路呼叫一起傳遞至伺服器。

這些值將會傳遞至伺服器，不受目前流量（驗證/授權）的影響。 如果您想要變更值，可以隨時呼叫此方法。

| API呼叫： setOptions |
| --- |
| public void setOptions（HashMap&lt;String，String>選項） |

**可用性：** v1.9.2+

**引數：**

- *選項*：包含全域SDK選項的對應&lt;字串，字串>。 目前提供下列選項：
   - **applicationProfile** — 它可用來根據這個值設定伺服器組態。
   - **ap_vi** -Experience Cloud識別碼(visitorID)。 此值稍後可用於進階分析報表。
   - **ap_ai** - Advertising ID
   - **device_info** — 使用者端資訊，如下所述： [傳遞使用者端資訊裝置連線與應用程式](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)。

[回到頂端……](#apis)


### checkAuthentication {#checkAuthN}

**描述：**&#x200B;檢查驗證狀態。 其做法是在本機權杖儲存空間中搜尋有效的驗證權杖。 此方法不會執行任何網路呼叫，我們建議您在主要執行緒上呼叫它。 應用程式會使用它來查詢使用者的驗證狀態並相應地更新UI （即更新登入/登出UI）。 驗證狀態是透過&#x200B;[*setAuthenticationStatus()*](#setAuthNStatus)&#x200B;回呼傳送給應用程式。

如果MVPD支援「每位請求者驗證」功能，則多個驗證權杖可以儲存在裝置上。  如需有關此功能的詳細資訊，請參閱Android技術概觀中的[快取指引](#$caching)區段。

| API呼叫：檢查驗證狀態 |
| --- |
| public void checkAuthentication() |

**可用性：** v1.0+

**引數：**&#x200B;無

已觸發&#x200B;**回呼：** `setAuthenticationStatus()`

[返回Android API...](#api)


### getAuthentication {#getAuthN}

**描述：**&#x200B;啟動完整驗證工作流程。 首先檢查驗證狀態。 如果尚未驗證，則會啟動驗證流程狀態機器：

- 如果上次驗證嘗試成功，則會略過MVPD選取階段並觸發&#x200B;[*navigateToUrl()*](#navigagteToUrl)&#x200B;回呼。 應用程式會使用此回呼來例項化WebView控制項，該控制項會向使用者顯示MVPD的登入頁面。
- 如果上次驗證嘗試不成功，或使用者明確登出，則會觸發&#x200B;[*displayProviderDialog()*](#displayProviderDialog)&#x200B;回呼。 您的應用程式會使用此回呼來顯示MVPD選擇UI。 此外，您的應用程式也需要透過[setSelectedProvider()](#setSelectedProvider)方法通知存取啟用程式庫有關使用者的MVPD選取專案，以繼續驗證流程。

由於使用者的認證是在MVPD登入頁面上驗證的，因此您的應用程式必須監控使用者在MVPD登入頁面進行驗證時發生的多個重新導向操作。 輸入正確的認證時，WebView控制項會重新導向至由&#x200B;*AccessEnabler.ADOBEPASS\_REDIRECT\_URL*&#x200B;常數定義的自訂URL。 此URL不打算由WebView載入。 應用程式必須攔截此URL，並將此事件解譯為登入階段完成的訊號。 然後，它應該將控制項移交給存取啟用程式，以便完成驗證流程(透過呼叫&#x200B;*getAuthenticationToken()*&#x200B;方法)。

如果MVPD支援「每位請求者的驗證」功能，則多個驗證權杖可以儲存在裝置上（每位程式設計師一個）。  如需有關此功能的詳細資訊，請參閱Android技術概觀中的[快取指引](#$caching)區段。

最後，驗證狀態會透過&#x200B;*setAuthenticationStatus()*&#x200B;回呼與應用程式通訊。



| API呼叫：起始驗證流程 |
| --- |
| 公開void getAuthentication() |

**可用性：** v1.0+

| API呼叫：起始驗證流程 |
| --- |
| 公開void getAuthentication（布林值forceAuthN， Map&lt;字串， Object> genericData） |

**可用性：** v1.8+

**引數：**

- *forceAuthn*：指定是否應該啟動驗證流程的標幟，無論使用者是否已驗證。
- *資料*：包含要傳送至Pay-TV pass服務的機碼值組的對應。 Adobe可使用此資料來啟用未來的功能，而不需變更SDK。

已觸發&#x200B;**回呼：** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[返回Android API...](#api)

### displayProviderDialog {#displayProviderDialog}

**說明**&#x200B;由Access Enabler觸發的回呼，通知應用程式必須具現化適當的UI元素，才能讓使用者選取想要的MVPD。 回呼會提供MVPD物件清單，內含其他資訊，有助於正確建立選取專案UI面板(例如指向MVPD標誌的URL、好記的顯示名稱等)

使用者選取想要的MVPD後，需要上層應用程式來繼續驗證流程，方法是呼叫&#x200B;*setSelectedProvider()*&#x200B;並傳遞與使用者選取專案相對應的MVPD識別碼。

>[!NOTE]
>
> 中止驗證流程
> </br></br>
> 請注意，這是使用者能夠按下「上一步」按鈕的階段，這等同於中止驗證流程。 在這種情況下，您的應用程式必須呼叫`setSelectedProvider()`方法，傳遞&#x200B;*null*&#x200B;做為引數，讓Access Enabler有機會重設其驗證狀態機器。

| 回呼：顯示MVPD選取專案UI |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**可用性：** v1.0+

**引數**：

- *mvpds*：包含MVPD相關資訊的MVPD物件清單，應用程式可使用此清單來建置MVPD選擇UI元素。

**觸發者：** `getAuthentication(), getAuthorization()`

[返回Android API...](#api)


### setselectedprovider {#setSelectedProvider}

**描述：**&#x200B;您的應用程式呼叫此方法，以告知存取啟用程式使用者的MVPD選擇。 應用程式可以使用此方法來選取或變更用於驗證的服務提供者。

如果選取的MVPD是TempPass MVPD，它隨後將自動使用該MVPD進行驗證，而無需呼叫getAuthentication()。

請注意，這不適用於促銷暫時傳遞，因為getAuthentication()方法有額外的引數。

將&#x200B;*null*&#x200B;作為引數傳遞時，Access Enabler會假設使用者已取消驗證流程（亦即按下[上一步]按鈕），並藉由重設驗證狀態機器以及呼叫帶有`AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR`錯誤碼的&#x200B;*setAuthenticationStatus()*&#x200B;回呼來回應。

| API呼叫：設定目前選取的提供者 |
| --- |
| public void setSelectedProvider(String mvpdId) |

**可用性：** v1.0+

**引數：**&#x200B;無

已觸發&#x200B;**回呼：** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[返回Android API...](#api)


### navigateToUrl {#navigagteToUrl}

**已棄用：**&#x200B;從Android SDK 3.0開始，只有在裝置上不存在Chrome自訂標籤時，才會使用navigateToUrl

**說明：**&#x200B;存取啟用程式所觸發的回呼，會通知應用程式使用者必須顯示MVPD登入頁面才能輸入其認證。 Access Enabler會以引數形式傳遞MVPD登入頁面的URL。 您的應用程式必須將WebView控制項例項化，並將其導向此URL。 此外，應用程式必須監視WebView控制項載入的URL，並攔截以`AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)`常數定義的自訂URL為目標的重新導向作業。 發生此事件時，應用程式必須關閉或隱藏WebView控制項，並呼叫&#x200B;*getAuthenticationToken()*&#x200B;方法，將控制項交還給Access Enabler程式庫。 Access Enabler會從後端伺服器擷取驗證Token，並將其儲存在本機的權杖儲存體中，藉此完成驗證流程。

>[!WARNING]
>
> **中止驗證流程** <br>請注意，這是使用者能夠按下[上一步]按鈕的時刻，這相當於中止驗證流程。 在這種情況下，您的應用程式必須呼叫&#x200B;_setSelectedProvider()_&#x200B;方法，將&#x200B;_null_&#x200B;傳遞為引數，並讓Access Enabler有機會重設其驗證狀態機器。

| 回呼：顯示MVPD登入頁面 |
| --- |
| 公開void navigateToUrl（字串url） |

**可用性：** v1.0+

**引數：**

- *url*：指向MVPD登入頁面的URL

**觸發者：** `getAuthentication(), setSelectedProvider()`

[返回Android API...](#api)


### getAuthenticationToken {#getAuthNToken}

**已棄用：**&#x200B;從Android SDK 3.0開始，由於Chrome自訂標籤用於驗證，因此應用程式不再使用此方法。

**描述：**&#x200B;從後端伺服器要求驗證Token，以完成驗證流程。 應用程式只應呼叫這個方法，以回應託管MVPD登入頁面的WebView控制項被重新導向至由`AccessEnabler.ADOBEPASS_REDIRECT_URL`常數定義的自訂URL的事件。

| API呼叫：擷取驗證Token |
| --- |
| 公開void getAuthenticationToken() |

**可用性：** v1.0+

**引數：**

- *Cookie*：在目標網域上設定的Cookie (如需參考實作，請參閱SDK中的示範應用程式)。

已觸發&#x200B;**回呼：** `setAuthenticationStatus()`，`sendTrackingData()`

[返回Android API...](#api)


### setAuthenticationStatus {#setAuthNStatus}

**描述：**通知存取啟用程式所觸發的回呼
驗證流程狀態的應用。 有許多
此流程可能因為使用者的
互動或因其他無法預見的情況(例如網路
連線問題等)。 此回呼會通知應用程式：
驗證流程的成功/失敗狀態，同時也是
視需要提供有關失敗原因的其他資訊。

| 回呼：報告驗證流程的狀態 |
| --- |
| public void setAuthenticationStatus(int status， String errorCode) |

**可用性：** v1.0+

**引數：**

- *狀態*：可以使用下列其中一個值：
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` — 驗證流程已成功完成
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` — 驗證流程失敗
- *代碼*：失敗原因。 如果&#x200B;*狀態*&#x200B;為`AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`，則&#x200B;*代碼*&#x200B;為空字串（亦即，由`AccessEnablerConstants.USER_AUTHENTICATED`常數定義）。 如果失敗，此引數可以採用下列其中一個值：
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` — 使用者未驗證。 當本機權杖快取中沒有有效的驗證權杖時，回應&#x200B;*checkAuthentication()*&#x200B;方法呼叫。
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` — 在上層應用程式將&#x200B;*null*&#x200B;傳遞到`setSelectedProvider()`之後，AccessEnabler已重設驗證狀態電腦，以中止驗證流程。  使用者可能已取消驗證流程（亦即按下「上一步」按鈕）。
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR` — 由於網路無法使用或使用者明確取消驗證流程等原因，驗證流程失敗。

**觸發者：** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[返回Android API...](#api)


### checkPreauthorizedResources {#checkPreauth}

>**已棄用：**&#x200B;從Android SDK 3.6開始，預先授權API正在取代checkPreauthorizedResources，提供延伸錯誤碼。

**描述：**&#x200B;應用程式使用此方法來判斷使用者是否已獲授權檢視特定的受保護資源。 此方法的主要用途是擷取用於裝飾UI的資訊（例如，使用鎖定和解鎖圖示指示存取狀態）。

| API呼叫：設定目前選取的提供者 |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**可用性：** v1.3+

**引數：** `resources`引數是應檢查其授權的資源陣列。 清單中的每個元素應為代表資源ID的字串。 資源ID受到與`getAuthorization()`呼叫中的資源ID相同的限制，也就是說，它應該是程式設計師與MVPD或媒體RSS片段之間建立的議定值。

已觸發&#x200B;**回呼：** `preauthorizedResources()`

[返回Android API...](#api)


### checkPreauthorizedResources {#checkPreauth2}

**已棄用：**&#x200B;從Android SDK 3.6開始，預先授權API正在取代checkPreauthorizedResources，提供延伸錯誤碼。

**描述：**&#x200B;應用程式使用此方法來判斷使用者是否已獲授權檢視特定的受保護資源。 此方法的主要用途是擷取用於裝飾UI的資訊（例如，使用鎖定和解鎖圖示指示存取狀態）。

| API呼叫：設定目前選取的提供者 |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**可用性：** v3.1+

**引數：** `resources`引數是應檢查其授權的資源陣列。 清單中的每個元素應為代表資源ID的字串。 資源ID受到與`getAuthorization()`呼叫中的資源ID相同的限制，也就是說，它應該是程式設計師與MVPD或媒體RSS片段之間建立的議定值。

`cache`引數指定是否可以使用快取的預先授權回應。 根據預設，快取為true，SDK會傳回先前快取的回應（若有）。

已觸發&#x200B;**回呼：** `preauthorizedResources()`

[返回Android API...](#api)

### preauthorizedResources {#preauthResources}

**已棄用：**&#x200B;從Android SDK 3.6開始，預先授權API正在取代checkPreauthorizedResources，提供延伸錯誤碼。 將不會在新API上呼叫preauthorizedResources回呼。


**描述：**&#x200B;由checkPreauthorizedResources()觸發的回呼。 提供使用者已獲授權可檢視的資源清單。

| API呼叫：設定目前選取的提供者 |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**可用性：** v1.3+

**引數：** `resources`引數是使用者已被授權檢視的資源陣列。

**觸發者：** `checkPreauthorizedResources()`

[返回Android API...](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**描述：**&#x200B;應用程式使用此方法來檢查授權狀態。 首先檢查驗證狀態。 如果未驗證，則會觸發&#x200B;*setTokenRequestFailed()*&#x200B;回呼，而且方法會結束。 如果使用者已通過驗證，它也會觸發授權流程。 檢視&#x200B;*getAuthorization()*&#x200B;方法的詳細資料。

| API呼叫：檢查授權狀態 |
| --- |
| public void checkAuthorization(String resourceId) |

**可用性：** v1.0+

| API呼叫：檢查授權狀態 |
| --- |
| public void checkAuthorization(String resourceId， Map&lt;String， Object> genericData) |

**可用性：** v1.8+

**引數：**

- *resourceId*：使用者要求授權的資源識別碼。
- *資料*：包含要傳送至Pay-TV pass服務的機碼值組的對應。 Adobe可使用此資料來啟用未來的功能，而不需變更SDK。

已觸發&#x200B;**回呼：** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[返回Android API...](#api)


### <span id="getAuthZ"></span>getAuthorization

**描述：**&#x200B;應用程式使用此方法來起始授權流程。 如果使用者尚未驗證，它也會起始驗證流程。 如果使用者已驗證，存取啟用程式會繼續發出授權權杖（如果本機權杖快取中不存在有效的授權權杖）和短期媒體權杖的請求。 取得簡短媒體權杖後，授權流程即視為完成。 會觸發&#x200B;*setToken()*&#x200B;回呼，並將短媒體權杖作為引數傳遞至應用程式。 如果授權因任何原因而失敗，則會觸發&#x200B;*tokenRequestFailed()*&#x200B;回呼，並提供錯誤碼和詳細資料。

| API呼叫：起始授權流程 |
| --- |
| public void getAuthorization(String resourceId) |

**可用性：** v1.0+

| API呼叫：起始授權流程 |
| --- |
| 公開void getAuthorization(String resourceId， Map&lt;String， Object> genericData) |

**可用性：** v1.8+

**引數：**

- *resourceId*：使用者要求授權的資源識別碼。
- *資料*：包含要傳送至Pay-TV pass服務的機碼值組的對應。 Adobe可使用此資料來啟用未來的功能，而不需變更SDK。

已觸發&#x200B;**回呼：** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **觸發了其他回呼** <br>此方法也可以觸發下列回呼（如果同時啟動驗證流程）： *setAuthenticationStatus()*，*displayProviderDialog()*，*navigateToUrl()*

**注意：請儘量使用checkAuthorization()而非getAuthorization()。 getAuthorization()方法將啟動完整的驗證流程（如果使用者未驗證），這可能會導致程式設計師端的複雜實作。**

[返回Android API...](#api)


### setToken {#setToken}

**描述：**&#x200B;由Access Enabler觸發的回呼，通知您的應用程式授權流程已順利完成。 短期媒體代號也會作為引數傳遞。

| 回呼：授權流程已成功完成 |
| --- |
| 公用void setToken(String token， String resourceId) |

**可用性：** v1.0+

**引數：**

- *權杖*：短期的媒體權杖
- *resourceId*：已取得授權的資源

**觸發者：** `checkAuthorization()`，`getAuthorization()`


[返回Android API...](#api)

### tokenRequestFailed {#tokenRequestFailed}

**描述：**&#x200B;由Access Enabler觸發的回呼，通知上層應用程式授權流程失敗。

| 回呼：授權流程失敗 |
| --- |
| 公開void tokenRequestFailed(String resourceId， <br>        字串errorCode， String errorDescription) |

**可用性：** v1.0+

**引數：**

- *resourceId*：已取得授權的資源
- *errorCode*：與失敗案例關聯的錯誤碼。 可能的值：
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR` — 使用者無法授權指定的資源
- *errorDescription*：有關失敗案例的其他詳細資料。 如果此描述性字串因任何原因而無法使用，Adobe Pass驗證會傳送空白字串&#x200B;**(&quot;)**。

  MVPD可使用此字串來傳遞自訂錯誤訊息或銷售相關訊息。 例如，如果訂閱者拒絕對資源的授權，MVPD可以傳送訊息，例如：「您目前沒有封裝中此頻道的存取權。 如果您想要升級您的套件，請按這裡。」 此訊息會由Adobe Pass驗證透過此回呼傳送給程式設計師，程式設計師可以選擇顯示或忽略此訊息。 Adobe Pass驗證也可以使用此引數來提供可能導致錯誤的狀況通知。 例如，「與提供者的授權服務通訊時發生網路錯誤。」

**觸發者：** `checkAuthorization(), getAuthorization()`

[返回Android API...](#api)

### 登出 {#logout}

**描述：**&#x200B;使用此方法來起始登出流程。 登出是一系列HTTP重新導向作業的結果，因為使用者需要同時從Adobe Pass驗證伺服器和MVPD伺服器登出。 因此，此流程無法使用Access Enabler程式庫發出的簡單HTTP要求完成。 SDK會使用Chrome自訂標籤來執行HTTP重新導向操作。 此流程將對使用者可見，並在完成後關閉

| API呼叫：起始登出流程 |
| --- |
| public void logout() |

**可用性：** v1.0+

**引數：**&#x200B;無

已觸發&#x200B;**回呼：**

- 適用於SDK 3.0之前版本的`navigateToUrl()`
- 適用於SDK 3.0版的`setAuthenticationStatus()`


[返回Android API...](#api)


### getselectedprovider {#getSelectedProvider}

**描述：**&#x200B;使用此方法來判斷目前選取的提供者。

| API呼叫：決定目前選取的MVPD |
| --- |
| 公開void getSelectedProvider() |

**可用性：** v1.0+

**引數：**&#x200B;無

已觸發&#x200B;**回呼：** `selectedProvider()`

[返回Android API...](#api)


### <span id="selectedProvider"></span>selectedProvider

**描述：**&#x200B;由Access Enabler觸發的回呼，此回呼會將目前所選MVPD的相關資訊傳送給應用程式。

| 回呼：目前所選MVPD的相關資訊 |
| --- |
| public void selectedProvider(Mvpd mvpd) |


**可用性：** v1.0+

**引數：**

- *mvpd*：包含目前所選MVPD相關資訊的物件

**觸發者：** `getSelectedProvider()`

[返回Android API...](#api)


### getMetadata {#getMetadata}

**描述：**&#x200B;使用此方法來擷取Access Enabler程式庫公開為中繼資料的資訊。 應用程式可提供複合MetadataKey物件，以存取此資訊。

| API呼叫：查詢AccessEnabler的中繼資料 |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**可用性：** v1.0+

程式設計師可以使用兩種中繼資料型別：

- 靜態中繼資料（驗證權杖TTL、授權權杖TTL和裝置ID）
- 使用者中繼資料(使用者特定資訊，例如使用者ID和郵遞區號；在驗證和/或授權流程中從MVPD傳遞至使用者的裝置)

**引數：**

- *metadataKey*：封裝索引鍵和args變數的資料結構，其含義如下：
   - 如果索引鍵是`METADATA_KEY_USER_META`，而且引數包含名稱= `METADATA_ARG_USER_META`且值= `[metadata_name]`的SerializableNameValuePair物件，則會針對使用者中繼資料進行查詢。 目前可用的使用者中繼資料型別清單：
      - `zip` — 郵遞區號

      - `householdID` — 家庭識別碼。 如果MVPD不支援附屬帳戶，這與`userID`相同。

      - `maxRating` — 使用者的家長評等上限

      - `userID` — 使用者識別碼。 如果MVPD支援附屬帳戶，且使用者不是主要帳戶，則`userID`將與`householdID`不同。

      - `channelID` — 使用者有權檢視的管道清單
   - 如果索引鍵是`METADATA_KEY_DEVICE_ID`，則會進行查詢以取得目前的裝置識別碼。 請注意，此功能預設為停用，程式設計師應聯絡Adobe以取得有關啟用和費用的資訊。
   - 如果索引鍵是`METADATA_KEY_TTL_AUTHZ`，而且引數包含名稱= `METADATA_ARG_RESOURCE_ID`且值= `[resource_id]`的SerializableNameValuePair物件，則會進行查詢以取得與指定資源關聯的授權權杖的到期時間。
   - 如果金鑰為`METADATA_KEY_TTL_AUTHN`，則會進行查詢以取得驗證權杖到期時間。



>[!NOTE]
>
>若為SDK 3.4.0，可從com.adobe.adobepass.accessenabler.api.profile.UserProfileService取得常數： `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN`。



>[!NOTE]
>
>程式設計師實際可用的使用者中繼資料取決於MVPD提供的功能。  此清單將進一步展開，因為新的中繼資料已推出並新增至Adobe Pass驗證系統。

已觸發&#x200B;**回呼：** [`setMetadataStatus()`](#setMetadaStatus)

**更多資訊：** [使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

[返回Android API...](#api)

### setmetadataStatus {#setMetadaStatus}

**描述：**&#x200B;由存取啟用程式觸發的回呼，此啟用程式會傳遞透過&#x200B;*getMetadata()*&#x200B;呼叫要求的中繼資料。

| 回撥：中繼資料擷取請求的結果 |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**可用性：** v1.0+

**引數：**

- *key*：包含要求中繼資料值的索引鍵和相關引數的MetadataKey物件（如需參考實作，請參閱示範應用程式）。
- *result*：包含所要求中繼資料的複合物件。 物件包含下列欄位：
   - *simpleResult*：字串，代表驗證TTL、授權TTL或裝置ID要求時的中繼資料值。 如果針對使用者中繼資料提出要求，則此值為Null。

   - *userMetadataResult*：包含JSON使用者中繼資料承載之Java表示法的物件。\
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

**使用者中繼資料物件的實際結構類似下列：**

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

- *encrypted*：布林值，指定擷取的中繼資料是否加密。 此引數僅對使用者中繼資料請求而言很重要，對始終未加密接收的靜態中繼資料（例如：驗證TTL）而言沒有意義。 如果此引數設為True，則由程式設計師決定是否使用白名單私密金鑰（與在[`setRequestor`](#setRequestor)呼叫中簽署要求者ID所用的相同私密金鑰）執行RSA解密，以取得未加密的使用者中繼資料值。

**觸發者：** [`getMetadata()`](#getMetadata)

**更多資訊：** [使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)


[返回Android API...](#api)


### getVersion {#getVersion}

**描述：**&#x200B;此方法可用來擷取AccessEnabler程式庫的版本。

| API呼叫：取得AccessEnabler版本 |
| --- |
| ```public static String getVersion()``` |


[返回Android API...](#api)

</br>

## 追蹤事件 {#tracking}

Access Enabler會觸發其他回呼，而此回呼不一定與權益流程相關。 實作名為&#x200B;*sendTrackingData()*&#x200B;的事件追蹤回呼函式是選擇性的，但可讓應用程式追蹤特定事件並編譯統計資料，例如成功/失敗的驗證/授權嘗試次數。 以下是&#x200B;*sendTrackingData()*&#x200B;回呼的規格：


### sendTrackingData {#sendTrackingData}

**描述：**&#x200B;由Access Enabler向應用程式發出各種事件（例如驗證/授權流程的完成/失敗）訊號所觸發的回呼。 sendTrackingData()也會報告裝置型別、Access Enabler使用者端型別和作業系統。

>[!WARNING]
>
> 裝置型別和作業系統衍生自使用公用Java程式庫([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils))和使用者代理程式字串。 請注意，此資訊僅以粗略的方式提供，以將運作量度劃分為裝置類別，但該Adobe對於錯誤結果概不負責。 請據以使用新功能。


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

- *event*：正在追蹤的事件。 追蹤事件型別共有三種：
   - **authorizationDetection：**&#x200B;當授權權杖要求傳回時（事件型別為`EVENT_AUTHZ_DETECTION`）
   - **authenticationDetection：**&#x200B;任何時候發生驗證檢查時（事件型別為`EVENT_AUTHN_DETECTION`）
   - **mvpdSelection：**&#x200B;當使用者在MVPD選擇表單中選取MVPD （事件型別為`EVENT_MVPD_SELECTION`）時
- *資料*：與報告事件相關的其他資料。 此資料會以值清單的形式呈現。

下列是解譯&#x200B;*資料*中值的指示
陣列：

- 事件型別&#x200B;*`EVENT_AUTHN_DETECTION`：*
   - **0** — 權杖要求是否成功(true/false)，如果以上為true：
   - **1** - MVPD ID字串
   - **2** - GUID （md5雜湊）
   - **3** — 權杖已在快取中(true/false)
   - **4** — 裝置型別
   - **5** — 存取啟用程式使用者端型別
   - **6** — 作業系統型別

- 針對事件型別`EVENT_AUTHZ_DETECTION`
   - **0** — 權杖要求是否成功(true/false)，如果成功：
   - **1** - MVPD ID
   - **2** - GUID （md5雜湊）
   - **3** — 權杖已在快取中(true/false)
   - **4** — 錯誤
   - **5** — 詳細資料
   - **6** — 裝置型別
   - **7** — 存取啟用程式使用者端型別
   - **8** — 作業系統型別

- 針對事件型別`EVENT_MVPD_SELECTION`
   - **0** — 目前所選MVPD的識別碼
   - **1** — 裝置型別
   - **2** — 存取啟用程式使用者端型別
   - **3** — 作業系統型別

**觸發者：** `checkAuthentication()`，`getAuthentication()`，`checkAuthorization()`，`getAuthorization()`，`setSelectedProvider()`

[返回Android API...](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
