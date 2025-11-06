---
title: Android SDK與動態使用者端註冊
description: Android SDK與動態使用者端註冊
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1301'
ht-degree: 0%

---

# （舊版）透過動態使用者端註冊的Android SDK {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

## 簡介 {#Intro}

適用於Android的Android AccessEnabler SDK已修改為在不使用工作階段Cookie的情況下啟用驗證。 由於越來越多的瀏覽器限制對Cookie的存取，因此需要使用其他方法來允許驗證。

針對Android，使用Chrome自訂標籤會限制從其他應用程式存取Cookie。

>**Android SDK 3.0.0**&#x200B;簡介：

- 動態使用者端註冊會根據已簽署的請求者ID和工作階段Cookie驗證，取代目前的應用程式註冊機制
- 驗證流程的Chrome自訂標籤

>[!NOTE]
>
>對於沒有Chrome的較舊Android版本，自訂標籤支援將使用WebView驗證，就像在較舊AccessEnabler SDK版本中一樣。


## 動態使用者端註冊 {#DCR}

Android SDK v3.0+將使用[動態使用者端註冊概述](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)中定義的動態使用者端註冊程式。


## 功能示範 {#Demo}

請觀看[此網路研討會](https://my.adobeconnect.com/pzkp8ujrigg1/)，其中提供更多功能內容，並包含如何使用TVE儀表板管理軟體陳述式以及如何使用Adobe提供的示範應用程式測試產生的陳述式的示範，該示範應用程式是Android SDK的一部分。

## API變更 {#API}


### Factory.getInstance

**描述：**&#x200B;將存取啟用程式物件具現化。 每個應用程式執行個體應該有單一Access Enabler執行個體。

| API呼叫：建構函式 |
| --- |
| 公用靜態AccessEnabler getInstance(Context appContext， String softwareStatement， String redirectUrl)<br>        擲回AccessEnablerException |


**可用性：** v3.0+

**引數：**

- *appContext*： Android應用程式內容
- softwareStatement：從TVE Dashboard取得的值，或如果字串中設定了「software\_statement」，則為&#x200B;*null*
- redirectUrl ：唯一的url，在TVE儀表板中明確新增的網域之一，或是如果「redirect\_uri」設定在strings.xml中，則為&#x200B;*null*

注意：無效的softwareStatement或redirectUrl將導致應用程式無法初始化AccessEnabler或註冊Adobe Pass驗證和授權的應用程式
</br>
注意： strings.xml中的redirectUrl引數或redirect\_uri應為應用程式在TVE儀表板中以相反順序新增的網域值(例如：若為TVE儀表板中新增的網域「adobe.com」，redirectUrl應為「com.adobe」。


### setRequestor

**描述：**&#x200B;已建立通道的識別。 每個管道在為Adobe Pass驗證系統註冊Adobe時都會獲得一個唯一的ID。 處理SSO和遠端權杖時，驗證狀態會在應用程式於背景時變更，當應用程式進入前景時，可再次呼叫setRequestor以便與系統狀態同步（如果SSO已啟用，會擷取遠端權杖，如果同時發生登出，則會刪除本機權杖）。

伺服器回應包含MVPD清單，以及附加至通道識別的一些設定資訊。 伺服器回應由Access Enabler程式碼內部使用。 只有作業的狀態（即SUCCESS/FAIL）會透過setRequestorComplete()回呼顯示給應用程式。

如果未使用&#x200B;*url*&#x200B;引數，則產生的網路呼叫會鎖定預設服務提供者URL： Adobe Release/Production環境。

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

- *requestorID*：與頻道關聯的唯一識別碼。 首次向Adobe Pass驗證服務註冊時，請將Adobe指派的唯一ID傳遞至您的網站。
- *url*：選用引數；預設會使用Adobe服務提供者[http://sp.auth.adobe.com/](http://sp.auth.adobe.com/)。 此陣列可讓您為Adobe提供的驗證和授權服務指定端點（不同的例項可能會用於偵錯）。 您可以使用此專案來指定多個Adobe Pass驗證服務提供者執行個體。 若這麼做，MVPD清單將由所有服務提供者的端點組成。 每個MVPD都與最快的服務提供者相關聯；也就是說，第一個回應並支援該MVPD的提供者。

已棄用：

- *signedRequestorID*：以您的私密金鑰數位簽署的請求者ID復本。<!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->。

已觸發&#x200B;**回呼：** `setRequestorComplete()`

### 登出

**描述：**&#x200B;使用此方法來起始登出流程。 登出是一系列HTTP重新導向作業的結果，因為使用者需要同時從Adobe Pass驗證伺服器和MVPD伺服器登出。 因此，此流程將開啟ChromeCustomTab視窗以執行登出。

| API呼叫：起始登出流程 |
| --- |
| public void logout() |

**可用性：** v3.0+

**引數：**&#x200B;無

已觸發&#x200B;**回呼：** `setAuthenticationStatus()`
</br></br>

## 程式設計師實作流程 {#Progr}

### **1. 註冊應用程式**

a.從Adobe Pass （ TVE控制面板）取得software\_statement和redirect\_uri

b.有兩個選項可將這些值傳遞至Adobe Pass SDK：

在strings.xml中新增：

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

呼叫AccessEnabler.getInstance(appContext，softwareStatement，
redirectUrl)


### 2.設定應用程式

a. setRequestor(requestor\_id)

SDK將執行下列操作：

- 註冊應用程式：使用&#x200B;**software\_statement**，SDK將取得&#x200B;**client\_id， client\_secret， client\_id\_issued\_at， redirect\_uris， grant\_types**。 此資訊會儲存在應用程式的內部儲存中。

- 使用client\_id、client\_secret和grant\_type=&quot;client\_credentials&quot;取得&#x200B;**access\_token**。 此access\_token將用於SDK對Adobe Pass伺服器進行的每次呼叫

**權杖錯誤回應：**

| 錯誤回應 | | |
| --- | --- | --- |
| HTTP 400 （錯誤請求） | {&quot;error&quot;： &quot;invalid\_request&quot;} | 請求缺少必要的引數、包含不受支援的引數值（授予型別除外）、重複引數、包含多個認證、使用多個機制來驗證使用者端，或格式錯誤。 |
| HTTP 400 （錯誤請求） | {&quot;error&quot;： &quot;invalid\_client&quot;} | 使用者端驗證失敗，因為使用者端不明。 SDK必須再次向授權伺服器註冊。 |
| HTTP 400 （錯誤請求） | {&quot;error&quot;： &quot;unauthorized\_client&quot;} | 已驗證的使用者端無權使用此授權授予型別。 |

- 如果MVPD需要被動驗證，Chrome自訂標籤將會開啟，以使用該MVPD被動執行，並在完成後關閉

b. checkAuthorization()

- true ：前往授權
- false ：前往選取MVPD

c. getAuthentication ： SDK將在呼叫引數中包含&#x200B;**access_token**

- 記憶的mvpd ：前往setSelectedProvider(mvpd_id)
- 未選取mvpd ： displayProviderDialog
- 已選取mvpd ：前往setSelectedProvider(mvpd_id)

d. setSelectedprovider

- mvpd\_id驗證URL已載入ChromeCustomTabs中
- 登入成功： delegate.setAuthenticationStatus ( SUCCESS )
- 登入已取消：重設MVPD選擇
- URL配置會建立為「adobepass://redirect_uri」，以便在驗證完成時擷取

e. get/checkAuthorization ： SDK將在標頭中包含&#x200B;**access_token**，作為授權：持有人&#x200B;**access_token**

- 如果授權成功，將會進行呼叫以取得
媒體權杖

f.登出：

- SDK將刪除目前請求者的有效Token （由其他應用程式取得而非透過SSO取得的驗證仍有效）
- SDK將開啟Chrome自訂標籤以存取mvpd_id登出端點。 完成後，Chrome自訂標籤將關閉
- URL配置會建立為「adobepass://logout」，以擷取登出完成時的時間
- 登出會觸發sendTrackingData(new Event(EVENT_LOGOUT，USER_NOT_AUTHENTICATED_ERROR)和回呼：setAuthenticationStatus(0，&quot;Logout&quot;)

**注意：**&#x200B;因為每個呼叫都需要&#x200B;**access_token，**&#x200B;以下可能的錯誤碼已在SDK中處理。


| 錯誤回應 | | |
| --- | ---|--- |
| invalid_request | 400 | 要求的格式錯誤。 SDK應停止執行對伺服器的呼叫。 |
| invalid_client | 403 | 不再允許使用者端ID執行要求。 sdk必須再次執行使用者端註冊。 |
| access_denied | 401 | access\_token無效。 sdk必須要求新的access_token。 |
