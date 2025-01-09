---
title: 暫時通過
description: 暫時通過
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '2243'
ht-degree: 0%

---

# 暫時通過 {#temp-pass}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 功能摘要 {#tempass-featur-summary}

Temp Pass可讓程式設計師為沒有MVPD帳戶憑證的使用者，提供對其受保護內容的暫時存取權。  臨時傳遞包括以下功能：

* Temp Pass可設定為提供暫時存取權，以涵蓋各種案例，包括：
   * 程式設計師可以提供其網站的每日短片（例如10分鐘預覽）。
   * 程式設計師可提供大型體育賽事（例如奧運會）的單一、長簡報（例如，四小時），或是NCAA March Madness。
   * 程式設計師可以提供前兩個案例的組合；例如，第一天的初始較長的檢視期間，隨後是一連串的短期期間，這些期間在隨後的某些天每天重複。
* 程式設計師指定其暫時通過的持續時間（存留時間或TTL）。
* 暫時傳遞會依請求者操作。  例如，NBC可以為請求者「NBCOlympics」設定4小時的暫時傳遞。
* 程式設計師可以重設授與特定請求者的所有Token。  用於實作Temp Pass的「臨時MVPD」必須設定為啟用「每個請求者的驗證」。
* **暫時傳遞存取權已授與特定裝置上的個別使用者**。 在使用者的「暫時傳遞」存取權到期後，該使用者將無法暫時存取相同的裝置，直到該使用者的授權權杖到期時從Adobe Pass驗證伺服器上清除為止。


>[!NOTE]
>
>臨時傳遞是Premium Workflow封裝的一部分。 如果您有興趣使用此功能，請聯絡您的Adobe Pass銷售代表。

## 功能詳細資料 {#tempass-featur-details}

* **如何計算檢視時間** - Temp Pass保持有效的時間與使用者在程式設計師應用程式上檢視內容的時間無關。  在透過Temp Pass的授權初始使用者請求後，到期時間透過將初始目前請求時間新增到程式設計師指定的TTL來計算。 此到期時間與使用者的裝置ID和程式設計師的請求者ID相關聯，並儲存在Adobe Pass驗證資料庫中。 每次使用者嘗試使用相同裝置的Temp Pass存取內容時，Adobe Pass驗證都會比較伺服器請求時間與使用者裝置ID和程式設計師請求者ID相關聯的到期時間。 如果伺服器要求時間小於到期時間，則會授與授權；否則，會拒絕授權。
* **設定引數** — 程式設計師可以指定下列Temp Pass引數，以建立Temp Pass規則：
   * **Token TTL** — 允許使用者在未登入MVPD的情況下觀看的時間長度。 這個時間是以時鐘為基礎，不管使用者是否觀看內容，都會過期。
  >[!NOTE]
  >要求者ID不能有多個與其關聯的暫時通過規則。
* **驗證/授權** — 在Temp Pass流程中，您將MVPD指定為「Temp Pass」。  Adobe Pass驗證不會與Temp Pass流程中的實際MVPD通訊，因此「Temp Pass」MVPD會授權任何資源。 程式設計師可以指定可使用Temp Pass存取的資源，就像他們對其網站上的其他資源所做的一樣。 媒體驗證器程式庫可照常使用，以驗證Temp Pass短媒體權杖，並在播放前強制執行資源檢查。
* **在暫時傳遞流程中追蹤資料** — 關於暫時傳遞權利流程期間追蹤資料的兩點：
   * 從Adobe Pass驗證傳遞至您&#x200B;**sendTrackingData()**&#x200B;回呼的追蹤ID是裝置ID的雜湊。
   * 由於Temp Pass流程中使用的MVPD ID是「Temp Pass」，因此相同的MVPD ID會傳回&#x200B;**sendTrackingData()**。 大多數程式設計師可能會想要以不同的方式處理暫時通過量度與實際MVPD量度。 這需要在您的Analytics實作中進行一些額外工作。

下圖顯示「暫時通過」流程：

![暫時傳遞流程](../../../assets/temp-pass-flow.png)

*圖：暫時傳遞流程*

## 實作暫時傳遞 {#implement-tempass}

在Adobe Pass驗證端，Temp Pass是在參與的程式設計師伺服器設定中新增名為「TempPass」的偽MVPD來實作。  這個偽MVPD的作用就像實際的MVPD，會暫時授予程式設計師受保護內容的存取權。

在程式設計師方面，Temp Pass的實作方式如下，適用於MVPD用於驗證的兩種情況：

* 程式設計師頁面&#x200B;**上的** iFrame。 無論MVPD的驗證型別為何，Temp Pass都能運作，但若是iFrame案例，則需要執行其他步驟以取消目前的驗證流程，並使用Temp Pass進行驗證。 這些步驟顯示在下面的[iFrame登入](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md)中。
* **重新導向至MVPD登入頁面**。 在更傳統的情況下，在開始使用MVPD驗證之前會顯示觸發暫時傳遞的UI，無需採取任何特殊步驟。 臨時傳遞的待遇應該與一般MVPD相同。

下列幾點適用於兩種實施情境：

* 「臨時通過」應僅對尚未請求臨時通過授權的使用者顯示在MVPD選擇器中。 在Cookie上保留標幟可封鎖後續請求的顯示。 只要使用者未清除瀏覽器快取，此功能就會運作。 如果使用者清除其瀏覽器快取，選擇器中會再次顯示「Temp Pass」，使用者將能夠再次請求。 只有在「暫時通過」時間尚未過期時，才會授予存取權。
* 當使用者透過Temp Pass請求存取時，Adobe Pass驗證伺服器將不會在驗證過程中執行其常用的安全性宣告標籤語言(SAML)請求。 而是會在每次在權杖對裝置有效時叫用驗證端點時，使其傳回成功。
* Temp Pass過期時，其使用者將不再經過驗證，因為在Temp Pass流程中，驗證權杖和授權權杖具有相同到期日。 為了向使用者說明其Temp Pass已過期，程式設計師必須在呼叫`setRequestor()`後立即擷取選取的MVPD，然後照常呼叫`checkAuthentication()`。 在`setAuthenticationStatus()`回呼中，可以執行檢查以判斷驗證狀態是否為0，以便如果選取的MVPD為「TempPass」，則可以向使用者顯示訊息，指出其Temp Pass工作階段已過期。
* 如果使用者在到期前刪除Temp Pass權杖，後續權益檢查將產生TTL等於剩餘時間的權杖。
* 如果使用者在到期後刪除暫時傳遞Token，後續軟體權利檔案檢查將傳回「使用者未授權」。

請參閱下列[範常式式碼](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#tempass-sample-code)中的範例，以取得如何編寫本節中說明的實作詳細資訊的範例。

## 程式碼範例 {#tempass-sample-code}

以下各節說明如何呼叫Adobe Pass驗證API以實作Temp Pass流程：

* [iFrame登入範例](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#iframe-login-sample)
* [自動登入範例](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#auto-login-sample)

### iFrame登入範例 {#iframe-login-sample}

此範例說明如何在MVPD支援iFrame整合的情況下實作Temp Pass：

```HTML
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript" src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var ae, ifrm, providersMenu, previousSelectedProvider;
        var tempassSelected = false;
 
        $(document).ready(function() {
            ifrm = $('#ifrm');
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {wmode: "transparent", allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded() {
            ae = $('#accessEnabler')[0];
            ae.setProviderDialogURL("none");
            ae.setRequestor("sample_requestor_Id");
            previousSelectedProvider = ae.getSelectedProvider(); 
            ae.checkAuthentication();
        }
 
        function createIFrame() {
            providersMenu.hide();
 
            // If the user already used TempPass once, hide the button
            if ($.cookie("TPSelected") == "1"){
                $('#tempassBtn').hide();
            }
            ifrm.show();
        }
 
        function displayProviderDialog(providers) {
            if (tempassSelected) {
                // Remember in a cookie that the user selected temp pass
                $.cookie("TPSelected", "1", {expires: 366, path: '/'});
 
                // Authenticate with temp pass
                ae.setSelectedProvider("TempPass");
            } else {
                $('#loginBtn').hide();
                providersMenu = $('<select></select>');
 
                providersMenu.change(function(event){
                    ae.setSelectedProvider(event.target.value);
                });
 
                $.each(providers, function(k, v) {
                    // Add the MVPDs to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if(v.ID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:v.ID}).text(v.displayName));                       
                    }
                });
                $('body').append(providersMenu);
            }
        }
 
        function setAuthenticationStatus(status, code) {
            loginBtn = $('#loginBtn');
            logoutBtn = $('#logoutBtn');
            console.log(previousSelectedProvider);
 
            if (status == 1) {
                $('#selectedProvider').text("Authenticated with " + ae.getSelectedProvider().MVPD + "   ");
                loginBtn.hide();
                logoutBtn.show();
 
                // Get authorization
                ae.getAuthorization("sample_requestor_Id");
            } else {
                // If selected provider is TempPass but the user is not authenticated,
                //   infer that the TempPass period has expired, so reset the MVPD selection
                if (previousSelectedProvider && previousSelectedProvider.MVPD == "TempPass") {
                    previousSelectedProvider = null;
                    ae.setSelectedProvider(null);
                    alert("Your Temp Pass has expired, please login with your regular cable provider!");
                }
                loginBtn.show();
                logoutBtn.hide();
            }
        }
 
        function selectTempPass() {
            ifrm.hide();
 
            // Signal the fact that the user selected temp pass
            tempassSelected = true;
 
            // Cancel the current authentication flow
            ae.setSelectedProvider(null);
 
            // Retry authentication
            ae.getAuthentication();
 
        }
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="ae.getAuthentication();">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
           style="display: none" onclick="ae.logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px; width: 400px; height: 400px; border: 2px solid red;">
        <button id="tempassBtn"
           onclick="selectTempPass();"
             style="float:left">Don't know your credentials? Click here to get a Temp Pass.
        </button>
        <button onclick="window.location.reload()" style="float:right">X</button>
        <br />
        <hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="90%" height="90%" frameborder="0"></iframe>
    </div>
    <br/>
    <div id="ae" style="display: none">
        <p>Loading Access Enabler...</p>
    </div>
</body>
</html>
```

#### iFrame登入使用案例 {#iframe-login-use-cases}

**第一次要求暫時傳遞：**

1. 使用者存取程式設計師頁面並按一下登入連結。
1. MVPD選取器隨即開啟，使用者可從清單中選擇MVPD。
1. 隨即顯示驗證iFrame。 此iFrame包含「暫時通過」連結。
1. 使用者按一下「Temp Pass」，程式設計師就會將標幟新增至Cookie，以防止使用者在後續造訪頁面時看到「Temp Pass」連結。
1. Temp Pass驗證請求到達Adobe Pass驗證伺服器，伺服器會產生驗證權杖。 TTL等於程式設計師為暫存階段設定的時段。
1. 臨時傳遞授權請求到達Adobe Pass驗證伺服器。
1. Adobe Pass驗證伺服器會從請求中擷取裝置和請求者ID，並將其與到期時間一起儲存在資料庫中。 到期時間的計算方式為：初始Temp Pass要求時間加上TTL （由程式設計師指定）。
1. Adobe Pass驗證伺服器會產生授權權杖。
1. 使用者存取受保護的內容。

**若要在回訪的Temp Pass使用者刪除瀏覽器Cookie後再次要求Temp Pass：**

1. 使用者存取程式設計師的頁面並按一下登入連結。
1. MVPD選取器隨即開啟，使用者可從清單中選擇MVPD。
1. 隨即顯示驗證iFrame。 此iFrame包含「暫時傳遞」連結（使用者已刪除原始Cookie，因此程式設計師不確定使用者之前是否按過「暫時傳遞」連結）。
1. 使用者再次按一下「暫時傳遞」按鈕，程式設計師就會再次將標幟新增至Cookie，以防止使用者在後續造訪頁面時看到「暫時傳遞」連結。
1. Temp Pass驗證請求到達Adobe Pass驗證伺服器，這些伺服器會產生驗證Token。 TTL現在是暫時通過的剩餘時間（目前時間以及與裝置ID相關聯的到期時間之間的差值）。
1. 臨時傳遞授權請求到達Adobe Pass驗證伺服器。
1. Adobe Pass驗證伺服器會從請求中擷取裝置和請求者ID，並使用它們從Adobe Pass驗證資料庫擷取到期時間。 系統會比較目前時間與到期時間。
1. 如果使用者的暫時傳遞尚未過期，Adobe Pass驗證伺服器會產生授權權杖。
1. 如果使用者的臨時密碼尚未過期，則使用者將可存取受保護的內容。

### 自動登入範例 {#auto-login-sample}

以下範例說明使用者造訪網站時，使用TempPass自動登入的情況。 使用者可以隨時選擇使用一般的MVPD登入，如果TempPass已過期，系統會發出警告：

```HTML
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript"
             src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript"
             src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript"
             src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var REQUESTOR = "REF";
        var RESOURCE = "sample_requestor_Id";
        var selectedProvider = null;
        var mvpds;
        var hasTempPassMVPD = false;
 
        // Used to cache the mvpd picker
        var picker;
 
        $(document).ready(function() {
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded(){
            console.log("AccessEnabler loaded");
            ae = $('#accessEnabler')[0];
 
            // Make sure the default picker is disabled
            ae.setProviderDialogURL("none");
 
            ae.setRequestor(REQUESTOR);
            ae.checkAuthentication();
        }
 
        /**
         * Callback received as a result of setRequestor()
         *
         * @param xml object holding the configuration for the current REQUESTOR
         * including the MVPD list
         */
        function setConfig(config) {
            // Save the mvpd list
            var mvpdList = $.parseXML(config);
            mvpds = $(mvpdList).find('mvpd');
 
            // Create the picker only once and cache it
            if(!picker) {
                picker = $('<div id="mvpdPicker"/>');
 
                var providersMenu = $('<select id="mvpdList" multiple></select>');
 
                $.each(mvpds, function(k, v) {
                    var mvpdID = $(v).find("id").text();
                    var mvpdName = $(v).find("displayName").text();
 
                    // Add the mvpd's to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if (mvpdID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:mvpdID}).text(mvpdName));
                    } else {
                        hasTempPassMVPD = true;
                    }
                });
                picker.append(providersMenu);
                picker.append($('<br/>'));
                picker.append($('<input type="button" onclick="login()" value="login" />'));
                picker.append($('<input type="button" onclick="cancelPicker()" value="cancel" />'));                  
            }
 
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");             
            }
        }
 
        /**
         * Callback triggered for iFramed MVPD's
         */
        function createIFrame() {
            $('#mvpdPicker').remove();
            $('#ifrm').show();
        }
 
        /**
         * Hides the MVPD picker
         * when the user clicks "Cancel"
         */
        function cancelPicker() {
            $('#video').show();
            $('#mvpdPicker').remove();
            $('#loginBtn').show();
        }
 
        /**
         * Pops up the MVPD picker
         */
        function showPicker() {
            $('#video').hide();
            $('#loginBtn').hide();
            $('body').append(picker);
        }
 
        function logout() {
            $.removeCookie('tempPassUsed');
            ae.logout();
        }
 
        /**
         * Performs login with the selected MVPD
         */
        function login() {
            selectedProvider = $('#mvpdList').val()[0];
 
            // Make sure we clear out previously
            // selected. This is a must if we want to force
            // login with a real MVPD while still logged in with
            // TempPass, without doing an ae.logout()
            ae.setSelectedProvider(null);
            ae.getAuthentication();
        }

        /**
         * Callback triggered by AccessEnabler. This is usually
         * triggered in order to display the MVPD picker, but
         * since we already constructed, cached, and displayed the
         * picker, and the user already picked the MVPD, we don't need
         * to do anything here but state management
         */
        function displayProviderDialog() {
            // If the selected MVPD is TempPass
            // store this fact in a cookie,
            // otherwise clear it
            if (selectedProvider != 'TempPass') {
                $.removeCookie('tempPassUsed');
            } else {
                $.cookie("tempPassUsed", 1);
            }
 
            // Since the picker was already shown
            // and the user picked an MVPD,
            // just proceed to login
            ae.setSelectedProvider(selectedProvider);
        }
 
        function setAuthenticationStatus(status, code) {
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");
            } else if(status == 1) {
                selectedProvider = ae.getSelectedProvider().MVPD;
                $('#selectedProvider').text("Authenticated with " + selectedProvider + "   ");
 
                // If authenticated with TempPass
                // allow the user to login with
                // a real MVPD
                if (selectedProvider == "TempPass") {
                    $('#loginBtn').show();
                    $('#logoutBtn').hide();
                } else {
                    $('#loginBtn').hide();
                    $('#logoutBtn').show();
                }
 
                // Get authorization
                // Note: This is mandatory in order to "start" the temp pass countdown
                ae.checkAuthorization(RESOURCE);
            } else if(code != "Provider not Selected Error") {
                // Auto-authenticate with TempPass only if we infer
                // that TempPass has not expired, otherwise we
                // inform the user that TempPass has expired
                if ($.cookie('tempPassUsed') == 1) {
                   $('#selectedProvider').text("Your Temp Pass has expired, please log in with your cable provider!");
                   $('#logoutBtn').show();
                   showPicker();
                } else {
                    selectedProvider = 'TempPass';
                    ae.getAuthentication();
                }
            }
        }
 
        /**
         * Displays the picker as a result
         * of user action
         */
        function loginClicked() {
            $('#loginBtn').hide();
            showPicker();
        }
 
        /**
         * Callback triggered in case of authorization success
         */
        function setToken(token) {
            console.log(token);
            $('#video').html('<img src=">');
        }
 
        /**
         * Callback triggered in case of authz failure
         */
        function tokenRequestFailed(resource, status, message) {
            console.log(resource);
            $('#video').html('<p style="color: red">' + status + ': ' + message + '</p>');
        }
 
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="loginClicked()">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
        style="display: none" onclick="logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px;
         width: 400px; height: 400px; border: 2px solid red;">
        <button onclick="window.location.reload()" style="float:right">Close this window</button>
        <br /><hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="80%" height="80%" frameborder="0"></iframe>  
    </div>
    <br/>
 
    <div id="video"></div>
    <div id="ae" style="display: none"><p>Loading Access Enabler...</p></div>
</body>
</html>
```

## 使用多個臨時路徑 {#use-mult-tempass}

某些事件需要分階段免費存取內容，例如免費存取的初始間隔（例如4小時），然後是每日免費存取（例如，後續每一天為10分鐘）。  為了讓程式設計師實作此情境，他們必須安排其Adobe連絡人，以便為程式設計師設定兩個暫時的MVPD。

此範例情境（初始4小時的免費工作階段，接著每日10分鐘的免費工作階段）Adobe會設定名為TempPass1的MVPD，其存留時間(TTL)為4小時，並為後續期間的TTL為10分鐘。  兩者都與程式設計師的請求者ID相關聯。

### 程式設計師實作 {#mult-tempass-prog-impl}

在Adobe設定兩個TempPass執行個體後，兩個額外的MVPD （TempPass1和TempPass2）會出現在程式設計師的MVPD清單中。  程式設計師必須執行下列步驟，實作多個臨時路徑：

1. 使用者初次造訪網站時，會自動使用TempPass1登入。 您可以使用上述的自動登入範例作為此工作的起點。
1. 當您偵測到TempPass1已過期時，請將事實儲存在Cookie/本機儲存體中，並向使用者呈現您的標準MVPD選取器。 **請確定從該清單**&#x200B;篩選掉TempPass1和TempPass2。
1. 在後續每一天，如果TempPass1已過期，請使用TempPass2自動登入該使用者。
1. TempPass2過期時，請儲存當天的事實（在Cookie/本機儲存體中），並向使用者呈現您的標準MVPD選取器。 請再次確定從該清單中篩選掉TempPass1和TempPass2。
1. 在每個新的日期，於00:00時，使用[Reset TempPass Web API](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#reset-all-tempass)重設TempPass2的所有暫時傳遞。

>[!NOTE]
>**程式設計備註：** Adobe Pass驗證沒有內建機制，無法在10分鐘後停止免費串流。  一旦TempPass2過期，由程式設計師決定是否限制存取。 為此，程式設計師可以在他們的網站/應用程式中每X分鐘實施一次「checkAuthorization」呼叫，其中X是程式設計師認為對其應用程式有意義的時間段。

## 重設所有暫時傳遞 {#reset-all-tempass}

某些商業規則需要定期清除臨時通行證，或臨時重設針對特定請求者ID和MVPD ID核發的所有臨時通行證。 此功能支援下列使用案例：

* 每日10分鐘的臨時通過（臨時通過必須在一天開始時重置）
* 在突發新聞期間，所有使用者都可以使用暫時傳遞。 （突發新聞一開始，所有裝置都必須重設此「暫時通過」。）
* 多重「暫時通過」案例提供某個長度的初始檢視期間組合，接著另一個長度的後續每日期間。

為了重設所有Temp Pass，Adobe Pass驗證為程式設計師提供&#x200B;*公用*&#x200B;網頁API：

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset
```

>[!NOTE]
>上述URL會取代先前的重設API。 舊的重設API (v1)不再受支援。

* **通訊協定：** HTTPS
* **主機：**
   * 發行版本 — mgmt.auth.adobe.com
   * 前述 — mgmt-prequal.auth.adobe.com
* **路徑：** /reset-tempass/v2/reset
* **查詢引數：** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **標頭：** ApiKey - 1232293681726481
* **回應：**
   * 成功 — HTTP 204
   * 失敗：
      * HTTP 400的不正確請求
      * 若未指定ApiKey，則為HTTP 401
      * HTTP 403 （如果ApiKey無效）

例如：

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

## 支援的使用者端 {#supp-clients}

平台支援暫時通過和重設工具：

| Adobe Pass驗證使用者端 | 暫時通過 | 重設工具 |
|:--------------------------------------:|:---------:|:----------:|
| js AccessEnabler | 是 | 是 |
| Native Client iOS | 是 | 是 |
| 原生使用者端tvOS | 是 | 是 |
| Native Client Android | 是 | 是 |
| Native Client fireTV | 是 | 是 |
| 無使用者端API | 是 | 是 |

## 限制和已知問題 {#limitations}

本節說明適用於目前實作Temp Pass的限制。

**JavaScript SDK**：支援從版本&#x200B;**3.X及更高版本**&#x200B;重設暫時傳遞功能。

<!--For Customers migrating from the 2.X JavaScript AccessEnabler to the 3.X JavaScript AccessEnabler, see [AccessEnabler JS 2.x to JS 3.x migration guide](https://tve.helpdocsonline.com/accessenabler-js-to-js-migration-guide).-->
