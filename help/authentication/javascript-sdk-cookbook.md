---
title: JavaScript SDK逐步指南
description: JavaScript SDK逐步指南
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# JavaScript SDK逐步指南 {#javascript-sdk-cookbook}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 簡介 {#intro}

本檔案說明程式設計師的上層應用程式為JavaScript與Adobe Pass驗證服務的整合所實作的權益工作流程。 JavaScript API參考資料連結包含在中。

另請注意 [相關資訊](#related) 區段包含一組JavaScript程式碼範例的連結。

## 權益流程 {#entitlement}

1. [必要條件](#prereq)
2. [啟動流程](#startup)
3. [驗證流程](#authn)
4. [授權流程](#authz)
5. [檢視Media流程](#logout)

</br>

![](assets/javascript-flows.png)


## 必要條件 {#prereq}

**相依性：**

- Adobe Pass Authentication Library (AccessEnabler)請洽詢您的Adobe Pass驗證帳戶管理員以安排此作業。
- 有效的Adobe Pass驗證請求者ID，請洽詢您的Adobe Pass驗證帳戶管理員以安排此作業。

建立回呼函式：

- `entitlementLoaded`
</br>

**觸發：** AccessEnabler已載入並完成初始化。

- `displayProviderDialog(mvpds)`

  **觸發：** `getAuthentication(),` 只有當使用者尚未選取提供者(MVPD)，且尚未驗證時，才會使用mvpds引數是使用者可用的提供者陣列。

- `setAuthenticationStatus(status, errorcode)`

  **觸發：**
   - `checkAuthentication()`每次。
   - `getAuthentication()` 僅當使用者已經驗證並已選取提供者時。

  傳回的狀態是成功或失敗；錯誤碼說明失敗的型別。

- `createIFrame(width, height)`

  **觸發：** `setSelectedProvider(providerID)`，前提是選取的提供者已設定為以IFrame顯示。

  >[!NOTE]
  >
  >提供者已設定為在iFrame中將驗證畫面轉譯為重新導向或，且程式設計師必須解決這兩項問題。

- `sendTrackingData(event, data)`

  **觸發器：** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  此 `event` 引數指出已發生的權益事件； `data` parameter是與事件相關的值清單。
- `setToken(token, resource)`
  **觸發：** `checkAuthorization()`和 `getAuthorization()` 成功授權後檢視資源。   此 `token` 引數是短期媒體Token； `resource` 引數是使用者有權檢視的內容。

- `tokenRequestFailed(resource, code, description)`
  **觸發：**`checkAuthorization()`&#x200B;和`getAuthorization()`  在授權失敗之後。\
  此 `resource` 引數是使用者嘗試檢視的內容； `code` parameter是錯誤碼，指出發生的失敗型別； `description` 引數說明與錯誤碼相關的錯誤。

- `selectedProvider(mvpd)`

  **觸發：** [`getSelectedProvider()`](#$getSelProv `mvpd` parameter提供使用者所選取之提供者的相關資訊。

- `setMetadataStatus(metadata, key, arguments)`

  **觸發：** `getMetadata().`\
  此 `metadata` parameter提供您要求的特定資料；key parameter是 `getMetadata()`請求；以及 `arguments` 引數是傳遞至的相同字典 `getMetadata()`.


## 2.啟動流程

**I.載入AccessEnabler JavaScript：**

**用於中繼設定檔**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

或……

**用於生產設定檔**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**觸發器：** 初始化完成後，Adobe Pass驗證會呼叫 `entitlementLoaded()` 回呼函式。 這是您應用程式與AccessEnabler通訊的進入點。


**二、** 呼叫 `setRequestor()`建立程式設計師的身分；傳入程式設計師的 `requestorID` 和（選擇性）Adobe Pass驗證端點的陣列。

**觸發器：** 無，但啟用 `displayProviderDialog()` 需要時呼叫。


**三、** 呼叫 `checkAuthentication()` 檢查現有的驗證，而不啟動完整驗證 [驗證流程].  如果此呼叫成功，您可以直接前往 `authorization flow`.  如果沒有，請繼續前往 `authentication flow`.

**相依性：** 成功呼叫 `setRequestor()`（此相依性也適用於所有後續呼叫）。

**觸發器：** `setAuthenticationStatus()` callback

</br>

## 3.驗證流程</span>


**相依性：** 成功呼叫 `setRequestor()`（此相依性也適用於所有後續呼叫）。


呼叫 `getAuthentication()` 以取得驗證狀態OR來觸發提供者驗證流程。

**觸發程式：**

- `displayProviderDialog()`如果使用者尚未通過驗證
- `setAuthenticationStatus()` 如果驗證已經發生

當AccessEnabler呼叫時到達驗證流程完成 `setAuthenticationStatus()`替換為 `isAuthenticated == 1`.

## 4.授權流程 {#authz}

**相依性：**

- 成功呼叫 `setRequestor()` （此相依性也適用於所有後續呼叫）。
- 與MVPD議定的有效ResourceID。 請注意，ResourceIDs應與任何其他裝置或平台上使用的相同，且在MVPD間將相同。

呼叫 `getAuthorization()` 並傳遞請求媒體的ResourceID。 成功的呼叫將傳回Short Media Token，以確認使用者有權檢視要求的媒體。

- 如果呼叫通過：使用者擁有有效的AuthN權杖，且使用者有權觀看要求的媒體。
- 如果呼叫失敗：檢查擲回的例外狀況，以判斷其型別（AuthN、AuthZ或其他專案）：
- 如果呼叫是AuthN錯誤，請重新啟動AuthN流程。
- 如果呼叫是AuthZ錯誤，則使用者無權觀看請求的媒體，並且應向使用者顯示某種錯誤訊息。
- 如果發生其他錯誤（連線錯誤、網路錯誤等）， 然後向使用者顯示適當的錯誤訊息。

使用媒體權杖驗證器，驗證從成功傳回的shortMediaToken `getAuthorization()` 呼叫。


**相依性：** 短媒體權杖驗證器（隨AccessEnabler程式庫提供）

- 如果驗證通過：為使用者顯示/播放請求的媒體。
- 如果失敗：AuthZ權杖無效，應拒絕媒體請求，並向使用者顯示錯誤訊息。

## 5.檢視Media流程 {#logout}

- 使用者選擇要檢視的媒體。
   - 媒體是否受到保護？
      - 您的應用程式會檢查媒體是否受到保護：
         - 如果媒體受到保護，您的應用程式會啟動上方授權(AuthZ)流程。
         - 如果媒體未受保護，請繼續檢視媒體流程。
         - 播放媒體

## 設定訪客ID {#visitorID}

設定 [Experience CloudvisitorID](https://experienceleague.adobe.com/docs/id-service/using/home.html) 從analytics的角度來看，值非常重要。 設定EC visitorID值後，SDK會將此資訊與每個網路呼叫一併傳送，而Adobe Pass驗證服務將會收集此資訊。 如此一來，您就可以將Adobe Pass驗證服務的分析資料與其他應用程式或網站的任何其他分析報表建立關聯。 如需如何設定EC visitorID的相關資訊，請參閱 [此處](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en).


>[!NOTE]
>
>請注意，自JS SDK 3.1.0版開始，即可支援此功能。

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
