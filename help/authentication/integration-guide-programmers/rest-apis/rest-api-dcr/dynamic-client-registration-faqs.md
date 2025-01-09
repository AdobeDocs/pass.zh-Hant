---
title: Dynamic Client Registration (DCR)常見問題集
description: Dynamic Client Registration (DCR)常見問題集
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Dynamic Client Registration (DCR)常見問題集 {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

本檔案提供有關Adobe Pass驗證動態使用者端註冊(DCR)採用之常見問題的高概觀回答。

如需整體動態使用者端註冊(DCR)的詳細資訊，請參閱[動態使用者端註冊概觀](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)檔案。

>[!MORELIKETHIS]
>
> * [REST API v2常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)

## 一般常見問答 {#general-faqs}

如果您正在處理需要整合「動態使用者端註冊」(DCR)的應用程式，不論是新應用程式還是從舊有機制移轉的現有應用程式，請由本節開始。

### 註冊階段常見問題集 {#registration-phase-faqs-general}

+++註冊階段常見問題集

#### 1.註冊階段的用途為何？ {#registration-phase-faq1}

註冊階段的目的是透過[動態使用者端註冊(DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr)程式，針對Adobe Pass驗證註冊使用者端應用程式。

動態使用者端註冊(DCR)程式要求使用者端應用程式取得一對使用者端憑證，並擷取存取權杖作為註冊階段的最終目標。

如需詳細資訊，請參閱[動態使用者端註冊概觀](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)檔案。

#### 2. 「註冊階段」是否為必要？ {#registration-phase-faq2}

註冊階段是強制性的，但如果使用者端應用程式具有快取的使用者端憑證對和仍然有效的存取權杖，則可以跳過此階段。

#### 3.什麼是軟體陳述式及其有效期限？ {#registration-phase-faq3}

軟體陳述式是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement)檔案中定義的辭彙。

軟體陳述式包含JSON Web權杖(JWT)，可由您的組織管理員或代表您行事的Adobe Pass驗證代表從Adobe Pass [TVE儀表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)產生及下載。

軟體陳述式的有效期限不受限制，但您隨時可以選擇要求Adobe Pass驗證代表撤銷該陳述式。

使用者端應用程式必須儲存軟體陳述式，並在需要擷取使用者端憑證時使用它。

如需詳細資訊，請參閱[Dynamic Client註冊概觀](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)檔案。

#### 4.如何產生及下載軟體陳述式？ {#registration-phase-faq4}

您的組織管理員或代表您行事的Adobe Pass驗證代表可透過Adobe Pass [TVE控制面板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)完成此作業。

如需詳細資訊，請參閱[TVE儀表板頻道使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications)或[TVE儀表板程式設計師使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications)檔案。

#### 5.如果軟體陳述式被撤銷，會發生什麼情況？ {#registration-phase-faq5}

撤銷軟體宣告時，請考量下列其中一個重要後果：

* 使用已撤銷軟體宣告的使用者端應用程式將無法再透過[軟體權利檔案](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement)流程，這表示使用者將無法播放內容。

#### 6.什麼是使用者端認證，其有效期限為多久？ {#registration-phase-faq6}

使用者端認證是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials)檔案中定義的辭彙。

使用者端認證包含使用者端識別碼和使用者端密碼組，可從「使用者端註冊」端點擷取。

使用者端憑證的有效時間範圍不受限制。

使用者端應用程式必須無限期儲存使用者端憑證，並在需要擷取存取權杖時使用它們。

如需詳細資訊，請參閱[擷取使用者端認證](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)檔案。

#### 7.如何管理使用者端憑證？ {#registration-phase-faq7}

建議使用者端應用程式在與Adobe Pass驗證的使用者端對伺服器和伺服器對伺服器整合時，為每個使用者應用程式執行個體管理一組唯一的使用者端認證。

使用者端應用程式必須無限期儲存使用者端憑證，並在需要擷取存取權杖時使用它們。

#### 8.如果快取的使用者端憑證遺失，會發生什麼情況？ {#registration-phase-faq8}

當快取的使用者端憑證遺失時，需要考慮三個重要後果：

* 使用者端應用程式必須取得一組新的使用者端認證。
* 使用者端應用程式必須使用新的使用者端憑證組來取得新的存取權杖。
* 使用者端應用程式將需要要求使用者重新驗證，因為使用者端應用程式將失去對先前取得的已驗證設定檔的存取權。

#### 9.什麼是存取權杖，其有效時間為何？ {#registration-phase-faq9}

存取權杖是[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token)檔案中定義的辭彙。

存取權杖包含可從使用者端權杖端點擷取的[持有人權杖](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)。

存取權杖在問題發生時指定的有限和較短時間範圍內有效。

使用者端應用程式必須儲存存取權杖並使用它，直到它定位REST API V2時過期。

使用者端應用程式必須在目前的存取權杖過期之前取得新的存取權杖，以防止未獲授權的請求。

如需詳細資訊，請參閱[擷取存取Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)檔案。

#### 10.使用者端應用程式如何重新整理存取權杖？ {#registration-phase-faq10}

使用者端應用程式必須重新整理存取權杖，其方式與擷取新存取權杖相同，但使用快取的使用者端認證。

使用者端應用程式不可重新註冊以重新整理存取權杖，而是必須使用儲存的使用者端認證，否則使用者必須重新驗證。

如需詳細資訊，請參閱[擷取存取Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)檔案。
