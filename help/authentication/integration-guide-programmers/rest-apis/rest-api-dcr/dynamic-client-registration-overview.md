---
title: Dynamic Client註冊概述
description: Dynamic Client註冊概述
exl-id: 9f98dfcd-4375-48c3-beff-259dfb1d3a26
source-git-commit: 7ca9d8996756086a6b963c0b6d5b0bb64608ecbc
workflow-type: tm+mt
source-wordcount: '809'
ht-degree: 0%

---

# Dynamic Client註冊概述 {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

動態使用者端註冊代表由[RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591)定義的授權機制，它以[RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)描述的OAuth 2.0授權架構為基礎。

Adobe Pass提供動態使用者端註冊服務，可讓您存取以下受保護的API：

* Adobe Pass驗證管理API：
   * [重設Temp Pass API](/help/premium-workflow/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
   * [降級API](/help/premium-workflow/degraded-access/degradation-feature.md#degradation-api-access)
   * [Proxy MVPD API](../../../integration-guide-mvpds/proxy-mvpd-webserv.md)
   * [權益服務監控API](/help/premium-workflow/esm/entitlement-service-monitoring-api.md)
* Adobe Pass驗證REST API：
   * [REST API V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [（舊版） REST API V1](../../legacy/rest-api-v1/rest-api-reference.md)
* Adobe Pass驗證SDK：
   * [（舊版） JavaScript SDK](../../legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
   * [（舊版） iOS/tvOS SDK](../../legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
   * [（舊版） Android SDK](../../legacy/sdks/android-sdk/android-sdk-api-reference.md)
   * [（舊版） FireOS SDK](../../legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> 動態使用者端註冊授權機製取代舊版Adobe Pass驗證解決方案，這些解決方案可能會停止服務：
>
> * 簽署的請求者ID機制。
> * 網域清單機制。
> * API金鑰機制。

採用動態使用者端註冊的主要優點包括：

* 增強式安全性。
* 跨平台的統一模型。
* 應用程式生命週期的精細控制。

若要瞭解如何管理和使用動態使用者端註冊的詳細資訊，請參閱下列章節。

## 動態使用者端註冊管理 {#dynamic-client-registration-management}

動態使用者端註冊管理程式可讓在特定平台上執行且需要存取特定Adobe Pass Authentication API的使用者端應用程式，透過[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)註冊。

Adobe Pass TVE Dashboard是供Adobe Pass驗證客戶（程式設計師）管理其設定和資料的工具。 此自助儀表板可啟用[Adobe Pass TVE儀表板使用手冊](../../../user-guide-tve-dashboard/tve-dashboard-overview.md)檔案中說明的一系列功能。

如果您可以存取[Adobe Pass TVE儀表板](https://experience.adobe.com/#/pass/authentication)，請依照下列各節中的步驟建立註冊的應用程式並下載軟體陳述式。

### 管理註冊的應用程式 {#manage-registered-applications}

>[!IMPORTANT]
>
> 如果您無法存取Adobe Pass TVE儀表板，請透過我們的[Zendesk](https://adobeprimetime.zendesk.com)建立票證，並要求您的技術客戶經理(TAM)建立註冊的應用程式，並與您共用軟體報表。

建立註冊應用程式有兩種可用方式：

* **程式設計師層級**

  程式設計人員層級的註冊程式可讓您建立已註冊的應用程式，連結至所有可用管道或選定的管道子集。 如需詳細資訊，請參閱[程式設計師的TVE儀表板使用手冊](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md)檔案。


* **頻道層級**

  頻道層級的註冊程式可讓您建立僅連結至目前所選頻道的已註冊應用程式。 如需詳細資訊，請參閱頻道的[TVE儀表板使用手冊](../../../user-guide-tve-dashboard/tve-dashboard-channels.md)檔案。

>[!IMPORTANT]
>
> 建議您建立具有更明確且有限許可權的註冊應用程式，以增強安全性並防止未經授權的存取。 因此，在建立註冊的應用程式時，請考慮針對指派的`channels`、`platforms`和`scopes`使用較窄的選項。
>
> 建議您針對使用者端應用程式的每次重大更新建立新的註冊應用程式，以管理其生命週期和使用狀況。 如有必要，請透過我們的[Zendesk](https://adobeprimetime.zendesk.com)建立票證，並要求您的技術客戶經理(TAM)撤銷註冊的應用程式，以封鎖特定使用者端應用程式版本的功能。

### 管理軟體陳述式 {#manage-software-statements}

>[!IMPORTANT]
>
> 如果您無法存取Adobe Pass TVE儀表板，請透過我們的[Zendesk](https://adobeprimetime.zendesk.com)建立票證，並要求您的技術客戶經理(TAM)建立註冊的應用程式，並與您共用軟體報表。

在下載軟體陳述式之前，請確定您已按照[管理註冊的應用程式](#manage-registered-applications)區段中的說明建立註冊的應用程式，符合您的使用者端應用程式需求。

您可以根據已註冊應用程式的建立層級，以下載軟體陳述式有兩種可用方式：

* **程式設計師層級**

  如需詳細資訊，請參閱[程式設計師的TVE儀表板使用手冊](../../../user-guide-tve-dashboard/tve-dashboard-programmers.md)檔案。

* **頻道層級**

  如需詳細資訊，請參閱頻道的[TVE儀表板使用手冊](../../../user-guide-tve-dashboard/tve-dashboard-channels.md)檔案。

軟體陳述式是JSON Web權杖(`JWT`)，其中包含您的使用者端應用程式軟體作為套件組合的資訊。 向[擷取使用者端認證](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API顯示時，軟體陳述式會使用JSON Web簽章(`JWS`)進行數位簽署。

如需軟體陳述式及其運作方式的詳細說明，請參閱[RFC 7591](https://tools.ietf.org/html/rfc7591)檔案。

## 動態使用者端註冊流程 {#dynamic-client-registration-flow}

總而言之，動態使用者端註冊授權機制包含數個步驟：

**管理**

* 使用者端代表必須建立已註冊的應用程式，如[管理已註冊的應用程式](#manage-registered-applications)區段中所述。
* 使用者端代表必須下載並內嵌軟體陳述式，如[管理軟體陳述式](#manage-software-statements)區段中所述。

**流量**

* 使用者端應用程式必須依照[擷取使用者端認證](apis/dynamic-client-registration-apis-retrieve-client-credentials.md) API檔案中的說明取得使用者端認證。
* 使用者端應用程式必須依照[擷取存取權杖](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) API檔案中的說明取得存取權杖。

請參閱[動態使用者端註冊流程](flows/dynamic-client-registration-flow.md)檔案，深入瞭解如何存取Adobe Pass保護的API。 此外，您也可以觀看此[網路研討會](https://my.adobeconnect.com/pzkp8ujrigg1/)影片，其中提供更多內容並包含示範。
