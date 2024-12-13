---
title: 如何將MVPD登入頁面從iFrame移轉至快顯視窗
description: 如何將MVPD登入頁面從iFrame移轉至快顯視窗
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 0%

---

# （舊版）如何將MVPD登入頁面從iFrame移轉至Popup {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 快顯視窗與iFrame {#popup-vs-iframe}

有些使用者在MVPD登入頁面的iFrame實作中遇到第三方Cookie問題。
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

Adobe Pass驗證團隊&#x200B;**建議在Firefox和Safari上實作快顯視窗/新視窗登入頁面**，而非iFrame版本。  不過，如果您要實作Internet Explorer的登入頁面，您可能會遇到快顯視窗實作的問題。 IE問題是因為使用者在快顯視窗中使用其MVPD進行驗證後，Adobe Pass驗證會強制上層頁面重新導向，而這會被Internet Explorer視為快顯封鎖程式。 Adobe Pass驗證團隊&#x200B;**建議為Internet Explorer**&#x200B;實作iFrame登入。

此技術備註中顯示的範常式式碼使用iFrame和快顯視窗的混合實作 — 在Internet Explorer上開啟iFrame，並在其他瀏覽器上開啟快顯視窗。

鑑於iFrame實作已經存在，技術備註的第一部分會顯示iFrame實作的程式碼，第二部分則會顯示變更，以將快顯視窗實作設為預設值。


## 在iFrame中具有登入頁面的MVPD選取器 {#mvpd-pickr-iframe}

先前的程式碼範例顯示HTML頁面，其中包含要建立iFrame的&lt;div>標籤以及「關閉iFrame」按鈕：

```HTML
<body> 
    <div id="mvpddiv">
        <div style="background: red">
            <input type="button" id="btnCloseIframe" value="X" onclick="closeIframeAction();">
        </div>
        <br/>
        <!-- We use the "about:blank" value so that when the iFrame loads
             we do not see the the parent page. -->
        <iframe id="mvpdframe" name="mvpdframe" src="about:blank"></iframe>
    </div> 
</body>
```

以下是相關的&#x200B;**JavaScript**&#x200B;代碼：

```JavaScript
/*
 * Callback indicating that the AccessEnabler swf has initialized
 */
function swfLoaded() {
    // AccessEnabler is loaded so we can use the API function it provides
    accessEnablerObject.setRequestor(requestorID); 
    accessEnablerObject.checkAuthentication();
} 
/*
* The code the correctly closes the opened IFrame and does not leave the
* AccessEnabler in an undefined state.
*/
function closeIframeAction () {
    accessEnablerObject.setSelectedProvider(null);
    /* We use the "about:blank" value so that when the iFrame loads
     * we do not see the the previous MVPD.
     */
    document.getElementById('mvpdframe').src="about:blank";
    document.getElementById('mvpddiv').style.visibility="hidden";
    document.getElementById('mvpddiv').style.display="none";
}
/*
* Some of the supported MVPDs require their login pages to be displayed within an iFrame.
* In your HTML page, you must include the following <div> tag: "<div id="mvpddiv"></div>".
* Called when the selected MVPD requires an iFrame in which to display its authentication UI.
*/
function createIFrame(inWidth, inHeight) {
     // Create the iFrame to the specified width and height for the auth page
    ifrm = document.createElement("IFRAME");
    ifrm.name = "mvpdframe";
    ...
    document.getElementById('mvpddiv').appendChild(ifrm);
    ...
    // Force the name into the DOM since it is still not refreshed, for IE
    window.frames["mvpdframe"].name = "mvpdframe";
}
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
    /* This is an example how previously the MVPD picker was build and will be replaced. */
}
/* 
* Sending the user selected MVPD of the custom dialog is achieved
* by calling the API function setSelectedProvider(providerID:String).
*/
function setSelectedProvider(providerID) {
    accessEnablerObject.setSelectedProvider(providerID);
}
```


## 在快顯視窗中顯示登入頁面的MVPD選取器 {#mvpd-pickr-popup}

由於我們不會再使用&#x200B;**iFrame**，因此HTML程式碼將不會包含iFrame或關閉iFrame的按鈕。 先前包含iFrame的div - **mvpddiv** — 將會保留並用於下列專案：

* 在快顯視窗焦點遺失時，通知使用者MVPD登入頁面已開啟
* 提供連結，以重新取得快顯視窗的焦點

```HTML
<body onload="javascript:loadAccessEnabler();"> 
   <div id="aeHolder">
       <div name="contentAccessEnabler" id="contentAccessEnabler"></div>
   </div>
   <div id="mvpddiv">
      <p>
      <strong>No login window?</strong>
      <br/>
      <a href="javascript:mvpdWindow.focus();">Click here to open it.</a>
      <br/>
      Still not working? Check popup blockers!
      </p>
   </div> 
 
   <div id="picker" style="display:none">
      Choose MVPD: <br/>
      <select id="mvpdList" multiple></select>
      <br/>
         <a id="authenticateButton" onclick="javascript:authenticate();">Authenticate</a>
   </div>
</body>
```

MVPD清單將顯示在名為&#x200B;**picker**&#x200B;的div中，作為選取的&#x200B;**-mvpdList**。

將使用新的API回呼 — **setConfig(configXML)**。 呼叫setRequestor(requestorID)函式之後會觸發回呼。 此回呼會傳回與先前設定的requestorID整合的MVPD清單。 在回呼方法中，會剖析傳入的XML，並快取MVPD清單。 也會建立MVPD選取器，但不會顯示。

```JavaScript
var mvpdList;  // The list of cached MVPDs
var mvpdWindow;  // The reference to the popup with the MVPD login page
var cancelTimer = 0;
/* Callback indicating that the AccessEnabler swf has initialized */
function swfLoaded() {
   accessEnablerObject = $('#AccessEnabler').get(0);
   // Using a custom implementation of the MVPD picker
   accessEnablerObject.setProviderDialogURL('none');
   accessEnablerObject.setRequestor(requestorID); 
}

function setConfig(configXML) {
   mvpdList = [];
   $.each($($.parseXML(configXML)).find('mvpd'), function (idx, item) {
      var mvpdId = $(item).find('id').text();
      mvpdList[mvpdId] = {
         displayName: $(item).find('displayName').text(),
         logo: $(item).find('logoUrl').text(),
         popup: $(item).find('iFrameRequired').text() == "true",
         width: $(item).find('iFrameWidth').text(),
         height: $(item).find('iFrameHeight').text()
      };

      $('#mvpdList').append($('<option value="' + mvpdId + '" title="' + mvpdId + '">' + mvpdList[mvpdId].displayName + '</option>'));
   });
   accessEnablerObject.getAuthentication();
}
```

呼叫getAuthentication()或getAuthorization()函式之後，就會觸發displayProviderDialog()回呼。 一般而言，在此回呼中，會建立並顯示MVPD清單。 由於MVPD選取器已建置，因此只需對使用者顯示即可完成。

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

使用者從選擇器選取MVPD後，需要建立快顯視窗。 如果使用about：blank或其他網域上的頁面建立快顯視窗，有些瀏覽器可能會封鎖快顯視窗，因此建議使用載入AccessEnabler的主機名稱來開啟快顯視窗。

在iFrame實作中，重設驗證流程是由btnCloseIframe按鈕和JavaScript函式closeIframeAction()所完成，但現在已無法再裝飾iFrame。 因此，觀看快顯視窗關閉的時間（由使用者或完成驗證流程）也會獲得相同的行為。 已新增程式碼片段，萬一使用者失去快顯視窗的焦點時也有幫助：

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

在createIFrame()回呼上，將會顯示&#x200B;**mvpddiv** div。

```JavaScript
function createIFrame(width, height) {
   $('#mvpddiv').show();
   if (useIframeLoginStyle) {
      ...
   }
}

/* Function called when user has selected the MVPD from the picker */
function authenticate() {
   var selectedMvpd = $('#mvpdList').val()[0];
   if (mvpdList[selectedMvpd].popup) {
      mvpdWindow = window.open("http://entitlement.auth-staging.adobe.com", "mvpdframe", "width=" + mvpdList[selectedMvpd].width + ",height=" + mvpdList[selectedMvpd].height, true);
      if (!mvpdWindow) {
         // Message to user that popup blocker is not allowing the authentication process to start
      }
      //watch the mvpd popup for close
      clearInterval(cancelTimer);
      cancelTimer = setInterval(checkClosed, 200);
   }
   accessEnablerObject.setSelectedProvider(selectedMvpd);
}

function checkClosed() {
   try {
      if (mvpdWindow && mvpdWindow["closed"]) {
         clearInterval(cancelTimer);
         accessEnablerObject.setSelectedProvider(null);
         accessEnablerObject.getAuthentication();
         $('#mvpddiv').hide();
      }
   } catch (error) {
      console.log(error);
   }
}
```

>[!IMPORTANT]
>
>* 程式碼範例包含所用要求者ID - &#39;REF&#39;的硬式編碼變數，該變數應被真正的程式設計人員要求者ID取代。
>* 程式碼範例只會從與所使用要求者ID關聯的白名單網域中正確執行。
>* 由於整個程式碼都可供下載，此技術備註中顯示的程式碼已遭截斷。 如需完整的範例，請參閱&#x200B;**JS iFrame與快顯範例**。
>* 外部JavaScript資料庫是從[Google託管服務](https://developers.google.com/speed/libraries/)連結。
