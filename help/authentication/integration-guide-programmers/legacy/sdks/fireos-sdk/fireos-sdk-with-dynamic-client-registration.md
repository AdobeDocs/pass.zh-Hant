---
title: Amazon FireOS SDK搭配Dynamic Client註冊
description: Amazon FireOS SDK搭配Dynamic Client註冊
exl-id: 27acf3f5-8b7e-4299-b0f0-33dd6782aeda
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1147'
ht-degree: 0%

---


# （舊版） Amazon FireOS SDK含Dynamic Client註冊 {#amazon-fireos-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>

## <span id=""></span>簡介 {#Intro}

已修改適用於FireTV的FireOS AccessEnabler SDK ，以啟用驗證而不使用工作階段Cookie。 由於越來越多的瀏覽器限制對Cookie的存取，因此需要另一種方法來允許驗證。

**FireOS SDK 3.0.4**&#x200B;以[動態使用者端註冊概述](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)取代目前以已簽署的請求者ID和工作階段Cookie驗證為基礎的應用程式註冊機制。


## API變更 {#API}

### Factory.getInstance

**描述：**&#x200B;將存取啟用程式物件具現化。 每個應用程式執行個體應該有單一Access Enabler執行個體。

| API呼叫：建構函式 |
| --- |
| 公用靜態AccessEnabler getInstance(Context appContext， String softwareStatement， String redirectUrl)<br>        擲回AccessEnablerException |

**可用性：** v3.0+

**引數：**

- *appContext*： Android應用程式內容
- *softwareStatement*：從TVE Dashboard取得的值，或如果字串中設定了「software\_statement」，則為&#x200B;*null*
- *redirectUrl* ：對於FireTV實作，此引數應該為Null。 此屬性上的任何設定都將被忽略。

**附註**

- 無效的softwareStatement將導致應用程式無法初始化AccessEnabler或註冊Adobe Pass驗證和授權的應用程式
- FireTV的redirectUrl引數由SDK設定為adobepass://android.app ，因為驗證是由唯一的AccessEnabler執行個體處理。

### setRequestor

**描述：**&#x200B;已建立通道的識別。 每個管道在為Adobe Pass驗證系統註冊Adobe時會獲得一個唯一的ID。 處理SSO和遠端權杖時，驗證狀態會在應用程式於背景時變更，當應用程式進入前景時，可再次呼叫setRequestor以便與系統狀態同步（如果SSO已啟用，會擷取遠端權杖，如果同時發生登出，則會刪除本機權杖）。

伺服器回應包含MVPD清單，以及附加至通道識別的一些設定資訊。 伺服器回應由Access Enabler程式碼內部使用。 只有作業的狀態（即SUCCESS/FAIL）會透過setRequestorComplete()回呼顯示給應用程式。

如果未使用&#x200B;*url*&#x200B;引數，則產生的網路呼叫會鎖定預設服務提供者URL：Adobe發行生產環境。

如果提供&#x200B;*url*&#x200B;引數的值，則產生的網路呼叫會鎖定&#x200B;*url*&#x200B;引數中提供的所有URL。 所有設定請求都在不同的執行緒中同時觸發。 第一個回應者在編譯MVPD清單時優先。 對於清單中的每個MVPD，「存取啟用程式」會記住關聯服務提供者的URL。 所有後續的軟體權利檔案請求都會導向在設定階段與目標MVPD配對之服務提供者相關聯的URL。

| API呼叫：要求者設定 |
| --- |
| public void setRequestor(String requestorId) |

**可用性：** v3.0+

| API呼叫：要求者設定 |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**可用性：** v3.0+

**引數：**

- *requestorID*：與頻道關聯的唯一識別碼。 首次向Adobe Pass驗證服務註冊時，請將Adobe指派的唯一ID傳遞至您的網站。
- *url*：選用引數；預設會使用Adobe服務提供者(http://sp.auth.adobe.com/)。 此陣列可讓您為Adobe提供的驗證和授權服務指定端點（不同的執行個體可能會用於偵錯）。 您可以使用此專案來指定多個Adobe Pass驗證服務提供者執行個體。 若這麼做，MVPD清單將由所有服務提供者的端點組成。 每個MVPD都與最快的服務提供者相關聯；也就是說，第一個回應並支援該MVPD的提供者。

已棄用：

- *signedRequestorID*：以您的私密金鑰數位簽署的請求者ID復本。<!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->。

已觸發&#x200B;**回呼：** `setRequestorComplete()`

</br>

### 登出

**描述：**&#x200B;使用此方法來起始登出流程。 登出是一系列HTTP重新導向作業的結果，因為使用者需要同時從Adobe Pass驗證伺服器和MVPD伺服器登出。 因此，此流程將開啟ChromeCustomTab視窗以執行登出。

| API呼叫：起始登出流程 |
| --- |
| public void logout() |

**可用性：** v3.0+

**引數：**&#x200B;無

已觸發&#x200B;**回呼：** `setAuthenticationStatus()`

## 程式設計師實作流程 {#Progr}

### **1. 註冊應用程式**

1. 從Adobe Pass取得software\_statement （ TVE控制面板）
1. 若要將這些值傳遞至Adobe Pass SDK，有兩個選項：
   - 在strings.xml中新增：

     ```
     <string name>"software\_statement">[softwarestatement value]</string>
     ```

   - 呼叫AccessEnabler.getInstance(appContext，softwareStatement， null)



### **2。 設定應用程式**

- a. setRequestor(requestor\_id)

  SDK將執行下列操作：

   - 註冊應用程式：使用&#x200B;**software\_statement**，SDK將取得&#x200B;**client\_id， client\_secret， client\_id\_issued\_at， redirect\_uris， grant\_types**。 此資訊會儲存在應用程式的內部儲存中。
   - 使用client\_id、client\_secret和grant\_type=&quot;client\_credentials&quot;取得&#x200B;**access\_token**。 此access\_token將用於SDK對Adobe Pass伺服器進行的每次呼叫。

| 權杖錯誤回應： |  |  |
|--- | --- | --- |
| HTTP 400 （錯誤請求） | {&quot;error&quot;： &quot;invalid\_request&quot;} | 請求缺少必要的引數、包含不受支援的引數值（授予型別除外）、重複引數、包含多個認證、使用多個機制來驗證使用者端，或格式錯誤。 |
| HTTP 400 （錯誤請求） | {&quot;error&quot;： &quot;invalid\_client&quot;} | 使用者端驗證失敗，因為使用者端不明。 SDK *必須*&#x200B;再次向授權伺服器註冊。 |
| HTTP 400 （錯誤請求） | {&quot;error&quot;： &quot;unauthorized\_client&quot;} | 已驗證的使用者端無權使用此授權授予型別。 |

- 如果MVPD需要被動驗證，WebView將會開啟以使用該MVPD被動執行，並在完成後關閉

- b. checkAuthorization()

   - *true* ：移至[授權]
   - *false* ：前往選取MVPD

- c. getAuthentication ： SDK將在呼叫引數中包含&#x200B;**access_token**

   - 記憶的mvpd ：前往setSelectedProvider(mvpd\_id)
   - 未選取mvpd ： displayProviderDialog
   - 已選取mvpd ：前往setSelectedProvider(mvpd\_id)

- d. setSelectedprovider

   - mvpd\_id驗證URL已載入ChromeCustomTabs中
   - 登入成功： delegate.setAuthenticationStatus ( SUCCESS )
   - 登入已取消：重設MVPD選擇
   - URL配置會建立為「adobepass://android.app」，以便在驗證完成時擷取

- e. get/checkAuthorization ： SDK將在標頭中包含**access\_token **作為授權：持有人&#x200B;**access\_token**

- 如果授權成功，將會呼叫以取得媒體權杖

- f.登出：

   - SDK將刪除目前請求者的有效Token （由其他應用程式取得而非透過SSO取得的驗證仍有效）
   - SDK將開啟Chrome自訂標籤以存取mvpd\_id登出端點。 完成後，Chrome自訂標籤將關閉
   - URL配置會建立為「adobepass://logout」，以擷取登出完成時的時間
   - 登出會觸發sendTrackingData(new Event(EVENT\_LOGOUT，USER\_NOT\_AUTHENTICATED\_ERROR)和回呼：setAuthenticationStatus(0，&quot;Logout&quot;)



**注意：**&#x200B;由於每個呼叫都需要&#x200B;**access_token**，以下可能的錯誤碼已在SDK中處理。

| 錯誤回應 |  |  |
|--- | --- | --- |
| invalid_request | 400 | 要求的格式錯誤。 SDK應停止執行對伺服器的呼叫。 |
| invalid_client | 403 | 不再允許使用者端ID執行要求。 sdk必須再次執行使用者端註冊。 |
| access_denied | 401 | access_token無效。 sdk必須要求新的access_token。 |
