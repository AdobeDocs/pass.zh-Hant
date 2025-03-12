---
title: 產品公告
description: 產品公告
exl-id: 3c9c66e1-d31d-4af3-8ab2-eb32492f42ca
source-git-commit: 6b309f66bfe1376bd62ad5e99b4f6383194af6c6
workflow-type: tm+mt
source-wordcount: '973'
ht-degree: 1%

---

# 產品公告 {#product-announcements}

<a href="https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements">![直播網路研討會系列](/help/authentication/assets/rest-api-v2/live-webinar-series-banner.png)</a>

我們身為Adobe Pass的重要驗證合作夥伴，誠摯邀請您參加我們即將舉辦的新REST API直播網路研討會。 新API已於去年推出，以提升一般使用者體驗並簡化與Adobe Pass服務的整合。 

我們將舉辦一系列的兩個網路研討會，概述新API、優點以及可透過移轉至新API啟用的其他使用案例。

請報名參加最適合您和您的團隊的網路研討會。

* [網路研討會1 - 2025年2月19日](https://events.teams.microsoft.com/event/83c6ec1e-2522-4918-910f-c529b4fb0574@fa7b1b5a-7b34-4387-94ae-d2c178decee1)
* [網路研討會2 - 2025年3月5日](https://events.teams.microsoft.com/event/6f673689-e848-4c00-93d9-4f5d8f108977@fa7b1b5a-7b34-4387-94ae-d2c178decee1)

在這場會議中，您將瞭解：

* REST API v2概述和優點
* 基本流程逐步解說
* 時間表和後續步驟

如果您符合下列條件，此網路研討會會相當實用：

* 新客戶計畫啟動新的TVE應用程式
* 需要移轉至新API的現有客戶
* 將為客戶實作API的實作合作夥伴

您可以在[這裡](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)找到有關新API的技術檔案。 我們鼓勵您和您的團隊在[這裡](https://forms.office.com/r/sJea78tUy3)填妥您想要在會議中討論的任何問題。

## 生命週期結束(EOL) {#eol}

本節概述計畫淘汰的特定Adobe Pass驗證功能和產品的支援終止和生命週期終止日期。

請務必隨時瞭解淘汰時間表，並計畫採取下列行動。

| 宣告 | 宣告日期 | 支援終止日期 | 生命週期結束日期 |   | 說明 |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|---------------------|---------------------------------------------|:--|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Adobe Pass驗證TVE控制面板URL生命週期結束 <ul><li><a href="https://console.auth.adobe.com">console.auth.adobe.com</a></li><li><a href="https://console.auth-staging.adobe.com">console.auth-staging.adobe.com</a></li><li><a href="https://console-prequal.auth.adobe.com">console-prequal.auth.adobe.com</a></li><li><a href="https://console-prequal.auth-staging.adobe.com">console-prequal.auth-staging.adobe.com</a></li></ul> | 10/02/2024 | 01/01/2025 | 03/12/2025 |   | 我們規劃於2025年3月12日&#x200B;**在[console.auth.adobe.com](https://console.auth.adobe.com)、[console.auth-staging.adobe.com](https://console.auth-staging.adobe.com)、[console-prequal.auth.adobe.com](https://console-prequal.auth.adobe.com)和[console-prequal.auth-staging.adobe.com](https://console-prequal.auth-staging.adobe.com)託管的Adobe Pass Authentication TVE Dashboard生命週期結束。**</br></br>在此日期之後，將無法再存取上述URL，而您將重新導向新的[TVE控制面板](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)，以繼續存取您的整合。</br></br>若要確保繼續提供服務，您必須存取託管於[experience.adobe.com/pass/authentication](https://experience.adobe.com/pass/authentication)的新[TVE儀表板](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)。 |
| Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5生命週期結束 | 12/11/2024 | 不適用 | <span style="color: red;">01/08/2025</span> |   | 我們計畫於&#x200B;**2025年1月8日**&#x200B;終止Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5的生命週期。 在此日期之後，AccessEnabler JavaScript SDK v3.5所提供的功能和服務將無法繼續運作，包括使用者驗證和授權。 |
| Adobe Pass驗證非DCR安全性機制EOL | 12/11/2024 | 不適用 | <span style="color: red;">01/20/2025</span> |   | 我們計畫於&#x200B;**2025年1月20日**&#x200B;終止Adobe Pass驗證非DCR安全性機制的生命週期。 此機制用於保護對以下Adobe Pass Authentication API的存取權：<ul><li><a href="/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md">重設暫時傳遞API</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md">降級API</a></li><li><a href="/help/authentication/integration-guide-mvpds/proxy-mvpd-webserv.md">Proxy MVPD API</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md">軟體權利檔案服務監視API</a></li></ul>在此日期之後，由上述使用此安全性機制存取的API所提供的功能和服務將無法繼續運作。</br></br>若要確保繼續提供服務，您必須移轉所有應用程式以使用[動態使用者端註冊](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)機制。 |
| Adobe Pass驗證REST API v1生命週期結束 | 12/11/2024 | 12/31/2025 | 2026年底（待定） |   | 我們計畫於&#x200B;**2025年12月31日**&#x200B;停止支援Adobe Pass Authentication REST API v1。 在此日期之後，將不再提供更新或修正。</br></br>若要確保持續支援，您必須將所有應用程式移轉至<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>。</br></br>我們計畫在2026年&#x200B;**月底之前**&#x200B;結束Adobe Pass Authentication REST API v1的生命週期。 在此日期之後，REST API v1提供的功能和服務將無法繼續運作，包括使用者驗證和授權。</br></br>若要確保繼續提供服務，您必須將所有應用程式移轉至<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>。 |
| Adobe Pass Authentication AccessEnabler SDK終止服務 | 12/11/2024 | 05/31/2026 | 2026年底（待定） |   | 我們計畫於2026年5月31日&#x200B;**停止支援Adobe Pass Authentication AccessEnabler SDK**。 在此日期之後，將不再提供更新或修正。</br></br>若要確保持續支援，您必須將所有應用程式移轉至<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>。</br></br>我們計畫在2026年&#x200B;**月底之前，結束Adobe Pass Authentication AccessEnabler SDK的生命週期**。 在此日期之後，AccessEnabler SDK所提供的功能和服務將無法繼續運作，包括使用者驗證和授權。</br></br>若要確保繼續提供服務，您必須將所有應用程式移轉至<a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>。 |

## 產品發行 {#product-releases}

本節會編譯Adobe Pass Authentication發行版本記錄的參考和對應的發行說明。

### 2025 {#product-releases-2025}

| 發行說明 | 日期 |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass Authentication 3.1.0發行說明](notes-releases/auth-rn-310.md) | 02/25/2025 - 02/27/2025 |
| [Adobe Pass Authentication JavaScript SDK 4.7.1發行說明](notes-releases/authn-rn-javascript-471.md) | 02/25/2025 - 02/27/2025 |

### 2024 {#product-releases-2024}

| 發行說明 | 日期 |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass Authentication 3.0.3發行說明](notes-releases/auth-rn-303.md) | 10/29/2024 - 10/31/2024 |
| [Adobe Pass Authentication 3.0發行說明](notes-releases/auth-rn-300.md) | 09/10/2024 - 09/12/2024 |
| [Adobe Pass Authentication 2.70發行說明](notes-releases/auth-rn-270.md) | 04/23/2024 - 04/25/2024 |
| [Adobe Pass Authentication 2.69發行說明](notes-releases/auth-rn-269.md) | 02/27/2024 - 02/29/2024 |
| [Adobe Pass Authentication JavaScript SDK 4.7.0發行說明](notes-releases/authn-rn-javascript-470.md) | 02/27/2024 - 02/29/2024 |
| [Adobe Pass Authentication iOS / tvOS SDK 3.9.2發行說明](notes-releases/authn-rn-ios-tvos-392.md) | 03/26/2024 |
| [Adobe Pass Authentication iOS / tvOS SDK 3.8.4發行說明](notes-releases/authn-rn-ios-tvos-384.md) | 01/26/2024 |

### 2023 {#product-releases-2023}

| 發行說明 | 日期 |
|---------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass Authentication 2.68發行說明](notes-releases/auth-rn-268.md) | 12/05/2023 - 12/07/2023 |
| [Adobe Pass Authentication 2.67發行說明](notes-releases/auth-rn-267.md) | 09/12/2023 - 09/14/2023 |
| [Adobe Pass Authentication 2.66發行說明](notes-releases/auth-rn-266.md) | 07/11/2023 - 07/13/2023 |
| [Adobe Pass Authentication 2.65.1發行說明](notes-releases/auth-rn-2651.md) | 06/20/2023 - 06/22/2023 |
| [Adobe Pass Authentication 2.65發行說明](notes-releases/auth-rn-265.md) | 25/04/2023 - 27/04/2023 |
| [Adobe Pass Authentication 2.64.1發行說明](notes-releases/auth-rn-2641.md) | 01/31/2023 - 02/02/2023 |
| [Adobe Pass Authentication iOS / tvOS SDK 3.8.3發行說明](notes-releases/authn-rn-ios-tvos-383.md) | 11/16/2023 |
| [Adobe Pass Authentication iOS / tvOS SDK 3.8.2發行說明](notes-releases/authn-rn-ios-tvos-382.md) | 02/10/2023 |
| [Adobe Pass Authentication iOS / tvOS SDK 3.8.1發行說明](notes-releases/authn-rn-ios-tvos-381.md) | 26/05/2023 |
| [Adobe Pass Authentication Android SDK 3.7.3發行說明](notes-releases/authn-rn-android-373.md) | 09/19/2023 |

### 2022 {#product-releases-2022}

| 發行說明 | 日期 |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Adobe Pass Authentication 2.64發行說明](notes-releases/auth-rn-264.md) | 11/08/2022 - 11/10/2022 |
| [Adobe Pass Authentication 2.63發行說明](notes-releases/auth-rn-263.md) | 09/20/2022 - 09/22/2022 |
| [Adobe Pass Authentication 2.62.1發行說明](notes-releases/auth-rn-2621.md) | 08/02/2022 - 08/04/2022 |
| [Adobe Pass Authentication JavaScript SDK 4.6.0發行說明](notes-releases/authn-rn-javascript-460.md) | 09/20/2022 - 09/22/2022 |

### 2021 {#product-releases-2021}

| 發行說明 | 日期 |
|-----------------------------------------------------------------------------------------------------------|------------|
| [Adobe Pass Authentication JavaScript SDK 4.4.0發行說明](notes-releases/authn-rn-javascript-440.md) | 06/22/2021 |
| [Adobe Pass Authentication iOS / tvOS SDK 3.7.0發行說明](notes-releases/authn-rn-ios-tvos-370.md) | 09/03/2021 |
