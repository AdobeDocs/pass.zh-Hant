---
title: REST API V2常見問題集
description: REST API V2常見問題集
source-git-commit: 1370554c66116a357970fb05c046608e261f0ed3
workflow-type: tm+mt
source-wordcount: '7304'
ht-degree: 0%

---

# REST API V2常見問題集 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

本檔案提供與Adobe Pass驗證REST API V2採用相關常見問題的高度概觀解答。

如需整體REST API V2的詳細資訊，請參閱[REST API V2概觀](./rest-api-v2-overview.md)檔案。

## 一般常見問答 {#general-faqs}

如果您正在處理需要整合REST API V2的應用程式，無論是新應用程式或從[REST API V1](#migrate-rest-api-v1-to-rest-api-v2)或[SDK](#migrate-sdk-to-rest-api-v2)移轉的現有應用程式，請由此章節開始。

如需移轉詳細資訊和步驟的詳細資訊，請參閱下一節。

+++註冊階段常見問題集

### 1.註冊階段的用途為何？ {#registration-phase-faq1}

註冊階段的目的是透過[動態使用者端註冊(DCR)](./rest-api-v2-glossary.md#dcr)程式，針對Adobe Pass驗證註冊使用者端應用程式。

動態使用者端註冊(DCR)程式要求使用者端應用程式取得一對使用者端憑證，並擷取存取權杖作為註冊階段的最終目標。

如需詳細資訊，請參閱[動態使用者端註冊概觀](/help/authentication/dcr-api/dynamic-client-registration-overview.md)檔案。

### 2. 「註冊階段」是否為必要？ {#registration-phase-faq2}

註冊階段是強制性的，但如果使用者端應用程式具有快取的使用者端憑證對和仍然有效的存取權杖，則可以跳過此階段。

### 3.什麼是軟體陳述式及其有效期限？ {#registration-phase-faq3}

軟體陳述式是在[字彙表](./rest-api-v2-glossary.md#software-statement)檔案中定義的辭彙。

軟體陳述式包含JSON Web權杖(JWT)，可由您的組織管理員或代表您行事的Adobe Pass驗證代表從Adobe Pass [TVE儀表板](./rest-api-v2-glossary.md#tve-dashboard)產生及下載。

軟體陳述式的有效期限不受限制，但您隨時可以選擇要求Adobe Pass驗證代表撤銷該陳述式。

使用者端應用程式必須儲存軟體陳述式，並在需要擷取使用者端憑證時使用它。

如需詳細資訊，請參閱[Dynamic Client註冊概觀](/help/authentication/dcr-api/dynamic-client-registration-overview.md)檔案。

### 4.如何產生及下載軟體陳述式？ {#registration-phase-faq4}

您的組織管理員或代表您行事的Adobe Pass驗證代表可透過Adobe Pass [TVE控制面板](./rest-api-v2-glossary.md#tve-dashboard)完成此作業。

如需詳細資訊，請參閱[TVE儀表板頻道使用手冊](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications)或[TVE儀表板程式設計師使用手冊](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications)檔案。

### 5.如果軟體陳述式被撤銷，會發生什麼情況？ {#registration-phase-faq5}

撤銷軟體宣告時，請考量下列其中一個重要後果：

* 使用已撤銷軟體宣告的使用者端應用程式將無法再透過[軟體權利檔案](./rest-api-v2-glossary.md#entitlement)流程，這表示使用者將無法播放內容。

### 6.什麼是使用者端認證，其有效期限為多久？ {#registration-phase-faq6}

使用者端認證是在[字彙表](./rest-api-v2-glossary.md#client-credentials)檔案中定義的辭彙。

使用者端認證包含使用者端識別碼和使用者端密碼組，可從「使用者端註冊」端點擷取。

使用者端憑證的有效時間範圍不受限制。

使用者端應用程式必須無限期儲存使用者端憑證，並在需要擷取存取權杖時使用它們。

如需詳細資訊，請參閱[擷取使用者端認證](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)檔案。

### 7.如何管理使用者端憑證？ {#registration-phase-faq7}

建議使用者端應用程式在與Adobe Pass驗證的使用者端對伺服器和伺服器對伺服器整合時，為每個使用者應用程式執行個體管理一組唯一的使用者端認證。

使用者端應用程式必須無限期儲存使用者端憑證，並在需要擷取存取權杖時使用它們。

### 8.如果快取的使用者端憑證遺失，會發生什麼情況？ {#registration-phase-faq8}

當快取的使用者端憑證遺失時，需要考慮三個重要後果：

* 使用者端應用程式必須取得一組新的使用者端認證。
* 使用者端應用程式必須使用新的使用者端憑證組來取得新的存取權杖。
* 使用者端應用程式將需要要求使用者重新驗證，因為使用者端應用程式將失去對先前取得的已驗證設定檔的存取權。

### 9.什麼是存取權杖，其有效時間為何？ {#registration-phase-faq9}

存取權杖是[字彙表](./rest-api-v2-glossary.md#access-token)檔案中定義的辭彙。

存取權杖包含可從使用者端權杖端點擷取的[持有人權杖](./appendix/headers/rest-api-v2-appendix-headers-authorization.md)。

存取權杖在問題發生時指定的有限和較短時間範圍內有效。

使用者端應用程式必須儲存存取權杖並使用它，直到它定位REST API V2時過期。

使用者端應用程式必須在目前的存取權杖過期之前取得新的存取權杖，以防止未獲授權的請求。

如需詳細資訊，請參閱[擷取存取Token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)檔案。

### 10.使用者端應用程式如何重新整理存取權杖？ {#registration-phase-faq10}

使用者端應用程式必須重新整理存取權杖，其方式與擷取新存取權杖相同，但使用快取的使用者端認證。

使用者端應用程式不可重新註冊以重新整理存取權杖，而是必須使用儲存的使用者端認證，否則使用者必須重新驗證。

如需詳細資訊，請參閱[擷取存取Token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)檔案。

+++

+++設定階段常見問題集

### 1.組態階段的用途為何？ {#configuration-phase-faq1}

「組態階段」的目的是提供使用者端應用程式主動整合的MVPD清單，以及Adobe Pass驗證針對每個MVPD儲存的組態詳細資訊。

當使用者端應用程式需要要求使用者選取他們的電視提供者時，「組態階段」會成為「驗證階段」的先決條件步驟。

### 2.設定階段是否為必要階段？ {#configuration-phase-faq2}

「組態階段」不是強制性的，在下列情況下，使用者端應用程式可以略過此階段：

* 使用者已經過驗證。
* 透過基本或促銷的TempPass功能，提供使用者暫時存取許可權。
* 使用者驗證已過期，但使用者端應用程式已快取先前選取的MVPD做為使用者體驗動機的選擇，而且只是提示使用者確認他們仍然是該MVPD的訂閱者。

### 3.什麼是設定？其有效期為多久？ {#configuration-phase-faq3}

設定是在[字彙表](./rest-api-v2-glossary.md#configuration)檔案中定義的辭彙。

組態包含可從組態端點擷取的MVPD清單。

使用者端應用程式可使用設定，在要求使用者選取其MVPD時，呈現稱為「選取器」的UI元件。

使用者端應用程式應在使用者完成驗證階段之前重新整理設定。

如需詳細資訊，請參閱[擷取組態](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)檔案。

### 4.使用者端應用程式可以管理自己的MVPD清單嗎？ {#configuration-phase-faq4}

使用者端應用程式可以管理自己的MVPD清單，但建議您使用Adobe Pass驗證提供的設定，以確保清單是最新且正確的。

如果使用者選取的MVPD沒有與指定的[服務提供者](./rest-api-v2-glossary.md#service-provider)的有效整合，使用者端應用程式將會從Adobe Pass Authentication REST API V2收到[錯誤](/help/authentication/enhanced-error-codes.md)。

### 5.使用者端應用程式可以篩選MVPD清單嗎？ {#configuration-phase-faq5}

使用者端應用程式可以根據自己的商業邏輯和需求（例如使用者位置或先前選取的使用者歷史記錄），實作自訂機制，以篩選設定回應中提供的MVPD清單。

### 6.如果與MVPD的整合已停用並標籤為非使用中，會發生什麼情況？ {#configuration-phase-faq6}

當與MVPD的整合被停用並標籤為非使用中時，MVPD會從其他組態回應中提供的MVPD清單中移除，而且有兩個重要的結果需要考慮：

* 該MVPD的未驗證使用者將無法再使用該MVPD完成驗證階段。
* 該MVPD的已驗證使用者將無法再使用該MVPD完成預先授權、授權或登出階段。

如果使用者選取的MVPD不再與指定的[服務提供者](./rest-api-v2-glossary.md#service-provider)有效整合，使用者端應用程式將會從Adobe Pass Authentication REST API V2收到[錯誤](/help/authentication/enhanced-error-codes.md)。

### 7.如果與MVPD的整合已重新啟用並標示為使用中，會發生什麼情況？ {#configuration-phase-faq7}

當與MVPD的整合重新啟用並標示為作用中時，MVPD會重新納入其他組態回應中提供的MVPD清單中，而且有兩個重要的結果需要考慮：

* 該MVPD的未驗證使用者將能夠再次使用該MVPD完成「驗證階段」。
* 該MVPD的已驗證使用者將能夠使用該MVPD再次完成預先授權、授權或登出階段。

### 8.如何啟用或停用與MVPD的整合？ {#configuration-phase-faq8}

您的組織管理員或代表您行事的Adobe Pass驗證代表可透過Adobe Pass [TVE控制面板](./rest-api-v2-glossary.md#tve-dashboard)完成此作業。

如需詳細資訊，請參閱[TVE儀表板整合使用手冊](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#disable-integration)檔案。

+++

+++驗證階段常見問題集

### 1.驗證階段的用途為何？ {#authentication-phase-faq1}

「驗證階段」的目的是讓使用者端應用程式能夠驗證使用者的身分並取得使用者中繼資料資訊。

當使用者端應用程式需要播放內容時，「驗證階段」會成為「預先授權階段」或「授權階段」的先決條件步驟。

### 2.使用者端應用程式如何知道使用者是否已驗證？ {#authentication-phase-faq2}

使用者端應用程式可以查詢下列其中一個端點，該端點能夠驗證使用者是否已通過驗證並傳回設定檔資訊：

* [設定檔端點API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的設定檔端點](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（驗證）程式碼API的設定檔端點](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

如需詳細資訊，請參閱下列檔案：

* [主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 3.使用者端應用程式如何取得使用者的中繼資料資訊？ {#authentication-phase-faq3}

使用者端應用程式可以查詢下列端點之一，這些端點能夠傳回[使用者中繼資料](/help/authentication/user-metadata-feature.md)資訊作為設定檔資訊的一部分：

* [設定檔端點API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的設定檔端點](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（驗證）程式碼API的設定檔端點](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

如需詳細資訊，請參閱下列檔案：

* [主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

### 4.什麼是驗證工作階段？其有效期為多久？ {#authentication-phase-faq4}

驗證工作階段是在[字彙表](./rest-api-v2-glossary.md#session)檔案中定義的辭彙。

驗證工作階段會儲存可從「工作階段」端點擷取的已起始驗證程式相關資訊。

驗證工作階段在問題時指定的有限和較短時間範圍內有效，這表示在要求重新啟動流程之前，使用者必須完成驗證流程的時間量。

使用者端應用程式可使用驗證工作階段回應，以瞭解如何進行驗證程式。 請注意，在某些情況下，使用者不需要驗證，例如提供暫時存取、存取效能降低，或使用者已經驗證。

如需詳細資訊，請參閱下列檔案：

* [建立驗證工作階段API](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [繼續驗證工作階段API](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [主要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 5.什麼是驗證碼？其有效期為多久？ {#authentication-phase-faq5}

驗證代碼是[字彙表](./rest-api-v2-glossary.md#code)檔案中定義的辭彙。

驗證代碼會儲存使用者啟動驗證程式時產生的唯一值，並唯一識別使用者的驗證工作階段，直到程式完成或驗證工作階段過期為止。

驗證代碼在起始驗證工作階段時指定的有限和較短時間範圍內有效，這表示在要求重新啟動流程之前，使用者必須完成驗證流程的時間量。

使用者端應用程式可使用驗證代碼，讓使用者在相同裝置或第二部裝置上完成或繼續驗證程式，因為驗證工作階段並未過期。

如需詳細資訊，請參閱下列檔案：

* [建立驗證工作階段API](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [繼續驗證工作階段API](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [主要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

### 6.使用者端應用程式如何知道使用者是否輸入有效的驗證代碼，以及驗證工作階段是否尚未過期？ {#authentication-phase-faq6}

使用者端應用程式可以傳送要求給負責擷取與驗證代碼關聯的驗證工作階段資訊的「工作階段」端點，來驗證使用者在次要（熒幕）應用程式中輸入的驗證代碼。

如果提供的驗證碼輸入錯誤或驗證工作階段過期，使用者端應用程式會收到[錯誤](/help/authentication/enhanced-error-codes.md)。

如需詳細資訊，請參閱[擷取驗證工作階段](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)檔案。

### 7.設定檔是什麼？其有效期為多久？ {#authentication-phase-faq7}

設定檔是在[字彙表](./rest-api-v2-glossary.md#profile)檔案中定義的辭彙。

設定檔會儲存有關使用者驗證有效性的資訊、中繼資料資訊以及可從「設定檔」端點擷取的更多資訊。

使用者端應用程式可使用設定檔來瞭解使用者的驗證狀態、存取使用者中繼資料資訊或尋找用於驗證的方法。

如需詳細資訊，請參閱下列檔案：

* [設定檔端點API](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的設定檔端點](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（驗證）程式碼API的設定檔端點](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

在查詢時指定的有限時間範圍內，設定檔有效，這表示在需要再次通過驗證階段之前，使用者已具有有效驗證的時間量。

這個有限的時間範圍稱為驗證(authN) [TTL](./rest-api-v2-glossary.md#ttl)，可由您的組織管理員或代表您行事的Adobe Pass驗證代表透過Adobe Pass [TVE儀表板](./rest-api-v2-glossary.md#tve-dashboard)檢視及變更。

如需詳細資訊，請參閱[TVE儀表板整合使用手冊](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)檔案。

+++

+++預先授權階段常見問題集

### 1.預先授權階段的用途為何？ {#preauthorization-phase-faq1}

預先授權階段的目的是讓使用者端應用程式能夠在其目錄中呈現使用者有權存取的資源子集。

當使用者第一次開啟使用者端應用程式或導覽至新區段時，「預先授權階段」可以增強使用者體驗。

### 2.預先授權階段是否為必要階段？ {#preauthorization-phase-faq2}

預先授權階段並非強制性，如果使用者端應用程式想要呈現資源目錄而不先根據使用者的許可權進行篩選，則可跳過此階段。

### 3.什麼是預先授權決定？ {#preauthorization-phase-faq3}

預先授權是在[字彙表](./rest-api-v2-glossary.md#preauthorization)檔案中定義的字詞，而決定字詞也可在[字彙表](./rest-api-v2-glossary.md#decision)中找到。

預先授權決定儲存有關MVPD預先授權處理查詢結果的資訊，這些資訊可從「決定預先授權」端點擷取。

使用者端應用程式可以使用預先授權決定，呈現電視提供者（資訊性）決定可讓使用者存取的資源子集。

如需詳細資訊，請參閱下列檔案：

* [擷取預先授權決定API](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [主要應用程式內執行的基本預先授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

### 4.為何預先授權決定遺失媒體權杖？ {#preauthorization-phase-faq4}

預先授權決定遺失媒體權杖，因為預先授權階段不能用於播放資源，因為這是授權階段的用途。

### 5.什麼是資源？支援哪些格式？ {#preauthorization-phase-faq5}

資源是在[字彙表](./rest-api-v2-glossary.md#resource)檔案中定義的辭彙。

資源是與MVPD議定的唯一識別碼，並與使用者端應用程式可串流的內容相關聯。

資源唯一識別碼可以有兩種格式：

* 簡單的字串格式，例如管道（品牌）的唯一識別碼。
* 包含其他資訊的媒體RSS (MRSS)格式，例如標題、分級和家長監護中繼資料。

如需詳細資訊，請參閱[識別受保護的資源](/help/authentication/identify-protected-resources.md)檔案。

### 6.使用者端應用程式一次可取得多少資源預先授權決定？ {#preauthorization-phase-faq6}

由於MVPD施加的條件，使用者端應用程式可以在單一API要求中取得有限數量資源的預先授權決定，通常最多5個。

您的組織管理員或代表您行事的Adobe Pass驗證代表透過Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard)同意MVPD後，可以檢視和變更此最大資源數量。

如需詳細資訊，請參閱[TVE儀表板整合使用手冊](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#add-more-properties)檔案。

+++

+++授權階段常見問題集

### 1.授權階段的用途為何？ {#authorization-phase-faq1}

「授權階段」的目的是讓使用者端應用程式在MVPD上驗證使用者的許可權後，能夠播放使用者要求的資源。

### 2.授權階段是否為必要階段？ {#authorization-phase-faq2}

授權階段是強制性的，如果客戶端應用程式想要播放使用者請求的資源，則無法跳過此階段，因為它需要先與MVPD驗證使用者是否有權釋放資料流。

### 3.什麼是授權決定？其有效期為多久？ {#authorization-phase-faq3}

授權是在[字彙表](./rest-api-v2-glossary.md#authorization)檔案中定義的字詞，而決定字詞也可在[字彙表](./rest-api-v2-glossary.md#decision)中找到。

授權決定儲存有關MVPD授權處理查詢結果的資訊，這些資訊可從Decisions Authorize端點擷取。

使用者端應用程式可以使用包含媒體權杖的授權決定來播放資源資料流，以備電視提供者（授權）決定允許使用者存取該資料流時使用。

如需詳細資訊，請參閱下列檔案：

* [擷取授權決定API](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [主要應用程式內執行的基本授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

授權決定適用於問題時指定的有限和較短時間範圍，表示在需要再次查詢MVPD之前，Adobe Pass驗證將快取該決定的時間量。

這個有限的時間範圍稱為授權(authZ) [TTL](./rest-api-v2-glossary.md#ttl)，可由您的組織管理員或代表您行事的Adobe Pass驗證代表透過Adobe Pass [TVE儀表板](./rest-api-v2-glossary.md#tve-dashboard)檢視及變更。

如需詳細資訊，請參閱[TVE儀表板整合使用手冊](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)檔案。

### 4.什麼是媒體代號？其有效期為多久？ {#authorization-phase-faq4}

媒體權杖是[字彙表](./rest-api-v2-glossary.md#media-token)檔案中定義的辭彙。

媒體權杖包含以純文字傳送的已簽署字串，可從「決定授權」端點擷取。

如需詳細資訊，請參閱[整合媒體權杖驗證器](/help/authentication/media-token-verifier-int.md)檔案。

媒體權杖在問題時指定的有限和較短時間範圍內有效，這表示在需要再次查詢Decisions Authorize端點之前，使用者端應用程式必須使用此權杖的時間量。

使用者端應用程式可以使用媒體權杖來播放資源串流，以備電視提供者（權威）決定允許使用者存取它時使用。

如需詳細資訊，請參閱下列檔案：

* [擷取授權決定API](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [主要應用程式內執行的基本授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

### 5.什麼是資源？支援哪些格式？ {#authorization-phase-faq5}

資源是在[字彙表](./rest-api-v2-glossary.md#resource)檔案中定義的辭彙。

資源是與MVPD議定的唯一識別碼，並與使用者端應用程式可串流的內容相關聯。

資源唯一識別碼可以有兩種格式：

* 簡單的字串格式，例如管道（品牌）的唯一識別碼。
* 包含其他資訊的媒體RSS (MRSS)格式，例如標題、分級和家長監護中繼資料。

如需詳細資訊，請參閱[識別受保護的資源](/help/authentication/identify-protected-resources.md)檔案。

### 6.使用者端應用程式一次可取得多少資源授權決定？ {#authorization-phase-faq6}

由於MVPD施加的條件，使用者端應用程式可以在單一API要求中取得有限資源數的授權決定，通常最多1個。

+++

+++登出階段常見問題集

### 1.登出階段的用途為何？ {#logout-phase-faq1}

登出階段的目的是讓使用者端應用程式能夠應使用者要求，在Adobe Pass驗證中終止使用者的已驗證設定檔。

+++

+++標頭常見問題集

### 1.如何計算Authorization標頭的值？ {#headers-faq1}

[Authorization](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)請求標頭包含使用者端應用程式存取受Adobe Pass保護的API所需的`Bearer`存取權杖。

在註冊階段期間，必須從Adobe Pass驗證取得授權標頭值。

如需詳細資訊，請參閱下列檔案：

* [Dynamic Client註冊概述](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [擷取使用者端認證API](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [擷取存取Token API](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [動態使用者端註冊流程](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

如果使用者端應用程式從REST API V1移轉至REST API V2，使用者端應用程式可以繼續使用相同的方法取得`Bearer`存取權杖。

### 2.如何計算AP-Device-Identifier標頭的值？ {#headers-faq2}

[AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)要求標頭包含由使用者端應用程式建立的串流裝置識別碼。

[AP-Device-Identifier](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)標標頭檔案提供了一些計算不同平台值的範例，但使用者端應用程式可以根據自己的商業邏輯和需求選擇使用不同的方法。

萬一使用者端應用程式從REST API V1移轉至REST API V2，使用者端應用程式可繼續使用與先前相同的方法運算裝置識別碼。

### 3.如何計算X-Device-Info標頭的值？ {#headers-faq3}

[X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)要求標頭包含與實際串流裝置相關的使用者端資訊（裝置、連線和應用程式）。

[X-Device-Info](/help/authentication/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)標標頭檔案提供一些計算不同平台之值的範例，但使用者端應用程式可以根據自己的商業邏輯和需求，選擇使用不同的方法。

萬一使用者端應用程式從REST API V1移轉至REST API V2，使用者端應用程式可繼續使用與先前相同的方法運算裝置資訊。

+++

## 移轉常見問題集 {#migration-faqs}

如果您使用的應用程式需要將現有應用程式移轉至REST API V2，請繼續本節內容。

+++一般移轉常見問題集

### 1.需要我一次向所有使用者推出移轉至REST API V2的新使用者端應用程式嗎？ {#migration-faq1}

不適用。

使用者端應用程式不需要同時推出整合REST API V2的新版本給所有使用者。

到2025年底，Adobe Pass驗證將繼續支援整合REST API V1或SDK的較舊使用者端應用程式版本。

### 2.需要我一次在所有API和流程中推出移轉至REST API V2的新使用者端應用程式嗎？ {#migration-faq2}

是。

使用者端應用程式需要同時推出跨所有Adobe Pass驗證API和流程整合REST API V2的新版本。

在「第二個熒幕驗證」流程中，使用者端應用程式必須同時針對[主要](./rest-api-v2-glossary.md#primary-application)和[次要](./rest-api-v2-glossary.md#secondary-application)應用程式推出整合REST API V2的新版本。

Adobe Pass驗證將不支援在API和流程之間整合REST API V2和REST API V1/SDK的「混合」實作。

### 3.更新至移轉至REST API V2的新使用者端應用程式時，是否會保留使用者驗證？ {#migration-faq3}

不適用。

在整合REST API V1或SDK的舊版使用者端應用程式中取得的使用者驗證將不會保留。

因此，使用者必須在移轉至REST API V2的新使用者端應用程式中重新驗證。

### 4.使用者端應用程式可以使用現有的註冊應用程式（軟體陳述式）嗎？ {#migration-faq4}

使用者端應用程式無法重複使用現有的註冊應用程式（軟體陳述式），因此必須產生並下載專用於使用REST API V2的新註冊應用程式（軟體陳述式）。

您的組織管理員或代表您行事的Adobe Pass驗證代表可透過Adobe Pass [TVE控制面板](./rest-api-v2-glossary.md#tve-dashboard)完成此作業。

如需詳細資訊，請參閱[TVE儀表板頻道使用手冊](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#registered-applications)或[TVE儀表板程式設計師使用手冊](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#registered-applications)檔案。

目前您必須要求Adobe Pass驗證代表啟用新註冊應用程式（軟體陳述式）的REST API V2使用方式，直到Adobe Pass [TVE Dashboard](./rest-api-v2-glossary.md#tve-dashboard)更新為允許此作業自我管理為止。

為了區分使用REST API V2的使用者端應用程式中使用的已註冊應用程式（軟體陳述式），我們要求您將特定的尾碼新增到已註冊應用程式名稱中，例如「RESTV2」。

### 5.使用者端應用程式可以使用現有的自訂配置嗎？ {#migration-faq5}

使用者端應用程式可重複使用透過Adobe Pass [TVE儀表板](./rest-api-v2-glossary.md#tve-dashboard)產生的現有自訂配置。

如需詳細資訊，請參閱[TVE儀表板頻道使用手冊](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#custom-schemes)或[TVE儀表板程式設計師使用手冊](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md#custom-schemes)檔案。

### 6. REST API V2是否預設啟用增強式錯誤代碼？ {#migration-faq6}

是。

整合REST API V2的使用者端應用程式受益於預設啟用的增強錯誤代碼功能。

如需詳細資訊，請參閱[增強錯誤碼](/help/authentication/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)檔案。

+++

### 從REST API V1移轉至REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

如果您正在處理需要從REST API V1移轉至REST API V2的應用程式，請繼續此子區段。

+++註冊階段常見問題集

#### 1.註冊階段需要哪些高階API移轉？ {#registration-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，註冊階段沒有高層級變更。

使用者端應用程式可以繼續使用相同的流程，透過[動態使用者端註冊(DCR)](./rest-api-v2-glossary.md#dcr)程式針對Adobe Pass驗證進行註冊，並取得存取權杖。

如需詳細資訊，請參閱下列檔案：

* [Dynamic Client註冊概述](/help/authentication/dcr-api/dynamic-client-registration-overview.md)
* [擷取使用者端認證API](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [擷取存取Token API](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [動態使用者端註冊流程](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)

+++

+++設定階段常見問題集

#### 1.設定階段需要哪些高階API移轉？ {#configuration-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|------------------------------------------------|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [GET<br/> /api/v1/config/{serviceProvider}](/help/authentication/provide-mvpd-list.md) | [GET<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++驗證階段常見問題集

#### 1.驗證階段需要哪些高階API移轉？ {#authentication-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取註冊代碼（驗證代碼） | [POST<br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/registration-code-request.md) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查註冊代碼（驗證代碼） | [GET<br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二個熒幕（應用程式）上繼續(MVPD)驗證 | [GET<br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 起始(MVPD)驗證 | [GET<br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [GET<br/> /api/v1/checkauthn （第一個畫面）](/help/authentication/check-authentication-token.md) <br/> [GET<br/> /api/v1/checkauthn （第二個畫面）](/help/authentication/check-authentication-flow-by-second-screen-web-app.md) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者驗證Token （設定檔） | [GET<br/> /api/v1/tokens/authn](/help/authentication/retrieve-authentication-token.md) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [GET<br/> /api/v1/tokens/usermetadata](/help/authentication/user-metadata.md) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++預先授權階段常見問題集

#### 1.預先授權階段需要哪些高階API移轉？ {#preauthorization-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [GET<br/> /api/v1/preauthorize （第一個畫面）](/help/authentication/retrieve-list-of-preauthorized-resources.md) <br/> [GET<br/> /api/v1/preauthorize （第二個畫面）](/help/authentication/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++授權階段常見問題集

#### 1.授權階段需要哪些高階API移轉？ {#authorization-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|-------------------------------------------------------|----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 起始(MVPD)授權 | [GET<br/> /api/v1/authorize](/help/authentication/initiate-authorization.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>起始(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 擷取授權Token （授權決定） | [GET<br/> /api/v1/tokens/authz](/help/authentication/retrieve-authorization-token.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>起始(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 擷取短授權Token （媒體Token） | [GET<br/> /api/v1/tokens/media](/help/authentication/obtain-short-media-token.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>起始(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++登出階段常見問題集

#### 1.登出階段需要哪些高階API移轉？ {#logout-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|-----------------|---------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動登出 | [GET<br/> /api/v1/logout](/help/authentication/initiate-logout.md) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### 從SDK移轉至REST API V2 {#migrate-sdk-to-rest-api-v2}

如果您正在處理需要從SDK移轉至REST API V2的應用程式，請繼續使用此子區段。

+++註冊階段常見問題集

#### 1.註冊階段需要哪些高階API移轉？ {#registration-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

##### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成動態使用者端註冊(DCR) | 提供軟體陳述式給建構函式 | [POST<br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET<br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[動態使用者端註冊概述](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[動態使用者端註冊流程](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成動態使用者端註冊(DCR) | 提供軟體陳述式給建構函式 | [POST<br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET<br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[動態使用者端註冊概述](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[動態使用者端註冊流程](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成動態使用者端註冊(DCR) | 提供軟體陳述式給建構函式 | [POST<br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET<br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[動態使用者端註冊概述](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[動態使用者端註冊流程](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成動態使用者端註冊(DCR) | 提供軟體陳述式給建構函式 | [POST<br/> /o/client/register](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET<br/> /o/client/token](/help/authentication/dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[動態使用者端註冊概述](/help/authentication/dcr-api/dynamic-client-registration-overview.md)</li><li>[動態使用者端註冊流程](/help/authentication/dcr-api/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

+++設定階段常見問題集

#### 1.設定階段需要哪些高階API移轉？ {#configuration-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

##### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------------------|--------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) | [GET<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) | [GET<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) | [GET<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

##### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

+++驗證階段常見問題集

#### 1.驗證階段需要哪些高階API移轉？ {#authentication-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

##### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 起始(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/javascript-sdk-api-reference.md#setSelProv) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/javascript-sdk-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/javascript-sdk-api-reference.md#getMeta) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler iOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 起始(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取註冊代碼（驗證代碼） | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查註冊代碼（驗證代碼） | [GET<br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二個熒幕（應用程式）上繼續(MVPD)驗證 | [GET<br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 起始(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/iostvos-sdk-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/iostvos-sdk-api-reference.md#getMeta) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 起始(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/android-sdk-api-reference.md#setSelectedProvider) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/android-sdk-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/android-sdk-api-reference.md#getMetadata) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取註冊代碼（驗證代碼） | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查註冊代碼（驗證代碼） | [GET<br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/return-registration-record.md) | [GET<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二個熒幕（應用程式）上繼續(MVPD)驗證 | [GET<br/> /api/v1/authenticate](/help/authentication/initiate-authentication.md) | [POST<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 起始(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/amazon-fireos-native-client-api-reference.md#getMetadata) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

+++預先授權階段常見問題集

#### 1.預先授權階段需要哪些高階API移轉？ {#preauthorization-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

##### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-javascript-sdk.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-ios-tvos-sdk.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/preauthorize-api-android-sdk.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| 範圍 | SDK | REST API V2 | 觀察 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

+++授權階段常見問題集

#### 1.授權階段需要哪些高階API移轉？ {#authorization-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

##### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取短授權Token （媒體Token） | [AccessEnabler.checkAuthorization](/help/authentication/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/javascript-sdk-api-reference.md#getAuthZ) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>起始(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取短授權Token （媒體Token） | [AccessEnabler.checkAuthorization](/help/authentication/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/iostvos-sdk-api-reference.md#getAuthZ) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>起始(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取短授權Token （媒體Token） | [AccessEnabler.checkAuthorization](/help/authentication/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/android-sdk-api-reference.md#getAuthZ) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>起始(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取短授權Token （媒體Token） | [AccessEnabler.checkAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) |              | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>起始(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

+++登出階段常見問題集

#### 1.登出階段需要哪些高階API移轉？ {#logout-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

##### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-----------------|-------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 啟動登出 | [AccessEnabler.logout](/help/authentication/javascript-sdk-api-reference.md#logout) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 啟動登出 | [AccessEnabler.logout](/help/authentication/iostvos-sdk-api-reference.md#logout) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-----------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 啟動登出 | [AccessEnabler.logout](/help/authentication/android-sdk-api-reference.md#logout) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

##### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-----------------|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 啟動登出 | [AccessEnabler.logout](/help/authentication/amazon-fireos-native-client-api-reference.md#logout) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) |              | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++