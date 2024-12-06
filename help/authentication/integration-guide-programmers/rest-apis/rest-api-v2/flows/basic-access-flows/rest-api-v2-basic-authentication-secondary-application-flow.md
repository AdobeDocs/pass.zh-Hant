---
title: 基本驗證 — 次要應用程式 — 流量
description: REST API V2 — 基本驗證 — 次要應用程式 — 流量
exl-id: 83bf592e-c679-4cfe-984d-710a9598c620
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2000'
ht-degree: 0%

---

# 在次要應用程式內執行的基本驗證流程 {#basic-authentication-flow-performed-within-secondary-application}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> REST API V2實作受到[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)檔案的限制。

Adobe Pass驗證許可權內的&#x200B;**驗證流程**&#x200B;可讓串流應用程式驗證使用者是否擁有有效的MVPD帳戶。 此程式要求使用者擁有作用中的MVPD帳戶，並在MVPD登入頁面上輸入有效的登入認證。

在下列情況下需要驗證流程：

* 使用者首次開啟應用程式時。
* 當使用者的先前驗證過期時。
* 當使用者從MVPD帳戶登出時。
* 當使用者想要使用不同的MVPD進行驗證時。

在所有這些情況下，呼叫任何設定檔端點的應用程式會收到空白回應或一或多個設定檔，但針對不同的MVPD。

**驗證流程**&#x200B;需要使用者代理程式（瀏覽器）來完成從應用程式到Adobe Pass後端，然後到MVPD登入頁面，最後返回應用程式的一系列呼叫。 此流程可能包括數個重新導向至MVPD系統並管理針對每個網域儲存的Cookie或工作階段，若沒有使用者代理程式，這對於實現和安全都極具挑戰。

根據主要應用程式（串流應用程式）支援使用者互動以選取MVPD以及在使用者代理程式中使用選取的MVPD進行驗證的功能，驗證情況如下：

* [在主要應用程式內執行驗證](rest-api-v2-basic-authentication-primary-application-flow.md)
* [使用預先選取的mvpd在次要應用程式內執行驗證](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [在次要應用程式內執行驗證，而不預先選取mvpd](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## 使用預先選取的mvpd在次要應用程式內執行驗證 {#perform-authentication-within-secondary-application-with-preselected-mvpd}

### 先決條件 {#prerequisites-perform-authentication-within-secondary-application-with-preselected-mvpd}

在啟動主要應用程式內的驗證流程，並透過次要應用程式內的使用者互動完成驗證流程之前，請確定符合下列先決條件：

* 串流應用程式必須選取MVPD。
* 串流應用程式必須起始驗證工作階段，才能使用選取的MVPD登入。
* 次要應用程式必須在使用者代理程式中使用選取的MVPD進行驗證。

>[!IMPORTANT]
>
> 假設
>
> <br/>
> 
> * 串流應用程式支援使用者互動以選取MVPD。
> * 次要應用程式（通常在次要裝置上）支援使用者互動，以在使用者代理程式中使用選取的MVPD進行驗證。

### 工作流程 {#workflow-perform-authentication-within-secondary-application-with-preselected-mvpd}

請依照指定的步驟，使用預先選取的MVPD來實作在次要應用程式內執行的基本驗證流程，如下圖所示。

![使用預先選取的mvpd在次要應用程式內執行驗證](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-with-preselected-mvpd.png)

*使用預先選取的mvpd在次要應用程式內執行驗證*

1. **建立驗證工作階段：**&#x200B;串流應用程式會呼叫工作階段端點，收集所有必要的資料以啟動驗證工作階段。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱[建立驗證工作階段](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API檔案：
   > 
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`、`mvpd`、`domainName`和`redirectUrl`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭
   >
   > <br/>
   > 
   > 串流應用程式在建立驗證工作階段時，必須在單一呼叫中提供所有必要的引數。

1. **指示下一個動作：**&#x200B;工作階段端點回應包含必要的資料，可引導串流應用程式瞭解下一個動作。

   >[!IMPORTANT]
   >
   > 如需工作階段回應中所提供資訊的詳細資訊，請參閱[建立驗證工作階段](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API檔案。
   > 
   > <br/>
   > 
   > 「工作階段」端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   > * 提供的`serviceProvider`與`mvpd`之間的整合必須是作用中。
   >
   > <br/>
   > 
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的額外資訊。

1. **繼續決策流程：**&#x200B;工作階段端點回應包含下列資料：
   * `actionName`屬性已設定為「授權」。
   * `actionType`屬性設定為「直接」。

   如果Adobe Pass後端識別有效的設定檔，串流應用程式就不需要使用選取的MVPD重新驗證，因為已經有設定檔可用於後續的決策流程。

1. **顯示驗證代碼：**&#x200B;工作階段端點回應包含下列資料：
   * `code`可用於在次要應用程式中繼續驗證工作階段。
   * `actionName`屬性已設定為「驗證」。
   * `actionType`屬性設定為「互動式」。

   如果Adobe Pass後端未識別有效的設定檔，串流應用程式會顯示可用於在次要應用程式中繼續驗證工作階段的`code`。

1. **驗證驗證碼：**&#x200B;次要應用程式會驗證所提供的使用者`code`，以確保它可以在使用者代理程式中繼續進行MVPD驗證。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱[擷取驗證工作階段資訊](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) API檔案：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`和`code`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭

1. **傳回有關驗證工作階段的資訊：**&#x200B;工作階段端點回應包含以下資料：
   * `existing`屬性包含已經提供的現有引數。
   * `missing`屬性包含遺漏的引數，需要提供這些引數才能完成驗證流程。

   >[!IMPORTANT]
   >
   > 請參閱[擷取驗證工作階段資訊](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) API檔案，以取得工作階段驗證回應中所提供資訊的詳細資訊。
   >
   > <br/>
   >
   > 「工作階段」端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   >
   > <br/>
   >
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的額外資訊。

   >[!TIP]
   >
   > 建議：次要應用程式可通知使用者，在錯誤回應指出遺失驗證工作階段的情況下，所使用的`code`無效，並通知他們使用新的工作階段重試。

1. **在使用者代理程式中開啟URL：**&#x200B;次要應用程式會開啟使用者代理程式，以載入自行計算的`url`，向驗證端點提出要求。 此流程可能包含數個重新導向，最終將使用者帶往MVPD登入頁面並提供有效認證。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱使用者代理程式](../../apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) API檔案中的[執行驗證：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`和`code`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭

1. **完成MVPD驗證：**&#x200B;如果驗證流程成功，使用者代理程式互動會在Adobe Pass後端儲存一般設定檔，並到達提供的`redirectUrl`。

1. **擷取特定程式碼的設定檔：**&#x200B;串流應用程式會傳送要求至設定檔端點，以收集擷取設定檔資訊所需的所有資料。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱特定程式碼](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API檔案的[擷取設定檔：
   > 
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`和`code`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭

   >[!TIP]
   >
   > 建議：串流應用程式可以使用`code`實作輪詢機制，以檢查一般設定檔是否已成功產生並儲存。

1. **傳回關於一般設定檔的資訊：**&#x200B;設定檔端點回應包含關於與所接收引數和標題關聯的一般設定檔的資訊。

   >[!IMPORTANT]
   >
   > 請參閱特定程式碼](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API檔案的[擷取設定檔，以取得設定檔回應中提供的詳細資訊。
   > 
   > <br/>
   > 
   > 設定檔端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   >
   > <br/>
   > 
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的額外資訊。

## 在次要應用程式內執行驗證，而不預先選取mvpd {#perform-authentication-within-secondary-application-without-preselected-mvpd}

### 先決條件 {#prerequisites-perform-authentication-within-secondary-application-without-preselected-mvpd}

在啟動主要應用程式內的驗證流程，並透過次要應用程式內的使用者互動完成驗證流程之前，請確定符合下列先決條件：

* 串流應用程式必須在需要登入時起始驗證工作階段。
* 次要應用程式必須選取MVPD。
* 次要應用程式必須在使用者代理程式中使用選取的MVPD進行驗證。

>[!IMPORTANT]
>
> 假設
>
> <br/>
> 
> * 次要應用程式（通常在次要裝置上）支援使用者互動以選取MVPD。
> * 次要應用程式（通常在次要裝置上）支援使用者互動，以在使用者代理程式中使用選取的MVPD進行驗證。

### 工作流程 {#workflow-perform-authentication-within-secondary-application-without-preselected-mvpd}

請依照指定的步驟來實作在次要應用程式中執行的基本驗證流程，而不使用預先選取的MVPD，如下圖所示。

![在次要應用程式內執行驗證，而不預先選取mvpd](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-without-preselected-mvpd.png)

*在次要應用程式內執行驗證，而不預先選取mvpd*

1. **建立驗證工作階段：**&#x200B;串流應用程式會收集一些必要的資料，藉由呼叫「工作階段」端點來啟動驗證工作階段。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱[建立驗證工作階段](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API檔案：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭
   >
   > <br/>
   > 
   > 串流應用程式在建立驗證工作階段時，無法在單一呼叫中提供所有必要的引數。

1. **指示下一個動作：**&#x200B;工作階段端點回應包含必要的資料，可引導串流應用程式進行下一個動作：
   * `code`可用於在次要應用程式中繼續驗證工作階段。
   * `actionName`屬性設定為「resume」。
   * `actionType`屬性設定為「直接」。

   >[!IMPORTANT]
   >
   > 如需工作階段回應中所提供資訊的詳細資訊，請參閱[建立驗證工作階段](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) API檔案。
   > 
   > <br/>
   > 
   > 「工作階段」端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   >
   > <br/>
   > 
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的額外資訊。

1. **顯示驗證代碼：**&#x200B;串流應用程式顯示可用於在次要應用程式中繼續驗證工作階段的`code`。

1. **提供驗證工作階段遺漏的引數：**&#x200B;次要應用程式會收集恢複驗證工作階段所需的所有遺漏資料，並呼叫工作階段端點。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱[繼續驗證工作階段](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) API檔案：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`、`mvpd`、`domainName`和`redirectUrl`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭

1. **指示下一個動作：**&#x200B;工作階段端點回應包含必要的資料，可引導串流應用程式瞭解下一個動作。

   >[!IMPORTANT]
   >
   > 如需工作階段回應中所提供資訊的詳細資訊，請參閱[繼續驗證工作階段](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) API檔案。
   > 
   > <br/>
   > 
   > 「工作階段」端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   > * 提供的`serviceProvider`與`mvpd`之間的整合必須是作用中。
   >
   > <br/>
   > 
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的額外資訊。

   >[!TIP]
   >
   > 建議：次要應用程式可通知使用者，在錯誤回應指出遺失驗證工作階段的情況下，所使用的`code`無效，並通知他們使用新的工作階段重試。

1. **指示現有的設定檔：**&#x200B;工作階段端點回應包含下列資料：
   * `actionName`屬性已設定為「授權」。
   * `actionType`屬性設定為「直接」。

   如果Adobe Pass後端識別有效的設定檔，串流應用程式就不需要使用選取的MVPD重新驗證，因為已經有設定檔可用於後續的決策流程。

1. **在使用者代理程式中開啟URL：**&#x200B;工作階段端點回應包含下列資料：
   * `url`可用來在MVPD登入頁面中起始互動式驗證。
   * `actionName`屬性已設定為「驗證」。
   * `actionType`屬性設定為「互動式」。

   如果Adobe Pass後端未識別有效的設定檔，次要應用程式會開啟使用者代理程式以載入提供的`url`，並向驗證端點提出要求。 此流程可能包含數個重新導向，最終將使用者帶往MVPD登入頁面並提供有效認證。

1. **完成MVPD驗證：**&#x200B;如果驗證流程成功，使用者代理程式互動會在Adobe Pass後端儲存一般設定檔，並到達提供的`redirectUrl`。

1. **擷取特定程式碼的設定檔：**&#x200B;串流應用程式會傳送要求至設定檔端點，以收集擷取設定檔資訊所需的所有資料。

   >[!IMPORTANT]
   >
   > 如需下列詳細資訊，請參閱特定程式碼](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API檔案的[擷取設定檔：
   >
   > * 所有&#x200B;_必要的_&#x200B;引數，例如`serviceProvider`和`code`
   > * 所有&#x200B;_必要的_&#x200B;標頭，例如`Authorization`、`AP-Device-Identifier`
   > * 所有&#x200B;_選用的_&#x200B;引數和標頭

   >[!TIP]
   >
   > 建議：串流應用程式可以使用`code`實作輪詢機制，以檢查一般設定檔是否已成功產生並儲存。

1. **傳回關於一般設定檔的資訊：**&#x200B;設定檔端點回應包含關於與所接收引數和標題關聯的一般設定檔的資訊。

   >[!IMPORTANT]
   >
   > 請參閱特定程式碼](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) API檔案的[擷取設定檔，以取得設定檔回應中提供的詳細資訊。
   > 
   > <br/>
   > 
   > 設定檔端點會驗證請求資料，以確保符合基本條件：
   >
   > * _必要_&#x200B;引數和標頭必須有效。
   >
   > <br/>
   > 
   > 如果驗證失敗，將會產生錯誤回應，提供可遵守[增強錯誤碼](../../../../features-standard/error-reporting/enhanced-error-codes.md)檔案的額外資訊。
