---
title: REST API V2常見問題集
description: REST API V2常見問題集
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 747c3d9b6de537be5e7e0a0244b2b301603d9b18
workflow-type: tm+mt
source-wordcount: '6460'
ht-degree: 0%

---

# REST API V2常見問題集 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

本檔案提供與Adobe Pass驗證REST API V2採用相關常見問題的高度概觀解答。

如需整體REST API V2的詳細資訊，請參閱[REST API V2概觀](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)檔案。

## 一般常見問答 {#general-faqs}

如果您正在處理需要整合REST API V2的應用程式，不論是新應用程式或從[REST API V1](#migration-rest-api-v1-to-rest-api-v2)或[SDK](#migration-sdk-to-rest-api-v2)移轉的現有應用程式，請由此區段開始。

如需移轉詳細資訊和步驟的詳細資訊，請參閱下一節。

>[!MORELIKETHIS]
>
> * [動態使用者端註冊(DCR)常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#general-faqs)

### 註冊階段常見問題集 {#registration-phase-faqs-general}

+++註冊階段常見問題集

請參閱[Dynamic Client Registration (DCR)常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs)檔案。

+++

### 設定階段常見問題集 {#configuration-phase-faqs-general}

+++設定階段常見問題集

#### 1.組態階段的用途為何？ {#configuration-phase-faq1}

「設定階段」的目的是提供使用者端應用程式主動整合的MVPD清單，以及Adobe Pass驗證針對每個MVPD儲存的設定詳細資訊。

當使用者端應用程式需要要求使用者選取他們的電視提供者時，「組態階段」會成為「驗證階段」的先決條件步驟。

#### 2.設定階段是否為必要階段？ {#configuration-phase-faq2}

「組態階段」不是強制性的，在下列情況下，使用者端應用程式可以略過此階段：

* 使用者已經過驗證。
* 透過基本或促銷的TempPass功能，提供使用者暫時存取許可權。
* 使用者驗證已過期，但使用者端應用程式已快取先前選取的MVPD作為使用者體驗動機的選擇，而且只是提示使用者確認他們仍然是該MVPD的訂閱者。

#### 3.什麼是設定？其有效期為多久？ {#configuration-phase-faq3}

設定是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration)檔案中定義的辭彙。

組態包含可從組態端點擷取的MVPD清單。

當要求使用者選取其MVPD時，使用者端應用程式可使用設定，呈現稱為「選擇器」的UI元件。

使用者端應用程式應在使用者完成驗證階段之前重新整理設定。

如需詳細資訊，請參閱[擷取組態](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)檔案。

#### 4.使用者端應用程式可以管理自己的MVPD清單嗎？ {#configuration-phase-faq4}

使用者端應用程式可以管理自己的MVPD清單，但建議您使用Adobe Pass驗證提供的設定，以確保清單是最新且正確的。

如果使用者選取的Adobe Pass沒有與指定的[服務提供者](rest-api-v2-glossary.md#service-provider)的有效整合，使用者端應用程式將會從MVPD Authentication REST API V2收到[錯誤](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)。

#### 5.使用者端應用程式可以篩選MVPD清單嗎？ {#configuration-phase-faq5}

使用者端應用程式可以根據自己的商業邏輯和需求（例如使用者位置或先前選取的使用者歷史記錄），實作自訂機制，以篩選設定回應中提供的MVPD清單。

#### 6.如果與MVPD的整合停用並標籤為非使用中，會發生什麼情況？ {#configuration-phase-faq6}

當與MVPD的整合停用並標籤為非使用中時，MVPD會從進一步設定回應中提供的MVPD清單中移除，且有兩個重要的後果需要考慮：

* 該MVPD未經驗證的使用者將無法再使用該MVPD完成驗證階段。
* 該MVPD的已驗證使用者將無法再使用該MVPD完成預先授權、授權或登出階段。

如果使用者選取的Adobe Pass不再與指定的[服務提供者](rest-api-v2-glossary.md#service-provider)有效整合，使用者端應用程式將會從MVPD Authentication REST API V2收到[錯誤](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)。

#### 7.如果已重新啟用與MVPD的整合，並標示為使用中，會發生什麼情況？ {#configuration-phase-faq7}

當與MVPD的整合重新啟用並標籤為作用中時，MVPD會重新納入進一步設定回應中提供的MVPD清單中，您需考慮兩個重要後果：

* 該MVPD的未經驗證使用者將能夠再次使用該MVPD完成驗證階段。
* 該MVPD的已驗證使用者將能夠使用該MVPD再次完成預先授權、授權或登出階段。

#### 8.如何啟用或停用與MVPD的整合？ {#configuration-phase-faq8}

您的組織管理員或代表您行事的Adobe Pass驗證代表可透過Adobe Pass [TVE控制面板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)完成此作業。

如需詳細資訊，請參閱[TVE儀表板整合使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration)檔案。

+++

### 驗證階段常見問題集 {#authentication-phase-faqs-general}

+++驗證階段常見問題集

#### 1.驗證階段的用途為何？ {#authentication-phase-faq1}

「驗證階段」的目的是讓使用者端應用程式能夠驗證使用者的身分並取得使用者中繼資料資訊。

當使用者端應用程式需要播放內容時，「驗證階段」會成為「預先授權階段」或「授權階段」的先決條件步驟。

#### 2.使用者端應用程式如何知道使用者是否已驗證？ {#authentication-phase-faq2}

使用者端應用程式可以查詢下列其中一個端點，該端點能夠驗證使用者是否已通過驗證並傳回設定檔資訊：

* [設定檔端點API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（驗證）程式碼API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

如需詳細資訊，請參閱下列檔案：

* [主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 3.使用者端應用程式如何取得使用者的中繼資料資訊？ {#authentication-phase-faq3}

使用者端應用程式可以查詢下列端點之一，這些端點能夠傳回[使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)資訊作為設定檔資訊的一部分：

* [設定檔端點API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（驗證）程式碼API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

如需詳細資訊，請參閱下列檔案：

* [主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 4.什麼是驗證工作階段？其有效期為多久？ {#authentication-phase-faq4}

驗證工作階段是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session)檔案中定義的辭彙。

驗證工作階段會儲存可從「工作階段」端點擷取的已起始驗證程式相關資訊。

驗證工作階段在問題時指定的有限和較短時間範圍內有效，這表示在要求重新啟動流程之前，使用者必須完成驗證流程的時間量。

使用者端應用程式可使用驗證工作階段回應，以瞭解如何進行驗證程式。 請注意，在某些情況下，使用者不需要驗證，例如提供暫時存取、存取效能降低，或使用者已經驗證。

如需詳細資訊，請參閱下列檔案：

* [建立驗證工作階段API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [繼續驗證工作階段API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 5.什麼是驗證碼？其有效期為多久？ {#authentication-phase-faq5}

驗證代碼是[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code)檔案中定義的辭彙。

驗證代碼會儲存使用者啟動驗證程式時產生的唯一值，並唯一識別使用者的驗證工作階段，直到程式完成或驗證工作階段過期為止。

驗證代碼在起始驗證工作階段時指定的有限和較短時間範圍內有效，這表示在要求重新啟動流程之前，使用者必須完成驗證流程的時間量。

使用者端應用程式可使用驗證代碼，讓使用者在相同裝置或第二部裝置上完成或繼續驗證程式，因為驗證工作階段並未過期。

如需詳細資訊，請參閱下列檔案：

* [建立驗證工作階段API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [繼續驗證工作階段API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 6.使用者端應用程式如何知道使用者是否輸入有效的驗證代碼，以及驗證工作階段是否尚未過期？ {#authentication-phase-faq6}

使用者端應用程式可以傳送要求給負責擷取與驗證代碼關聯的驗證工作階段資訊的「工作階段」端點，來驗證使用者在次要（熒幕）應用程式中輸入的驗證代碼。

如果提供的驗證碼輸入錯誤或驗證工作階段過期，使用者端應用程式會收到[錯誤](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)。

如需詳細資訊，請參閱[擷取驗證工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)檔案。

#### 7.設定檔是什麼？其有效期為多久？ {#authentication-phase-faq7}

設定檔是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile)檔案中定義的辭彙。

設定檔會儲存有關使用者驗證有效性的資訊、中繼資料資訊以及可從「設定檔」端點擷取的更多資訊。

使用者端應用程式可使用設定檔來瞭解使用者的驗證狀態、存取使用者中繼資料資訊或尋找用於驗證的方法。

如需詳細資訊，請參閱下列檔案：

* [設定檔端點API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（驗證）程式碼API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

在查詢時指定的有限時間範圍內，設定檔有效，這表示在需要再次通過驗證階段之前，使用者已具有有效驗證的時間量。

這個有限的時間範圍稱為驗證(authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl)，可由您的組織管理員或代表您行事的Adobe Pass驗證代表透過Adobe Pass [TVE儀表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)檢視及變更。

如需詳細資訊，請參閱[TVE儀表板整合使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)檔案。

+++

### 預先授權階段常見問題集 {#preauthorization-phase-faqs-general}

+++預先授權階段常見問題集

#### 1.預先授權階段的用途為何？ {#preauthorization-phase-faq1}

預先授權階段的目的是讓使用者端應用程式能夠在其目錄中呈現使用者有權存取的資源子集。

當使用者第一次開啟使用者端應用程式或導覽至新區段時，「預先授權階段」可以增強使用者體驗。

#### 2.預先授權階段是否為必要階段？ {#preauthorization-phase-faq2}

預先授權階段並非強制性，如果使用者端應用程式想要呈現資源目錄而不先根據使用者的許可權進行篩選，則可跳過此階段。

#### 3.什麼是預先授權決定？ {#preauthorization-phase-faq3}

預先授權是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization)檔案中定義的字詞，而決定字詞也可在[字彙表](rest-api-v2-glossary.md#decision)中找到。

預先授權決定儲存有關MVPD預先授權處理查詢結果的資訊，這些資訊可從「決定預先授權」端點擷取。

使用者端應用程式可以使用預先授權決定，呈現電視提供者（資訊性）決定可讓使用者存取的資源子集。

如需詳細資訊，請參閱下列檔案：

* [擷取預先授權決定API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### 4.為何預先授權決定遺失媒體權杖？ {#preauthorization-phase-faq4}

預先授權決定遺失媒體權杖，因為預先授權階段不能用於播放資源，因為這是授權階段的用途。

#### 5.什麼是資源？支援哪些格式？ {#preauthorization-phase-faq5}

資源是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource)檔案中定義的辭彙。

資源是與MVPD議定的唯一識別碼，並與使用者端應用程式可串流的內容相關聯。

資源唯一識別碼可以有兩種格式：

* 簡單的字串格式，例如管道（品牌）的唯一識別碼。
* 包含其他資訊的媒體RSS (MRSS)格式，例如標題、分級和家長監護中繼資料。

如需詳細資訊，請參閱[受保護的資源](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources)檔案。

#### 6.使用者端應用程式一次可取得多少資源預先授權決定？ {#preauthorization-phase-faq6}

由於MVPD施加的條件，使用者端應用程式可以在單一API要求中取得有限數量資源的預先授權決定，通常最多5個。

您的組織管理員或代表您行事的Adobe Pass驗證代表透過Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)同意MVPD後，可以檢視和變更此最大資源數量。

如需詳細資訊，請參閱[TVE儀表板整合使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties)檔案。

+++

### 授權階段常見問題集 {#authorization-phase-faqs-general}

+++授權階段常見問題集

#### 1.授權階段的用途為何？ {#authorization-phase-faq1}

授權階段的用途是，在使用者端應用程式透過MVPD驗證其許可權後，提供播放使用者要求的資源的功能。

#### 2.授權階段是否為必要階段？ {#authorization-phase-faq2}

授權階段是強制性的，如果客戶端應用程式想要播放使用者請求的資源，則無法跳過此階段，因為它需要在釋放資料流之前向MVPD驗證使用者是否有權使用。

#### 3.什麼是授權決定？其有效期為多久？ {#authorization-phase-faq3}

授權是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization)檔案中定義的字詞，而決定字詞也可在[字彙表](rest-api-v2-glossary.md#decision)中找到。

授權決定儲存有關MVPD授權程式查詢結果的資訊，這些資訊可從Decisions Authorize端點擷取。

使用者端應用程式可以使用包含媒體權杖的授權決定來播放資源資料流，以備電視提供者（授權）決定允許使用者存取該資料流時使用。

如需詳細資訊，請參閱下列檔案：

* [擷取授權決定API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

授權決定在問題時指定的有限和較短時間範圍內有效，這表示在需要再次查詢MVPD之前，Adobe Pass驗證將快取其的時間量。

這個有限的時間範圍稱為授權(authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl)，可由您的組織管理員或代表您行事的Adobe Pass驗證代表透過Adobe Pass [TVE儀表板](rest-api-v2-glossary.md#tve-dashboard)檢視及變更。

如需詳細資訊，請參閱[TVE儀表板整合使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)檔案。

#### 4.什麼是媒體代號？其有效期為多久？ {#authorization-phase-faq4}

媒體權杖是[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token)檔案中定義的辭彙。

媒體權杖包含以純文字傳送的已簽署字串，可從「決定授權」端點擷取。

如需詳細資訊，請參閱[媒體權杖驗證器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)檔案。

媒體權杖在問題時指定的有限和較短時間範圍內有效，這表示在需要再次查詢Decisions Authorize端點之前，使用者端應用程式必須使用此權杖的時間量。

使用者端應用程式可以使用媒體權杖來播放資源串流，以備電視提供者（權威）決定允許使用者存取它時使用。

如需詳細資訊，請參閱下列檔案：

* [擷取授權決定API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### 5.什麼是資源？支援哪些格式？ {#authorization-phase-faq5}

資源是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource)檔案中定義的辭彙。

資源是與MVPD議定的唯一識別碼，並與使用者端應用程式可串流的內容相關聯。

資源唯一識別碼可以有兩種格式：

* 簡單的字串格式，例如管道（品牌）的唯一識別碼。
* 包含其他資訊的媒體RSS (MRSS)格式，例如標題、分級和家長監護中繼資料。

如需詳細資訊，請參閱[受保護的資源](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources)檔案。

#### 6.使用者端應用程式一次可取得多少資源授權決定？ {#authorization-phase-faq6}

由於MVPD施加的條件，使用者端應用程式可以在單一API要求中取得有限資源數的授權決定，通常最多1個。

+++

### 登出階段常見問題集 {#logout-phase-faqs-general}

+++登出階段常見問題集

#### 1.登出階段的用途為何？ {#logout-phase-faq1}

登出階段的目的是讓使用者端應用程式能夠應使用者要求，在Adobe Pass驗證中終止使用者的已驗證設定檔。

+++

### 標題常見問答 {#headers-faqs-general}

+++標頭常見問題集

#### 1.如何計算Authorization標頭的值？ {#headers-faq1}

[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)請求標頭包含使用者端應用程式存取受Adobe Pass保護的API所需的`Bearer`存取權杖。

在註冊階段期間，必須從Adobe Pass驗證取得授權標頭值。

如需詳細資訊，請參閱下列檔案：

* [Dynamic Client註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [擷取使用者端認證API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [擷取存取Token API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [動態使用者端註冊流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

如果使用者端應用程式從REST API V1移轉至REST API V2，使用者端應用程式可以繼續使用相同的方法取得`Bearer`存取權杖。

#### 2.如何計算AP-Device-Identifier標頭的值？ {#headers-faq2}

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)要求標頭包含由使用者端應用程式建立的串流裝置識別碼。

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)標標頭檔案提供了一些計算不同平台值的範例，但使用者端應用程式可以根據自己的商業邏輯和需求選擇使用不同的方法。

萬一使用者端應用程式從REST API V1移轉至REST API V2，使用者端應用程式可繼續使用與先前相同的方法運算裝置識別碼。

#### 3.如何計算X-Device-Info標頭的值？ {#headers-faq3}

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)要求標頭包含與實際串流裝置相關的使用者端資訊（裝置、連線和應用程式）。

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)標標頭檔案提供一些計算不同平台之值的範例，但使用者端應用程式可以根據自己的商業邏輯和需求，選擇使用不同的方法。

萬一使用者端應用程式從REST API V1移轉至REST API V2，使用者端應用程式可繼續使用與先前相同的方法運算裝置資訊。

+++

## 移轉常見問題集 {#migration-faqs}

如果您使用的應用程式需要將現有應用程式移轉至REST API V2，請繼續本節內容。

>[!MORELIKETHIS]
>
> * [動態使用者端註冊(DCR)常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### 一般移轉常見問題集 {#general-migration-faqs}

+++一般移轉常見問題集

#### 1.需要我一次向所有使用者推出移轉至REST API V2的新使用者端應用程式嗎？ {#migration-faq1}

不適用。

使用者端應用程式不需要同時推出整合REST API V2的新版本給所有使用者。

到2025年底，Adobe Pass驗證將繼續支援整合REST API V1或SDK的舊版使用者端應用程式。

#### 2.需要我一次在所有API和流程中推出移轉至REST API V2的新使用者端應用程式嗎？ {#migration-faq2}

是。

使用者端應用程式需要同時推出跨所有Adobe Pass驗證API和流程整合REST API V2的新版本。

在「第二個熒幕驗證」流程中，使用者端應用程式必須同時針對[主要](rest-api-v2-glossary.md#primary-application)和[次要](rest-api-v2-glossary.md#secondary-application)應用程式推出整合REST API V2的新版本。

Adobe Pass驗證將不支援在API和流程之間整合REST API V2和REST API V1/SDK的「混合」實作。

#### 3.更新至移轉至REST API V2的新使用者端應用程式時，是否會保留使用者驗證？ {#migration-faq3}

不適用。

在整合REST API V1或SDK的較舊使用者端應用程式版本中取得的使用者驗證將不會保留。

因此，使用者必須在移轉至REST API V2的新使用者端應用程式中重新驗證。

#### 4. REST API V2是否預設啟用增強式錯誤代碼？ {#migration-faq4}

是。

整合REST API V2的使用者端應用程式受益於預設啟用的增強錯誤代碼功能。

如需詳細資訊，請參閱[增強錯誤碼](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)檔案。

+++

### 從REST API V1移轉至REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

如果您正在處理需要從REST API V1移轉至REST API V2的應用程式，請繼續此子區段。

#### 註冊階段常見問題集 {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++註冊階段常見問題集

##### 1.註冊階段需要哪些高階API移轉？ {#registration-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，註冊階段沒有高層級變更。

使用者端應用程式可以繼續使用相同的流程，透過[動態使用者端註冊(DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr)程式針對Adobe Pass驗證進行註冊，並取得存取權杖。

如需詳細資訊，請參閱下列檔案：

* [Dynamic Client註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [擷取使用者端認證API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [擷取存取Token API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [動態使用者端註冊流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

#### 設定階段常見問題集 {#configuration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++設定階段常見問題集

##### 1.設定階段需要哪些高階API移轉？ {#configuration-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [GET<br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 驗證階段常見問題集 {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++驗證階段常見問題集

##### 1.驗證階段需要哪些高階API移轉？ {#authentication-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取註冊代碼（驗證代碼） | [POST<br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查註冊代碼（驗證代碼） | [GET<br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二個熒幕繼續(MVPD)驗證（應用程式） | [GET<br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 啟動(MVPD)驗證 | [GET<br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [GET<br/> /api/v1/checkauthn （第一個畫面）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET<br/> /api/v1/checkauthn （第二個畫面）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者驗證Token （設定檔） | [GET<br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [GET<br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### 預先授權階段常見問題集 {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++預先授權階段常見問題集

##### 1.預先授權階段需要哪些高階API移轉？ {#preauthorization-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [GET<br/> /api/v1/preauthorize （第一個畫面）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET<br/> /api/v1/preauthorize （第二個畫面）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 授權階段常見問題集 {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++授權階段常見問題集

##### 1.授權階段需要哪些高階API移轉？ {#authorization-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動(MVPD)授權 | [GET<br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 擷取授權Token （授權決定） | [GET<br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 擷取短授權Token （媒體Token） | [GET<br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### 登出階段常見問題集 {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++登出階段常見問題集

##### 1.登出階段需要哪些高階API移轉？ {#logout-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動登出 | [GET<br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### 從SDK移轉至REST API V2 {#migration-sdk-to-rest-api-v2}

如果您正在處理需要從SDK移轉至REST API V2的應用程式，請繼續此子區段。

#### 註冊階段常見問題集 {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++註冊階段常見問題集

##### 1.註冊階段需要哪些高階API移轉？ {#registration-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

###### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成動態使用者端註冊(DCR) | 提供軟體陳述式給建構函式 | [POST<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET<br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[動態使用者端註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[動態使用者端註冊流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成動態使用者端註冊(DCR) | 提供軟體陳述式給建構函式 | [POST<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET<br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[動態使用者端註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[動態使用者端註冊流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成動態使用者端註冊(DCR) | 提供軟體陳述式給建構函式 | [POST<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET<br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[動態使用者端註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[動態使用者端註冊流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成動態使用者端註冊(DCR) | 提供軟體陳述式給建構函式 | [POST<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET<br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[動態使用者端註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[動態使用者端註冊流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### 設定階段常見問題集 {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++設定階段常見問題集

##### 1.設定階段需要哪些高階API移轉？ {#configuration-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

###### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [GET<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [GET<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [GET<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET<br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 驗證階段常見問題集 {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++驗證階段常見問題集

##### 1.驗證階段需要哪些高階API移轉？ {#authentication-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

###### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取註冊代碼（驗證代碼） | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查註冊代碼（驗證代碼） | [GET<br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二個熒幕繼續(MVPD)驗證（應用程式） | [GET<br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 啟動(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取註冊代碼（驗證代碼） | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查註冊代碼（驗證代碼） | [GET<br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二個熒幕繼續(MVPD)驗證（應用程式） | [GET<br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 啟動(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET<br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | 使用下列其中一項： <br/><br/> [GET<br/> /api/v2/{serviceProvider}/設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET<br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### 預先授權階段常見問題集 {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++預先授權階段常見問題集

##### 1.預先授權階段需要哪些高階API移轉？ {#preauthorization-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

###### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| 範圍 | SDK | REST API V2 | 觀察 |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 授權階段常見問題集 {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++授權階段常見問題集

##### 1.授權階段需要哪些高階API移轉？ {#authorization-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

###### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取短授權Token （媒體Token） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取短授權Token （媒體Token） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取短授權Token （媒體Token） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取短授權Token （媒體Token） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### 登出階段常見問題集 {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++登出階段常見問題集

##### 1.登出階段需要哪些高階API移轉？ {#logout-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

###### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動登出 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動登出 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動登出 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動登出 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [GET<br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
