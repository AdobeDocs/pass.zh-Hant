---
title: JavaScript SDK API參考
description: JavaScript SDK API參考
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: 4eb5fc1eb1eea4c5e27bbee298db4b9b4ba2daef
workflow-type: tm+mt
source-wordcount: '2836'
ht-degree: 0%

---

# JavaScript SDK API參考 {#javascript-sdk-api-reference}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## API參考 {#api-reference}

這些函式會起始與MVPD互動的要求。 所有呼叫為非同步；您必須實作 [回呼](#callbacks) 若要處理回應：

- [setRequestor()](#setReq)
- [getAuthorization()](#getAuthZ)
- [getAuthentication()](#getAuthN)
- [checkAuthentication()](#checkAuthN)
- [checkAuthorization()](#checkAuthZ)
- [checkPreauthorizedResources()](#checkPreauthRes)
- [getMetadata()](#getMeta)
- [setSelectedProvider()](#setSelProv)
- [logout()](#logout)


## setRequestor （inRequestorID、端點、選項）{#setrequestor(inRequestorID,endpoints,options)}

**說明：** 識別要求源自的網站。  您必須在通訊工作階段中的任何其他API呼叫之前進行此呼叫。

**引數：**

- *Inrequestorid*  — 註冊期間Adobe指派給原始網站的唯一識別碼。

- *端點*  — 此為選用引數。 可以是下列其中一個值：

   - 陣列，可讓您為Adobe提供的驗證和授權服務指定端點（不同的例項可能會用於偵錯）。 若提供多個URL，MVPD清單會由所有服務提供者的端點組成。 每個MVPD都與最快的服務提供者相關聯；也就是先回應且支援該MVPD的提供者。 根據預設（若未指定值），會使用Adobe服務提供者(<http://sp.auth.adobe.com/>)。

  範例：
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *選項*  — 包含應用程式ID值、訪客ID值重新整理較少設定（背景登出登出）和MVPD設定(iFrame)的JSON物件。 所有值均為選用。
   1. 若指定，系統會在資料庫執行的所有網路呼叫上報告Experience CloudvisitorID。 此值稍後可用於進階分析報表。
   2. 如果指定應用程式的唯一識別碼 — `applicationId`  — 此值將新增至應用程式進行的所有後續呼叫，做為X-Device-Info HTTP標頭的一部分。 此值稍後可從擷取 [ESM](/help/authentication/entitlement-service-monitoring-overview.md) 使用正確查詢的報表。

  **注意：** 所有JSON金鑰都區分大小寫。

  範例：

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- 程式設計師可以覆寫在Adobe Pass驗證中設定的MVPD設定，方法是指定是否需要iFrame才能登入(*iFrameRequired* 索引鍵)和iFrame維度(*iFrameWidth* 和 *iFrameHeight* 鍵)。 JSON物件有下列範本：

```JSON
    {  
       "visitorID": <string>,
       "backgroundLogin": <boolean>,
       "backgroundLogout": <boolean>,
       "mvpdConfig":{  
          "MVPD_ID_1":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          },
          ...
          "MVPD_ID_N":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          }
       }
    }
```


上述範本中的所有頂層索引鍵都是選用的，且具有預設值(*backgroundlogin*， *backgroundLogut*&#x200B;預設為false，而mvpdConfig為null — 表示不會覆寫MVPD設定)。


- **注意**：為上述引數指定無效值/型別將會導致未定義的行為。



以下是以下情況的設定範例：啟用不需重新整理的登入和登出、將MVPD1變更為完整頁面重新導向登入（非iFrame）並將MVPD2變更為寬度為500且高度為300的iFrame登入：

```JSON
    {  
       "backgroundLogin": true,
       "backgroundLogout": true,
       "mvpdConfig":{  
          "MVPD1":{  
             "iFrameRequired": false
          },
          "MVPD2":{  
             "iFrameRequired": true,
             "iFrameWidth": 500,
             "iFrameHeight": 300
          }
       }
    }
```


**已觸發回呼：** [setConfig()](#setconfigconfigxml-setconfigconfigxml)
</br>

[返回頁首](#top)

</br>

## getAuthorization(inResourceID， redirect_url) {#getauthorization(inresourceid,redirect_url)}

**說明：** 要求指定資源的授權。 每次客戶嘗試存取可授權資源時，請呼叫此函式，以從Access Enabler取得短期授權權杖。 資源ID會與提供授權的MVPD商定。

使用目前客戶的快取驗證Token。 如果找不到此類Token，會先起始驗證程式，然後繼續授權。

**引數：**

- `inResourceID`  — 使用者要求授權的資源識別碼。
- `redirect_url`  — 可選擇提供重新導向URL，讓MVPD的授權處理作業將使用者傳回該頁面，而非啟動授權的頁面。


**已觸發回呼：** [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) 成功時， [tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) 失敗時

>[!CAUTION]
>
>請儘量使用checkAuthorization()而非getAuthorization()。 getAuthorization()方法將會啟動完整的驗證流程（如果使用者未驗證），這可能會導致程式設計師端的複雜實作。

</b>

[返回頁首](#top)

</br>

## getAuthentication(redirect_url) {#getauthentication(redirect_url}

**說明：** 要求驗證目前客戶。 通常會呼叫以回應按一下登入按鈕。 檢查目前客戶的快取驗證Token。 如果找不到此類Token，則會起始驗證程式。 這會叫用預設或自訂提供者選擇對話方塊，然後使用選取的提供者重新導向至MVPD的登入介面。

成功時，為使用者建立和儲存驗證Token。 如果驗證失敗，提供者會傳回適當的錯誤訊息給您的 [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode) 回撥。

**引數：**

- redirect_url — 可選擇性地提供重新導向URL，讓MVPD的驗證處理作業將使用者傳回該頁面，而非啟動驗證的頁面。

**已觸發回呼：** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)， [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders)， [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[返回頁首](#top)

</br>

## checkAuthN {#checkauthn}

**說明：** 檢查目前客戶的目前驗證狀態。  未與任何UI建立關聯。

**已觸發回呼：** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

[返回頁首](#top)

</br>

## checkAuthorization(inResourceID) {#checkauthorization(inresourceid)}

**說明：** 應用程式會使用此方法來檢查目前客戶和指定資源的授權狀態。 首先檢查驗證狀態。 如果未驗證，則會觸發tokenRequestFailed()回呼，而方法會結束。 如果使用者已通過驗證，它也會觸發授權流程。 欲知詳情，請參閱 [getAuthorization()](#getAuthZ方法。

>[!TIP]
>
> **使用檢查狀態函式**  在請求授權之前，您不需要檢查驗證或授權的狀態。 例如，您可以呼叫這些函式來更新您自己的狀態顯示。 請勿在您需要任何使用者互動時使用它們。

**引數：**

- `inResourceID`  — 使用者要求授權的資源識別碼。


**已觸發回呼：**
[setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)， [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)， [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)， [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorizedResources(resources) {#checkPreauthorizedResources(resources)}

**說明：** 要求資源清單的「預檢」授權狀態。

**引數：**

- *資源*：resources引數是應檢查其授權的資源陣列。 清單中的每個元素應為代表資源ID的字串。 資源ID的限制與 `getAuthorization()` 呼叫，也就是說，這是程式設計師與MVPD或媒體RSS片段之間建立的議定值。

</br>

## checkPreauthorizedResources(resources-cache=true) {#checkPreauthorizedResources(resources-cache=true)}

此API變體從JS SDK 4.0版開始提供使用


**引數：**

- *資源*：resources引數是應檢查其授權的資源陣列。 清單中的每個元素應為代表資源ID的字串。 資源ID的限制與 `getAuthorization()` 呼叫，也就是說，這是程式設計師與MVPD或媒體RSS片段之間建立的議定值。

- *快取*：檢查預先授權的資源時，是否使用內部快取。 此為選用引數，預設為 **true**. 如果為true，則行為與上述API相同，這表示對此函式的後續呼叫將使用內部快取來解析預先授權的資源。 通過 **false** 此引數會停用內部快取，導致每次 **checkPreauthorizedResources** 已呼叫API。

**已觸發回呼：** [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[返回頁首](#top)
</br>

## getMetadata(Key) {#getMetadata}

**說明：** 擷取Access Enabler程式庫公開為中繼資料的資訊。

中繼資料有兩種型別：

- **靜態** （驗證權杖TTL、授權權杖TTL和裝置ID）
- **使用者中繼資料** （這包括在驗證和/或授權流程期間，從MVPD傳遞至使用者裝置的使用者特定資訊）

**更多資訊：** [使用者中繼資料](#UserMetadata)

**引數：**

- *key*：指定請求的中繼資料的ID：
   - 如果索引鍵為 `"TTL_AUTHN",` 然後進行查詢以取得驗證權杖到期時間。

   - 如果索引鍵為 `"TTL_AUTHZ"` 而params是一個陣列，其中包含資源id作為字串，然後進行查詢以取得與指定資源關聯的授權權杖的到期時間。

   - 如果索引鍵為 `"DEVICEID"` 接著會執行查詢以獲取目前的裝置id。 請注意，此功能預設為停用，程式設計師應聯絡Adobe以取得有關啟用和費用的資訊。

   - 如果索引鍵來自以下使用者中繼資料型別清單，則會將包含對應使用者中繼資料的JSON物件傳送至 [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata) 回呼函式：

   - `"zip"`  — 郵遞區號

   - `"encryptedZip"`  — 加密的郵遞區號

   - `"householdID"`  — 家庭識別碼。 在MVPD不支援附屬帳戶的情況下，這將與userID相同。

   - `"maxRating"`  — 使用者的父母分級上限

   - `"userID"`  — 使用者識別碼。 在MVPD支援附屬帳戶，且使用者不是主要帳戶的情況下，userID將會與householdID不同。

   - `"channelID"`  — 使用者有權檢視的管道清單

   - `"is_hoh"`  — 識別使用者是否為戶主的旗標

   - `"encryptedZip"`  — 加密的郵遞區號

   - `"typeID"`  — 識別使用者帳戶是否為主要/次要帳戶的旗標

   - `"primaryOID"`  — 家庭識別碼

   - `"postalCode"`  — 類似郵遞區號

   - `"acctID"`  — 帳戶ID

   - `"acctParentID"`  — 帳戶父級ID

  **注意**：程式設計師可用的實際使用者中繼資料取決於MVPD提供的內容。  另請參閱 [使用者中繼資料](#UserMetadata) 以取得目前可用的使用者中繼資料清單。


例如：

```JSON
    // Assume that a reference to the AccessEnabler has been previously 
    // obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
        if (status ==  1) {
            //user is authenticated, request metadata
            ae.getMetadata("zip");
            ae.getMetadata("maxRating");
        } else {
            ...
      }
    }
```


**已觸發回呼：** [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[返回頁首](#top)

</br>


## setSelectedProvider(providerid) {#setSelectedProvider}

**說明：** 當使用者從您的提供者選擇UI選取MVPD以將提供者選擇傳送至Access Enabler時呼叫此函式，或使用null引數呼叫此函式，以防使用者未選取提供者即解除您的提供者選擇UI。

**已觸發回呼：**[ setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)， [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[返回頁首](#top)

</br>

## getSelectedProvider() {#getSelectedProvider}

**說明：** 在提供者選擇對話方塊中擷取客戶選擇的結果。 這可在初始驗證檢查後隨時使用。

此函式為非同步，且其結果會傳回至 `selectedProvider()` 回呼函式。

- **MVPD** 目前選取的MVPD，若未選取MVPD，則為null。
- **AE_State** 目前客戶為「新使用者」、「使用者未驗證」或「使用者已驗證」其中之一的驗證結果

**已觸發回呼：** [selectedProvider()](#getselectedprovider-getselectedprovider)

</br>

[返回頁首](#top)

</br>

## 登出 {#logout}

**說明：** 登出目前的客戶，清除該使用者的所有驗證和授權資訊。 從客戶系統刪除所有authN和authZ權杖。

**已觸發回呼：** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
</br>

[返回頁首](#top)

</br>

## 回呼定義 {#calllback-definitions}

您必須實作這些回撥來處理非同步要求呼叫的回應：

- [entitlementLoaded()](#entitlementloaded-entitlementloaded)
- [setConfig()](#setconfigconfigxml-setconfigconfigxml)
- [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders)
- [createIFrame()](#createiframe-createiframeinwidthminheight)
- [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
- [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)
- [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)
- [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)
- [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)
- [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)
- [selectedProvider()](#selectedproviderresult-selectedprovider)

</br>

## entitlementLoaded() {#entitlementLoaded}

**說明：** 當Access Enabler完成初始化並準備好接收請求時觸發。 實作此回撥以瞭解何時可以使用Access Enabler API開始通訊。
</br>

[返回頂端](#top)

</br>

## setConfig(configXML) {#setconfig(configXML)}

**說明：** 實作此回呼以接收設定資訊和MVPD清單。

**引數：**

- *configXML*：儲存目前REQUESTOR （包括MVPD清單）之設定的xml物件。


**觸發者：** [setRequestor()](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[返回頂端](#top)

</br>

## displayProviderDialog(providers) {#displayproviderdialog(providers)}

**說明：** 實作此回呼以叫用您自己的自訂提供者選擇UI。 您的對話方塊應使用顯示名稱（和選用的標誌）來提供客戶的選擇。 當客戶做出選擇並解除對話方塊時，請在呼叫中將所選提供者的相關ID傳送至 *setSelectedProvider()*.

**引數：**

- *提供者*  — 代表所要求MVPD的物件陣列：

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**觸發者：** [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl)， [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[返回頂端](#top)

</br>

## createIFrame(inWidth， inHeight) {#createIFrame(inWidth,inHeight)}

**說明：** 如果使用者選取的MVPD需要iFrame才能顯示其驗證登入頁面UI，請實作此回呼。

**觸發者：**[ setSelectedProvider()](#setselectedproviderproviderid-setselectedprovider)

</br> [返回頂端](#top)

</br>

## setAuthenticationStatus(isAuthenticated， errorCode) {#set-authn-status-isauthn-error}

**說明：** 實作此回撥以接收驗證狀態（1=authenticated或0=not authenticated）和描述性錯誤訊息（如果在嘗試判斷驗證狀態時發生錯誤，則為空字串成功完成檢查）。

>[!NOTE]
> 
>如果您使用目前的 [進階錯誤報告](/help/authentication/error-reporting.md) 系統，您可以忽略傳送至此函式的errorCode引數。  不過，isAuthenticated旗標仍可用於追蹤軟體權利檔案流程中使用者的驗證狀態


**引數：**

- *isAuthenticated*  — 提供驗證狀態： 1 （已驗證）或0 （未驗證）。
- *errorCode*  — 判斷驗證狀態時發生錯誤。 空白字串（如果沒有）。


**觸發者：** [checkAuthentication()](#checkauthn-checkauthn)， [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl)， [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[返回頂端](#top)

</br>

## sendTrackingData(trackingEventType， trackingData) {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>裝置型別和作業系統衍生自使用公用Java程式庫(<http://java.net/projects/user-agent-utils>)和使用者代理字串。 請注意，此資訊僅以粗略的方式提供，以將運作量度劃分為裝置類別，但該Adobe對於錯誤結果概不負責。 請據以使用新功能。

**說明：** 實作此回撥以在特定事件發生時接收追蹤資料。 例如，您可以使用它來追蹤有多少使用者以相同認證登入。 目前無法設定追蹤。 使用Adobe Pass驗證1.6， `sendTrackingData()` 也會報告有關裝置、 Access Enabler使用者端和作業系統型別的資訊。 此 `sendTrackingData()` 回呼會保持回溯相容。

- 裝置型別的可能值：
   - 電腦
   - 平板電腦
   - 行動
   - gameconsole
   - 未知

- Access Enabler使用者端型別的可能值：
   - html5
   - ios
   - android


傳遞事件型別和一系列相關資訊。 事件型別為：

| mvpdSelection | 使用者在提供者選擇對話方塊中選取了MVPD。 |
| ----------------------- | --------------------------------------------------------- |
| authenticationDetection | 驗證檢查已完成。 |
| authorizationDetection | 授權要求已完成。 |

</br>
資料是每個事件型別專屬的：
</br>

| 事件型別（字串） | 資料（陣列） |
|:--- | :--- |
| mvpdSelection | 0：選取的MVPD |
|  | 1：裝置型別 |
|  | 2：存取啟用程式使用者端型別 |
|  | 3：作業系統 |
| authenticationDetection | 0：權杖要求是否成功(true/false) |
|  | 1： MVPD ID |
|  | 2： GUID |
|  | 3：權杖已位於快取中(true/false) |
|  | 4：裝置型別 |
|  | 5：存取啟用程式使用者端型別 |
|  | 6：作業系統 |
| authorizationDetection | 0：權杖要求是否成功(true/false) |
|  | 1： MVPD ID |
|  | 2： GUID |
|  | 3：權杖已位於快取中(true/false) |
|  | 4：錯誤 |
|  | 5：詳細資料 |
|  | 6：裝置型別 |
|  | 7：存取啟用程式使用者端型別 |
|  | 8：作業系統 |


**觸發者：** [checkAuthentication()](#checkauthn-checkauthn)， [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl)， [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)， [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[返回頂端](#top)

</br>

## setToken(inRequestedResourceID， inToken) {#setToken(inRequestedResourceID,inToken)}

**說明：** 實作此回撥以接收短期媒體權杖(inToken)和資源(inRequestedResourceID)的ID (inRequestedResourceID)，針對其提出授權要求或檢查授權要求且已成功完成。

**觸發者：** [checkAuthorization()](#checkAuthZ)， [getAuthorization()](#getAuthZ)
</br>

[返回頂端](#top)

</br>

## tokenRequestFailed(inRequestedResourceID， inRequestErrorCode， inRequestDetailedErrorMessage) {#token-request-failed-error-msg}

**說明：** 實作此回呼以在授權或檢查授權要求失敗時發出訊號。 可選擇由MVPD用來提供自訂訊息，由程式設計師顯示。

>[!IMPORTANT]
>
>此回呼函式屬於舊版原始Adobe Pass驗證錯誤報告系統的一部分。 保留是為了回溯相容性，但如果您已使用目前的進階錯誤報告系統實作自己的回呼，則根本不需要使用此函式。 較新的錯誤報告系統提供了授權（或其他操作）失敗原因的更多詳細資訊，以及每種錯誤或警告型別的建議動作過程。

**引數：**

- *inRequestedResourceID*  — 提供授權請求所使用的資源ID的字串。
- *inRequestErrorCode*  — 顯示Adobe Pass驗證錯誤碼的字串，指出失敗的原因；可能的值是「使用者未驗證錯誤」和「使用者未授權錯誤」；如需詳細資訊，請參閱下方的「回撥錯誤代碼」。
- *inRequestDetailedErrorMessage*  — 適用於顯示的額外描述性字串。 如果此描述性字串因任何原因而無法使用，Adobe Pass驗證會傳送空白字串 **(「」)**.  MVPD可使用此功能傳遞自訂錯誤訊息或銷售相關訊息。 例如，如果拒絕訂閱者的資源授權，MVPD可能會回覆一個 `*inRequestDetailedErrorMessage*` 例如： **「您目前無法在封裝中存取此管道。 如果您想要升級套件，請按一下\*這裡\*。」** 此訊息會由Adobe Pass驗證透過此回呼傳遞至程式設計師的網站。 然後程式設計師可以選擇顯示或忽略它。 Adobe Pass驗證也可以使用 `*inRequestDetailedErrorMessage*` 通知程式設計師可能導致錯誤的情況。 例如， **「與提供者的授權服務通訊時發生網路錯誤」。**



**觸發者：**  [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)， [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[返回頂端](#top)

</br>


## preauthorizedResources(authorizedResources) {#preauthorizedResources(authorizedResources)}

**說明：** Access Enabler觸發的回呼，會傳送在呼叫後傳回的授權資源清單 `checkPreauthorizedResources()`.

**引數：**

- *authorizedResources*：已授權資源的清單。

**觸發者：** [checkPreauthorizedResources()](#checkPreauthRes)
</br>

[返回頂端](#top)

</br>

## setMetadataStatus(key， encrypted， data) {#setMetadataStatus(key,encrypted,data)}

**說明：** 由Access Enabler觸發的回呼，透過傳遞請求的中繼資料 `getMetadata()` 呼叫。

**更多資訊：** [使用者中繼資料](#userMetadata)

**引數：**

- *索引鍵（字串）*：提出請求的中繼資料的索引鍵。
- *encrypted （布林值）*：表示「值」是否已加密的旗標。 如果這是「true」，則「value」實際上將會是JSON Web加密的實際值表示法。
- *資料（JSON物件）*：以中繼資料表示的JSON物件。若為簡單要求(&#39;`TTL_AUTHN`&#39;， &#39;`TTL_AUTHZ`&#39;， &#39;`DEVICEID`&#39;)，結果為字串（代表驗證TTL、授權TTL或裝置ID）。 在使用者中繼資料請求的情況下，結果可以是代表中繼資料裝載的基本或JSON物件。 JSON使用者中繼資料物件的實際結構類似於以下內容：

```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
            zip: ["12345", "34567"],
            maxrating: { 
                "MPAA": "PG-13",
                "VCHIP": "TV-Y", 
                "URL": "http://exam.pl/e/manage/ratings"
            },
            householdid: "3456",
            uid: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
            channelID: ["channel-1", "channel-2"]
    }
```


例如：

```JSON
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
        if (encrypted) {
            //the metadata value is encrypted
            //needs to be decrypted by the programmer
            data = decrypt(data);
        }
        alert(key + "=" + data);
    }
```


**觸發者：** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[返回頂端](#top)

</br>

## selectedProvider(result) {#selectedProvider(result)}

**說明：** 實作此回撥以接收目前選取的MVPD以及封裝在 `result` 引數。 此 `result` parameter是一個物件，具有以下屬性：

- **MVPD** 目前選取的MVPD，若未選取MVPD，則為null。
- **AE\_State** 目前使用者的驗證結果，屬於「新使用者」、「使用者未驗證」或「使用者已驗證」其中之一

**觸發者：** [getSelectedProvider()](#getSelProv)

</br>

[返回頂端](#top)

</br>

### 回呼錯誤代碼 {#callback-error-codes}

| 一般錯誤 | |
|:--- | :--- | 
| 內部錯誤 | 嘗試處理要求時發生系統錯誤。 |
| 未選取提供者錯誤 | 當客戶在提供者選擇對話方塊中取消時發生 |
| 無法使用提供者錯誤 | 當沒有提供者可用時發生。 |

| 驗證錯誤 | |
|:--- | :--- | 
| 一般驗證錯誤 | 原因不明或無法發佈時傳回。 |
| 內部驗證錯誤 | 嘗試驗證時發生錯誤。 |
| 使用者未驗證錯誤 | 使用者未驗證。 |
| 多個驗證請求錯誤 | 第一個驗證請求完成之前接收到其他驗證請求。 |

| 授權錯誤 | |
|:--- | :--- | 
| 一般授權錯誤 | 原因不明或無法發佈時傳回。 |
| 內部授權錯誤 | 嘗試授權時發生系統錯誤。 |
| 使用者未授權錯誤 | 客戶無權檢視要求的內容。 |

<!--

### Related Information {#Related-Infornation}

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
* **JavaScript Code Samples**
* [Error Reporting](/help/authentication/error-reporting.md)
* [Understanding Tokens](#understanding_tokens)
* **Tracking Data in Adobe Pass Authentication**
-->

[返回頁首](#top)
