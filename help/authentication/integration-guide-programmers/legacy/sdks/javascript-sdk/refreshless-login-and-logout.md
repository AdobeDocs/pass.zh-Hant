---
title: 不需重新整理的登入和登出
description: 不需重新整理的登入和登出
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1761'
ht-degree: 0%

---

# 不需重新整理的登入和登出 {#tefresh-less-login-and-logout}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

針對Web應用程式，您必須考慮驗證和登出使用者的某些不同可能情況。  MVPD需要使用者登入MVPD的網頁以進行驗證，且以下附加因素也會生效：

- 有些MVPD需要從您的網站完全重新導向至其登入頁面
- 有些MVPD會要求您在網站上開啟iFrame，以顯示MVPD的登入頁面
- 有些瀏覽器無法妥善處理iFrame案例，因此針對這些瀏覽器，較好的選擇是使用快顯視窗而非iFrame

在Adobe Pass Authentication 2.7之前，所有這些驗證使用者的情況都會涉及程式設計師頁面的完整頁面重新整理。對於2.7和後續版本，Adobe Pass Authentication團隊已改善這些流程，讓使用者不必在登入和登出期間在您的應用程式上體驗頁面重新整理。


## 詳細說明 {#detailed_description}

讓我們從原始驗證和登出流程的摘要開始，然後按照改善的驗證和登出流程進行。 請注意，前四個區段會處理一般MVPD （非TempPass），而最後一個區段則說明需要套用至TempPass的特殊實作：

- [原始驗證流程](#orig_authn)
- [原始登出流程](#orig_logout)
- [已改善驗證流程](#improved_authn)
- [已改善登出流程](#improved_logout)
- [TempPass流程](#improved_temppass)

</br>

## 原始驗證/登出流程 {#orig_authn}

**驗證**

Adobe Pass驗證Web使用者端有兩種驗證方式，視MVPD的需求而定：

1. **整頁重新導向 —**&#x200B;使用者選取提供者之後    （已設定全頁重新導向）從    程式設計師的網站`setSelectedProvider(<mvpd>)`會在AccessEnabler上叫用，且使用者會重新導向至MVPD的登入頁面。 在使用者提供有效認證後，他會被重新導向回程式設計師的網站。 AccessEnabler已初始化，並在`setRequestor`期間從Adobe Pass驗證擷取驗證Token。
1. **iFrame /彈出式視窗 —**&#x200B;當使用者選取提供者（已設定iFrame）後，會在AccessEnabler上叫用`setSelectedProvider(<mvpd>)`。 此動作將觸發`createIFrame(width, height)`回呼，通知程式設計師以名稱`"mvpdframe"`和提供的維度建立iFrame （或快顯視窗，視瀏覽器/偏好設定而定）。 建立iFrame/快顯視窗後，AccessEnabler會在iFrame/快顯視窗中載入MVPD的登入頁面。 使用者提供有效的認證，而iFrame/快顯視窗會重新導向至Adobe Pass驗證，而後者會傳回JS程式碼片段，關閉iFrame/快顯視窗並重新載入上層頁面（程式設計師網站）。 與流程1類似，驗證權杖是在`setRequestor`期間擷取。

`displayProviderDialog`回呼（由`getAuthentication`/`getAuthorization`觸發）傳回MVPD清單及其適當的設定。 MVPD的`iFrameRequired`屬性可讓程式設計師知道它應該啟動流程1還是流程2。 請注意，程式設計師只需要對流程2採取額外的動作（建立iFrame/快顯視窗）。

**取消驗證**

此外，在某些情況下，使用者會透過關閉登入頁面來明確取消驗證流程。 以下是情境和程式設計師的建議解決方案：

1. **全頁重新導向 —**&#x200B;當登入頁面關閉時，使用者需要再次導覽至程式設計師的網站，並從頭開始整個流程。 在此案例中，程式設計師端不需要任何明確動作。
1. **iFrame -**&#x200B;建議程式設計師將iFrame託管於附有「關閉」按鈕的`div` （或類似的UI元件）內。 當使用者按下[關閉]按鈕時，程式設計師會毀損iFrame以及相關的UI並執行`setSelectedProvider(null)`。 此呼叫可讓AccessEnabler清除其內部狀態，並讓使用者能夠起始後續驗證流程。 將觸發`setAuthenticationStatus`和`sendTrackingData(AUTHENTICATION_DETECTION...)`，以表示失敗的驗證流程（在`getAuthentication`和`getAuthorization`上）。
1. **快顯視窗 —**&#x200B;有些瀏覽器無法準確偵測視窗關閉事件，因此在這裡必須採取不同的方法（與上述iFrame流程相反）。 Adobe建議程式設計師初始化計時器，定期驗證登入快顯視窗是否存在。 如果視窗不存在，程式設計師可以確定使用者已手動取消登入流程，而且程式設計師可以繼續呼叫`setSelectedProvider(null)`。 觸發的回呼與上方流程2中的相同。

</br>

## 原始登出流程 {#orig_logout}

AccessEnabler的登出API會清除程式庫的本機狀態，並在目前的索引標籤/視窗中載入MVPD的登出URL。 瀏覽器會導覽至MVPD的登出端點，當程式完成時，會將使用者重新導向回程式設計師的網站。 使用者唯一需要的動作是按下「登出」按鈕/連結並起始流程；MVPD的登出端點不需要使用者互動。

**原始驗證/登出流程以及頁面重新整理**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## 改善（不需重新整理）驗證 {#improved_authn}

>[!NOTE]
>
>改良的無重新整理登入和登出流程需要瀏覽器支援現代化的HTML5技術，包括網頁傳訊。

上述的驗證（登入）和登出流程都可在每個流程完成後重新載入首頁面，以提供類似的使用者體驗。  目前的功能旨在透過提供不需重新整理（背景）的登入和登出來改善使用者體驗。 程式設計師可以透過將兩個布林值旗標（`backgroundLogin`和`backgroundLogout`）傳遞給`setRequestor` API的`configInfo`引數來啟用/停用背景登入和登出。 預設會停用背景登入/登出（提供與先前實作的相容性）。

**範例：**

```JSON
    var configInfo = {
        callSetConfig: true,
        backgroundLogin: true,
        backgroundLogout: true
    };
    accessEnabler.setRequestor(REQUESTOR_ID, null, configInfo);
```

**驗證**

以下幾點說明原始驗證流程和改進流程之間的轉換：

1. 完整頁面重新導向會由執行MVPD登入的新瀏覽器標籤取代。 當使用者選取MVPD （具有`iFrameRequired = false`）時，程式設計師必須建立名為`mvpdwindow`的新索引標籤（透過`window.open`）。 程式設計師接著執行`setSelectedProvider(<mvpd>)`，允許AccessEnabler在新索引標籤中載入MVPD登入URL。 使用者提供有效認證後，Adobe Pass驗證將關閉索引標籤，並將window.postMessage傳送至程式設計師的網站，以通知AccessEnabler驗證流程已完成。 系統會觸發下列回呼：

   - 如果流程是由`getAuthentication`起始： `setAuthenticationStatus`且會觸發`sendTrackingData(AUTHENTICATION_DETECTION...)`以表示驗證成功/失敗。

   - 如果流程是由`getAuthorization`起始： `setToken/tokenRequestFailed`且會觸發`sendTrackingData(AUTHORIZATION_DETECTION...)`以表示授權成功/失敗。

1. iFrame/彈出式視窗流程大致維持不變，差異在於使用者提供有效認證後，不會重新載入上層頁面。 iFrame/快顯視窗會在登入後自動關閉，並傳送`window.postMessage`至上層頁面，通知AccessEnabler流程已完成。 觸發的回呼與上一個流程相同，**加上下列新回呼：`destroyIFrame`**。 `destroyIFrame`回呼可讓程式設計師移除任何相關/輔助元件的iFrame，例如UI裝飾。 舊驗證流程不需要存在此回呼，因為登入完成後，Adobe Pass驗證會重新載入程式設計師的頁面，因此會摧毀該頁面上的所有UI元件。

</br>

>[!IMPORTANT]
> 
>您必須將MVPD登入iFrame或快顯視窗載入為包含AccessEnabler執行個體的頁面的直接子頁面。 如果MVPD登入iFrame或彈出式視窗在包含AccessEnabler執行個體的頁面下方巢狀內嵌兩個或多個層級，則流程可能會掛起。 例如，如果您的iFrame位於首頁面和MVPD iFrame (Page =\> iFrame =\> MVPD iFrame)之間，則登入流程可能會失敗。

</br>

**取消驗證**

以下是取消驗證的流程：

1. **瀏覽器索引標籤 —**&#x200B;由於索引標籤基本上是新視窗，因此擷取其關閉事件與情境3中從舊驗證流程中討論的限制相同。 此外，這裡無法使用計時器方法，因為無法區分使用者手動關閉的標籤和在登入流程結束時自動關閉的標籤。 此處的解決方案是讓AccessEnabler在使用者取消流程時保持「無訊息」（不觸發回呼）。 此外，程式設計師不需要採取任何特定動作。 使用者將能夠啟動另一個驗證流程，而不會收到「多重驗證請求錯誤」錯誤（此錯誤已在背景登入的AccessEnabler中停用）。

1. **iFrame -**&#x200B;程式設計師可以從舊的驗證流程（從iFrame建立包裝函式UI和觸發至`setSelectedProvider(null)`的相關「關閉」按鈕）採用案例2中討論的方法。 雖然此方法不再是強烈的要求（如上述案例1所述，背景登入可允許多個驗證流程），但Adobe仍建議使用。

1. **快顯視窗 —**&#x200B;此程式與上方的[瀏覽器]索引標籤流程相同。

</br>

## 已改善登出流程 {#improved_logout}

新的登出流程將在隱藏的iFrame中執行，因此會消除完整頁面重新導向。  這是可行的，因為使用者不需要在MVPD的登出頁面上採取特定動作。

登出流程完成後，系統會將iFrame重新導向至自訂Adobe Pass驗證端點。 這會向父系提供執行`window.postMessage`的JS程式碼片段，通知AccessEnabler登出已完成。 已觸發下列回呼： `setAuthenticationStatus()`和`sendTrackingData(AUTHENTICATION_DETECTION ...)`，表示使用者已不再驗證。

下圖顯示不需重新整理的流程，可讓使用者在不重新整理應用程式首頁面的情況下登入其MVPD：

**已改善（不需重新整理）驗證/登出流程**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## TempPass流程 {#improved_temppas}

不需重新整理的登入方式對TempPass型別的MVPD採用不同的方式。

由於TempPass流程需要自動建立視窗，並在沒有明確使用者互動的情況下關閉，因此對於某些瀏覽器（快顯視窗封鎖程式）可能會造成問題。 因此，AccessEnabler會在幕後實作登入階段，而不需要程式設計師建立的Web容器。

以下為程式設計師在實作不需重新整理的登入和登出之TempPass時需要注意的事項：

- 在開始驗證之前，只需要為非TempPass MVPD建立iFrame或快顯視窗。 程式設計師可以讀取MVPD物件的`tempPass`屬性（由`setConfig()` / `displayProviderDialog()`傳回），以偵測MVPD是否為TempPass。

- `createIFrame()`回呼必須包含對TempPass的檢查，而且只有在MVPD不是TempPass時才執行其邏輯。

- `destroyIFrame()`回呼必須包含對TempPass的檢查，而且只有在MVPD不是TempPass時才執行其邏輯。

- 在驗證完成之後叫用`setAuthenticationStatus()`和`sendTrackingData()`回呼（與一般MVPD的無重新整理流程完全相同）。

>[!NOTE]
>
>此流程僅適用於無重新整理的TempPass。 對於重新整理流程，需要明確處理TempPass （當TempPass需要iFrame /快顯視窗時）

</br>

下列程式碼範例示範如何處理程式設計師網站上的MVPD視窗（適用於一般MVPD和TempPass）：

```javascript
    var aeHostname = "https://entitlement.auth.adobe.com";
    var mvpdWindow = null;
    var mvpd = <mvpd_object_from_displayProviderDialog>;
    var useIframeLogin = <boolean_depending_on_browser_or_Programmer_preferences>;
    var backgroundLogin = <boolean_depending_on_Programmer_preferences>;
     
    // Do not create any windows for refreshless and temp pass
    if (!(backgroundLogin && mvpd.tempPass)) {
        if (backgroundLogin && !mvpd.popup) {
            mvpdWindow = window.open(aeHostname, "mvpdwindow");
        } else if (mvpd.popup && !useIframeLogin) {
            var width = mvpd.width;
            var height = mvpd.height;
            // Center on screen
            var top = (document.all) ? window.screenTop : window.screenY + 100;
            var left = (document.all) ? window.screenLeft : window.screenX + window.innerWidth / 2 - width / 2;
        
            mvpdWindow = window.open(aeHostname, "mvpdframe",
                           "width=" + width + ",height=" + height + ",top=" + top + ",left=" + left);
            // Monitor the mvpd popup for close
            if (!backgroundLogin) {
                clearInterval(cancelTimer);
                cancelTimer = setInterval(function () {
                    if (mvpdWindow && mvpdWindow.closed) {
                        clearInterval(cancelTimer);
                        $('#mvpddiv').hide();
                        accessEnablerAPI.setSelectedProvider(null);
                    }
                }, 200);
            }
        }
    }
```
