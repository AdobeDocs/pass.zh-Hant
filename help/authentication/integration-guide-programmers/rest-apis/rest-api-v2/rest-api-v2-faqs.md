---
title: REST API V2常見問題集
description: REST API V2常見問題集
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: ebe0a53e3ba54c2effdef45c1143deea0e6e57d3
workflow-type: tm+mt
source-wordcount: '9566'
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

### 註冊階段常見問題集 {#registration-phase-faqs-general}

+++註冊階段常見問題集

請參閱[Dynamic Client Registration (DCR)常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs)檔案。

+++

### 設定階段常見問題集 {#configuration-phase-faqs-general}

+++設定階段常見問題集

#### 1.組態階段的用途為何？ {#configuration-phase-faq1}

設定階段的用途是向使用者端應用程式提供與其主動整合的MVPD清單，以及由Adobe Pass驗證針對每個MVPD儲存的設定詳細資料（例如，`id`、`displayName`、`logoUrl`等）。

當使用者端應用程式需要要求使用者選取他們的電視提供者時，「組態階段」會成為「驗證階段」的先決條件步驟。

#### 2.設定階段是否為必要階段？ {#configuration-phase-faq2}

設定階段並非強制性，使用者端應用程式必須在使用者需要選取其MVPD以進行驗證或重新驗證時，才可擷取設定。

在下列情況下，使用者端應用程式可以略過此階段：

* 使用者已經過驗證。
* 透過基本或促銷[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)功能提供使用者暫時存取。
* 使用者驗證已過期，但使用者端應用程式已快取先前選取的MVPD作為使用者體驗動機的選擇，而且只是提示使用者確認他們仍然是該MVPD的訂閱者。

#### 3.什麼是設定？其有效期為多久？ {#configuration-phase-faq3}

設定是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration)檔案中定義的辭彙。

組態包含由下列屬性`id`、`displayName`、`logoUrl`等定義的MVPD清單，這些屬性可從[組態](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)端點擷取。

使用者端應用程式只有在使用者需要選取其MVPD以進行驗證或重新驗證時，才必須擷取設定。

當使用者需要選取電視提供者時，使用者端應用程式可使用設定回應，以呈現具有可用MVPD選項的UI選擇器。

使用者端應用程式必須儲存使用者選取的MVPD識別碼(如MVPD的組態層級`id`屬性所指定)，才能繼續驗證、預先授權、授權或登出階段。

如需詳細資訊，請參閱[擷取組態](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)檔案。

#### 4.使用者端應用程式是否應該將組態回應資訊快取在永久儲存體中？ {#configuration-phase-faq4}

使用者端應用程式只有在使用者需要選取其MVPD以進行驗證或重新驗證時，才必須擷取設定。

使用者端應用程式應將設定回應資訊快取到記憶體儲存體中，以避免不必要的請求並改善使用者體驗，當：

* 使用者已經過驗證。
* 透過基本或促銷[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)功能提供使用者暫時存取。
* 使用者驗證已過期，但使用者端應用程式已快取先前選取的MVPD作為使用者體驗動機的選擇，而且只是提示使用者確認他們仍然是該MVPD的訂閱者。

#### 5.使用者端應用程式可以管理自己的MVPD清單嗎？ {#configuration-phase-faq5}

使用者端應用程式可以管理自己的MVPD清單，但必須使MVPD識別碼與Adobe Pass驗證保持同步。 因此，建議您使用Adobe Pass驗證提供的設定，以確保清單是最新且正確的。

如果提供的Adobe Pass識別碼無效，或是使用者端應用程式未與指定的[服務提供者](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)有效整合，使用者端應用程式將會從MVPD驗證REST API V2收到[錯誤](rest-api-v2-glossary.md#service-provider)。

#### 6.使用者端應用程式可以篩選MVPD清單嗎？ {#configuration-phase-faq6}

使用者端應用程式可以根據自己的商業邏輯和需求（例如使用者位置或先前選取的使用者歷史記錄），實作自訂機制，以篩選設定回應中提供的MVPD清單。

使用者端應用程式可以篩選[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) MVPD的清單，或整合仍在開發或測試中的MVPD。

#### 7.如果與MVPD的整合停用並標籤為非使用中，會發生什麼情況？ {#configuration-phase-faq7}

當與MVPD的整合停用並標籤為非使用中時，MVPD會從進一步設定回應中提供的MVPD清單中移除，您需考慮兩個重要後果：

* 該MVPD未經驗證的使用者將無法再使用該MVPD完成驗證階段。
* 該MVPD的已驗證使用者將無法再使用該MVPD完成預先授權、授權或登出階段。

如果使用者選取的Adobe Pass不再與指定的[服務提供者](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)有效整合，使用者端應用程式將會從MVPD Authentication REST API V2收到[錯誤](rest-api-v2-glossary.md#service-provider)。

#### 8.如果與MVPD的整合啟用回頭並標籤為使用中，會發生什麼情況？ {#configuration-phase-faq8}

當與MVPD的整合重新啟用並標籤為作用中時，MVPD會重新納入進一步設定回應中提供的MVPD清單中，您需考慮兩個重要後果：

* 該MVPD的未經驗證使用者將能夠再次使用該MVPD完成驗證階段。
* 該MVPD的已驗證使用者將能夠使用該MVPD再次完成預先授權、授權或登出階段。

#### 9.如何啟用或停用與MVPD的整合？ {#configuration-phase-faq9}

您的組織管理員或代表您行事的Adobe Pass驗證代表可透過Adobe Pass [TVE控制面板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)完成此作業。

如需詳細資訊，請參閱[TVE儀表板整合使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration)檔案。

+++

### 驗證階段常見問題集 {#authentication-phase-faqs-general}

+++驗證階段常見問題集

#### 1.驗證階段的用途為何？ {#authentication-phase-faq1}

「驗證階段」的目的是讓使用者端應用程式能夠驗證使用者的身分並取得使用者中繼資料資訊。

當使用者端應用程式需要播放內容時，「驗證階段」會成為「預先授權階段」或「授權階段」的先決條件步驟。

#### &#x200B;2. 「驗證階段」是否為必要？ {#authentication-phase-faq2}

驗證階段是強制性的，當使用者在Adobe Pass驗證中沒有有效的設定檔時，使用者端應用程式必須驗證使用者。

在下列情況下，使用者端應用程式可以略過此階段：

* 使用者已經過驗證，設定檔仍然有效。
* 透過基本或促銷[TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)功能提供使用者暫時存取。

使用者端應用程式錯誤處理需要處理[錯誤](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)代碼（例如，`authenticated_profile_missing`、`authenticated_profile_expired`、`authenticated_profile_invalidated`等），這表示使用者端應用程式需要使用者進行驗證。

#### 3.什麼是驗證工作階段？其有效期限為多久？ {#authentication-phase-faq3}

驗證工作階段是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session)檔案中定義的辭彙。

驗證工作階段會儲存可從「工作階段」端點擷取的已起始驗證程式相關資訊。

驗證工作階段的有效時間有限，且在由`notAfter`時間戳記所指定的問題發生時很短，這表示在需要重新啟動流程之前，使用者必須完成驗證程式的時間長度。

使用者端應用程式可使用驗證工作階段回應，以瞭解如何進行驗證程式。 請注意，在某些情況下，使用者不需要驗證，例如提供暫時存取、存取效能降低，或使用者已經驗證。

如需詳細資訊，請參閱下列檔案：

* [建立驗證工作階段API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [繼續驗證工作階段API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 4.什麼是驗證碼？其有效期為多久？ {#authentication-phase-faq4}

驗證代碼是[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code)檔案中定義的辭彙。

驗證代碼會儲存使用者啟動驗證程式時產生的唯一值，並唯一識別使用者的驗證工作階段，直到程式完成或驗證工作階段過期為止。

驗證程式碼的有效時間範圍僅限於起始驗證工作階段時指定的有限且較短的時間範圍（以`notAfter`時間戳記表示），這表示使用者在需要重新啟動流程之前必須完成驗證程式的時間量。

使用者端應用程式可使用驗證碼來驗證使用者是否已成功完成驗證，並擷取使用者的設定檔資訊，通常透過輪詢機制。

使用者端應用程式也可以使用驗證代碼，讓使用者在相同裝置或第二個（熒幕）裝置上完成或繼續驗證程式，因為驗證工作階段並未到期。

如需詳細資訊，請參閱下列檔案：

* [建立驗證工作階段API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [繼續驗證工作階段API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 5.使用者端應用程式如何知道使用者是否輸入有效的驗證代碼，以及驗證工作階段是否尚未過期？ {#authentication-phase-faq5}

使用者端應用程式可以傳送要求給負責繼續驗證工作階段或擷取與驗證代碼關聯的驗證工作階段資訊的其中一個「工作階段」端點，來驗證使用者在次要（熒幕）應用程式中輸入的驗證代碼。

如果輸入的驗證碼錯誤或驗證工作階段過期，使用者端應用程式會收到[錯誤](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)。

如需詳細資訊，請參閱[繼續驗證工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)和[擷取驗證工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)檔案。

#### 6.使用者端應用程式如何知道使用者是否已驗證？ {#authentication-phase-faq6}

使用者端應用程式可以查詢下列其中一個端點，該端點能夠驗證使用者是否已通過驗證並傳回設定檔資訊：

* [設定檔端點API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（驗證）程式碼API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

如需詳細資訊，請參閱下列檔案：

* [主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 7.設定檔是什麼？其有效期為多久？ {#authentication-phase-faq7}

設定檔是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile)檔案中定義的辭彙。

設定檔會儲存有關使用者驗證有效性的資訊、中繼資料資訊以及可從「設定檔」端點擷取的更多資訊。

使用者端應用程式可使用設定檔來瞭解使用者的驗證狀態、存取使用者中繼資料資訊、尋找用於驗證的方法或用於提供身分識別的實體。

如需詳細資訊，請參閱下列檔案：

* [設定檔端點API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（驗證）程式碼API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

設定檔在被`notAfter`時間戳記查詢時指定的有限時間範圍內有效，這表示使用者在需要再次通過驗證階段之前具有有效驗證的時間量。

這個有限的時間範圍稱為驗證(authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl)，可由您的組織管理員或代表您行事的Adobe Pass驗證代表透過Adobe Pass [TVE儀表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)檢視及變更。

如需詳細資訊，請參閱[TVE儀表板整合使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)檔案。

#### 8.使用者端應用程式是否應該將使用者的設定檔資訊快取在永久儲存體中？ {#authentication-phase-faq8}

使用者端應用程式應將部分使用者設定檔資訊快取到永久性儲存體中，以避免不必要的請求，並改善使用者體驗，同時考慮到以下方面：

| 屬性 | 使用者體驗 |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `mvpd` | 使用者端應用程式可使用此項來追蹤使用者選取的電視提供者，並在預先授權或授權階段中繼續使用它。<br/><br/>當目前的使用者設定檔過期時，使用者端應用程式可以使用記憶中的MVPD選項，並直接要求使用者確認。 |
| `attributes` | 使用者端應用程式可使用此專案，根據不同的[使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)金鑰（例如，`zip`、`maxRating`等）來個人化使用者體驗。<br/><br/>使用者中繼資料在驗證流程完成後即可使用，因此使用者端應用程式不需要查詢個別端點來擷取[使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)資訊，因為它已包含在設定檔資訊中。<br/><br/>在授權流程期間，可能會根據MVPD和特定的中繼資料屬性，更新某些中繼資料屬性。 因此，使用者端應用程式可能需要再次查詢設定檔API，以擷取最新的使用者中繼資料。 |
| `notAfter` | 使用者端應用程式可使用此項來追蹤使用者設定檔到期日。<br/><br/>使用者端應用程式錯誤處理需要處理[錯誤](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)代碼（例如，`authenticated_profile_missing`、`authenticated_profile_expired`、`authenticated_profile_invalidated`等），這表示使用者端應用程式需要使用者進行驗證。 |

#### 9.使用者端應用程式可以擴充使用者的設定檔而不需要重新驗證嗎？ {#authentication-phase-faq9}

不適用。

使用者設定檔的到期時間是由使用MVPD建立的驗證TTL所決定，因此若沒有使用者互動，則無法延伸至超過其有效期限。

因此，使用者端應用程式必須提示使用者重新驗證，並與MVPD登入頁面互動，以重新整理我們在系統上的設定檔。

不過，對於支援[家用驗證](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) (HBA)的MVPD，使用者不需要輸入認證。

#### 10.每個可用設定檔端點的使用案例為何？ {#authentication-phase-faq10}

基本設定檔端點的設計是為了讓使用者端應用程式能夠知道使用者的驗證狀態、存取使用者中繼資料資訊、尋找用於驗證的方法或用於提供身分識別的實體。

每個端點皆適合特定使用案例，如下所示：

| API | 說明 | 使用案例 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [設定檔端點API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | 擷取所有使用者設定檔。 | **使用者第一次開啟使用者端應用程式**<br/><br/>&#x200B;在此案例中，使用者端應用程式不會將使用者選取的MVPD識別碼快取到永久儲存體中。<br/><br/>因此，它將傳送單一要求以擷取所有可用的使用者設定檔。 |
| 特定MVPD API的[設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | 擷取與特定MVPD相關聯的使用者設定檔。 | **使用者在上次造訪中進行驗證後返回使用者端應用程式**<br/><br/>&#x200B;在此情況下，使用者端應用程式必須將使用者先前選取的MVPD識別碼快取到永久儲存區。<br/><br/>因此，它會傳送單一要求，以擷取該特定MVPD的使用者設定檔。 |
| [特定（驗證）程式碼API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 擷取與特定驗證代碼相關聯的使用者設定檔。 | **使用者啟動驗證程式**<br/><br/>&#x200B;在此案例中，使用者端應用程式必須判斷使用者是否已順利完成驗證，並擷取其設定檔資訊。<br/><br/>因此，它會啟動輪詢機制，以擷取與驗證碼相關聯的使用者設定檔。 |

如需詳細資訊，請參閱主要應用程式[內執行的](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)基本設定檔流程，以及次要應用程式[檔案內執行的](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)基本設定檔流程。

設定檔SSO端點有不同的用途，它讓使用者端應用程式能夠使用合作夥伴驗證回應建立使用者設定檔，並在單次作業中擷取它。

| API | 說明 | 使用案例 |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [設定檔SSO端點API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) | 使用合作夥伴驗證回應來建立和擷取使用者設定檔。 | **使用者允許應用程式使用合作夥伴單一登入來進行驗證**<br/><br/>&#x200B;在此案例中，使用者端應用程式必須在收到合作夥伴驗證回應之後建立使用者設定檔，並透過單次、一次性的作業擷取該設定檔。 |

對於任何後續的查詢，必須使用基本設定檔端點來判斷使用者的驗證狀態、存取使用者中繼資料資訊、尋找用於驗證的方法或用於提供身分識別的實體。

如需詳細資訊，請參閱[使用合作夥伴流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)的單一登入[Apple SSO逐步指南(REST API V2)](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)檔案。

#### 11.如果使用者有多個MVPD設定檔，使用者端應用程式該怎麼做？ {#authentication-phase-faq11}

支援多個設定檔的決策，取決於使用者端應用程式的業務需求。

大部分的使用者將只有一個設定檔。 但是，如果存在多個設定檔（如下所述），使用者端應用程式負責決定設定檔選擇的最佳使用者體驗。

使用者端應用程式可選擇提示使用者選取所需的MVPD設定檔，或自動進行選取，例如從回應中選擇第一個使用者設定檔，或選擇有效期最長的使用者設定檔。

REST API v2支援多個設定檔以適應：

* 使用者可能必須在一般MVPD設定檔與透過單一登入(SSO)取得的設定檔之間進行選擇。
* 使用者無需從其一般MVPD登出，即可獲得暫時存取許可權。
* 具有MVPD訂閱結合直接對消費者(DTC)服務的使用者。
* 有多個MVPD訂閱的使用者。

#### 12.當使用者設定檔過期時，會發生什麼事？ {#authentication-phase-faq12}

當使用者設定檔到期時，它們不再包含在來自設定檔端點的回應中。

如果Profiles端點傳回空的設定檔對應回應，使用者端應用程式必須建立新的驗證工作階段，並提示使用者重新驗證。

如需詳細資訊，請參閱[建立驗證工作階段API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)檔案。

#### 13.使用者設定檔何時會失效？ {#authentication-phase-faq13}

使用者設定檔在下列情況下會失效：

* 驗證TTL過期時，如Profiles端點回應中的`notAfter`時間戳記所指示。
* 使用者端應用程式變更[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)標頭值時。
* 當使用者端應用程式更新用於擷取[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)標頭值的使用者端認證時。
* 當使用者端應用程式撤銷或更新用來取得使用者端認證的軟體陳述式時。

#### 14.使用者端應用程式應何時啟動輪詢機制？ {#authentication-phase-faq14}

為了確保效率並避免不必要的請求，使用者端應用程式必須在下列條件下啟動輪詢機制：

**在主要（熒幕）應用程式內執行的驗證**

主要（串流）應用程式應在瀏覽器元件載入`redirectUrl`工作階段[端點要求中](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)引數指定的URL後，於使用者到達最終目的地頁面時開始輪詢。

**在次要（熒幕）應用程式內執行的驗證**

主要（串流）應用程式應在使用者起始驗證程式後立即開始輪詢 — 在收到[工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)端點回應並向使用者顯示驗證代碼之後。

#### 15.使用者端應用程式何時應停止輪詢機制？ {#authentication-phase-faq15}

為了確保效率並避免不必要的請求，使用者端應用程式必須在下列條件下停止輪詢機制：

**成功的驗證**

已成功擷取使用者的設定檔資訊，確認其驗證狀態。 此時，不再需要輪詢。

**驗證工作階段與程式碼到期**

驗證工作階段和程式碼會過期，如`notAfter`工作階段[端點回應中的](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)時間戳記（例如30分鐘）所指示。 如果發生這種狀況，使用者必須重新啟動驗證程式，而且使用先前驗證代碼的輪詢應該立即停止。

**已產生新的驗證碼**

如果使用者在主要（熒幕）裝置上要求新的驗證碼，則現有工作階段不再有效，使用先前驗證碼的輪詢應立即停止。

#### 16.使用者端應用程式應用於輪詢機制的呼叫間隔為何？ {#authentication-phase-faq16}

為了確保效率並避免不必要的請求，使用者端應用程式必須在下列條件下設定輪詢機制頻率：

| **在主要（熒幕）應用程式內執行的驗證** | **在次要（熒幕）應用程式內執行的驗證** |
|----------------------------------------------------------------------------|----------------------------------------------------------------------------|
| 主要（串流）應用程式應每3-5秒或更長時間輪詢一次。 | 主要（串流）應用程式應每3-5秒或更長時間輪詢一次。 |

#### 17.使用者端應用程式可傳送的輪詢要求數目上限是多少？ {#authentication-phase-faq17}

使用者端應用程式必須遵守Adobe Pass驗證[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits)所定義的目前限制。

使用者端應用程式錯誤處理必須能夠處理[429太多要求](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response)錯誤碼，這表示使用者端應用程式已超過允許的要求數目上限。

如需詳細資訊，請參閱[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)檔案。

#### 18.使用者端應用程式如何取得使用者的中繼資料資訊？ {#authentication-phase-faq18}

使用者端應用程式可以查詢下列端點之一，這些端點能夠傳回[使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)資訊作為設定檔資訊的一部分：

* [設定檔端點API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [特定MVPD API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [特定（驗證）程式碼API的設定檔端點](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

使用者中繼資料在驗證流程完成後即可使用，因此使用者端應用程式不需要查詢個別端點來擷取[使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)資訊，因為它已包含在設定檔資訊中。

如需詳細資訊，請參閱下列檔案：

* [主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

在授權流程期間，某些中繼資料屬性可能會根據MVPD和特定的中繼資料屬性而更新。 因此，使用者端應用程式可能需要再次查詢上述API，以擷取最新的使用者中繼資料。

#### 19.使用者端應用程式應如何管理降級存取？ {#authentication-phase-faq19}

[降級功能](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)可讓使用者端應用程式維持使用者的順暢串流體驗，即使使用者的MVPD驗證或授權服務發生問題亦然。

作為摘要，這可確保MVPD臨時服務中斷的情況下對內容的存取不中斷。

鑑於您的組織打算使用進階降級功能，使用者端應用程式必須處理降級存取流程，以概述REST API v2端點在此類情境下的行為。

如需詳細資訊，請參閱[降級存取流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)檔案。

#### 20.使用者端應用程式應如何管理暫時存取？ {#authentication-phase-faq20}

[TempPass功能](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)可讓使用者端應用程式提供使用者暫時存取許可權。

作為摘要，這可提供在指定時段內有限的內容存取權或預先定義數量的VOD標題，以吸引使用者。

鑑於您的組織打算使用高級TempPass功能，使用者端應用程式必須處理暫時存取流程，概述了REST API v2端點在此類情況下的行為。

在舊版API中，使用者端應用程式必須登出已透過一般MVPD驗證的使用者，才能提供暫時存取權。

透過REST API v2，使用者端應用程式在授權資料流時，可以順暢地在一般MVPD和TempPass MVPD之間切換，而不需要使用者登出。

如需詳細資訊，請參閱[暫時存取流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)檔案。

#### 21.使用者端應用程式應如何管理跨裝置單一登入存取？ {#authentication-phase-faq21}

如果使用者端應用程式提供跨裝置的一致唯一使用者識別碼，REST API v2可啟用跨裝置單一登入。

此識別碼（稱為服務權杖）必須由使用者端應用程式透過實作或整合所選的外部Identity Service而產生。

如需詳細資訊，請參閱[使用服務權杖流程的單一登入](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)檔案。

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

#### 4.使用者端應用程式是否應該將預先授權決定快取到永久儲存體中？ {#preauthorization-phase-faq4}

使用者端應用程式不需要將預先授權決定儲存在永久儲存體中。 但是，建議將允許決策快取到記憶體中以改善使用者體驗。 這有助於避免對已預先授權的資源之決定預先授權端點進行不必要的呼叫，從而減少延遲並改善效能。

#### 5.使用者端應用程式如何判斷預先授權決定被拒絕的原因？ {#preauthorization-phase-faq5}

使用者端應用程式可檢查包含在決定預先授權端點回應中的[錯誤碼和訊息](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)，以判斷拒絕預先授權決定的原因。 這些詳細資料可為insight提供預先授權請求被拒絕的特定原因，有助於告知使用者體驗或觸發應用程式中的任何必要處理。

如果預先授權決定遭拒，請確定任何針對擷取預先授權決定所實作的重試機制，都不會導致無休止的回圈。

請考慮將重試限制在合理數字，並透過向使用者呈現清楚的意見反應來適當地處理拒絕。

#### 6.為何預先授權決定遺失媒體權杖？ {#preauthorization-phase-faq6}

預先授權決定遺失媒體權杖，因為預先授權階段不能用於播放資源，因為這是授權階段的用途。

#### 7.如果預先授權決定已經存在，可以略過「授權階段」嗎？ {#preauthorization-phase-faq7}

不適用。

即使預先授權決定可用，也無法跳過授權階段。 預先授權決定僅供參考，不會授予實際的播放許可權。 預先授權階段旨在提供早期指引，但在播放任何內容之前仍需要授權階段。

#### 8.什麼是資源？支援哪些格式？ {#preauthorization-phase-faq8}

資源是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource)檔案中定義的辭彙。

資源是與MVPD議定的唯一識別碼，並與使用者端應用程式可串流的內容相關聯。

資源唯一識別碼可以有兩種格式：

* 簡單的字串格式，例如管道（品牌）的唯一識別碼。
* 包含其他資訊的媒體RSS (MRSS)格式，例如標題、分級和家長監護中繼資料。

如需詳細資訊，請參閱[受保護的資源](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources)檔案。

#### 9.使用者端應用程式一次可取得多少資源預先授權決定？ {#preauthorization-phase-faq9}

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

#### 4.使用者端應用程式是否應將授權決定快取至永久儲存區？ {#authorization-phase-faq4}

使用者端應用程式不需要將授權決定儲存在永久儲存體中。

#### 5.使用者端應用程式如何判斷授權決定被拒絕的原因？ {#authorization-phase-faq5}

使用者端應用程式可以檢查包含在來自Decisions Authorize端點的回應中的[錯誤碼和訊息](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)，以判斷拒絕授權決定的原因。 這些詳細資料可為insight提供授權請求遭拒絕的特定原因，有助於告知使用者體驗或觸發應用程式中的任何必要處理。

如果授權決定遭拒，請確定任何針對擷取授權決定所實作的重試機制都不會導致無限回圈。

請考慮將重試限制在合理數字，並透過向使用者呈現清楚的意見反應來適當地處理拒絕。

#### 6.什麼是媒體代號？其有效期為何？ {#authorization-phase-faq6}

媒體權杖是[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token)檔案中定義的辭彙。

媒體權杖包含以純文字傳送的已簽署字串，可從「決定授權」端點擷取。

如需詳細資訊，請參閱[媒體權杖驗證器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)檔案。

媒體權杖在問題時指定的有限和較短時間範圍內有效，這表示使用者端應用程式必須驗證和使用它之前的時間限制。

使用者端應用程式可以使用媒體權杖來播放資源串流，以備電視提供者（權威）決定允許使用者存取它時使用。

如需詳細資訊，請參閱下列檔案：

* [擷取授權決定API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### 7.使用者端應用程式是否應在播放資源資料流之前驗證媒體權杖？ {#authorization-phase-faq7}

是。

使用者端應用程式必須先驗證媒體權杖，才能開始播放資源資料流。 應該使用[媒體權杖驗證器](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier)來執行此驗證。 透過驗證傳回`serializedToken`中的`token`，使用者端應用程式可協助防止未經授權的存取（例如串流擷取），並確保只有經過適當授權的使用者才能播放內容。

#### 8.使用者端應用程式是否應在播放期間重新整理過期的媒體權杖？ {#authorization-phase-faq8}

不適用。

當串流正在播放時，使用者端應用程式不需要重新整理過期的媒體Token。 如果媒體權杖在播放期間到期，則應該允許資料流繼續而不中斷。 不過，使用者端在下次嘗試播放資源時，必須要求新的授權決定，並取得新的媒體代號。

#### 9.授權決定中每個時間戳記屬性的用途為何？ {#authorization-phase-faq9}

授權決定包括數個時間戳記屬性，這些屬性提供有關授權本身及關聯媒體權杖的有效期間的重要內容。 這些時間戳記根據與授權決定或媒體權杖相關，有不同的用途。

**決定層級時間戳記**

以下時間戳記說明整體授權決定的有效期間：

| 屬性 | 說明 | 附註 |
|-------------|----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | 發出授權決定的時間（毫秒）。 | 這會標籤授權有效性視窗的開頭。 |
| `notAfter` | 授權決定到期的時間（毫秒）。 | [授權存留時間(TTL)](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#authorization-ttl-management)決定授權在需要重新授權之前維持有效時間。 此TTL會與MVPD代表商議。 |

**權杖層級時間戳記**

以下時間戳記說明與授權決定繫結的媒體權杖的有效期：

| 屬性 | 說明 | 附註 |
|-------------|-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | 發佈媒體權杖的時間（毫秒）。 | 這會標籤代號何時變成適用於播放的有效代號。 |
| `notAfter` | 媒體權杖過期的時間（毫秒）。 | 媒體權杖的生命週期故意縮短（通常為7分鐘），以將誤用風險降至最低，並解決權杖產生伺服器和權杖驗證伺服器之間的潛在時鐘差異。 |

#### 10.什麼是資源？支援哪些格式？ {#authorization-phase-faq10}

資源是在[字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource)檔案中定義的辭彙。

資源是與MVPD議定的唯一識別碼，並與使用者端應用程式可串流的內容相關聯。

資源唯一識別碼可以有兩種格式：

* 簡單的字串格式，例如管道（品牌）的唯一識別碼。
* 包含其他資訊的媒體RSS (MRSS)格式，例如標題、分級和家長監護中繼資料。

如需詳細資訊，請參閱[受保護的資源](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources)檔案。

#### 11.使用者端應用程式一次可取得多少資源授權決定？ {#authorization-phase-faq11}

由於MVPD施加的條件，使用者端應用程式可以在單一API要求中取得有限資源數的授權決定，通常最多1個。

+++

### 登出階段常見問題集 {#logout-phase-faqs-general}

+++登出階段常見問題集

#### 1.登出階段的用途為何？ {#logout-phase-faq1}

登出階段的目的是讓使用者端應用程式能夠應使用者要求，在Adobe Pass驗證中終止使用者的已驗證設定檔。

#### 2.登出階段是否為必要？ {#logout-phase-faq2}

登出階段是強制性的，使用者端應用程式必須提供使用者登出的功能。

+++

### 標題常見問答 {#headers-faqs-general}

+++標頭常見問題集

#### 1.如何計算Authorization標頭的值？ {#headers-faq1}

>[!IMPORTANT]
>
> 如果使用者端應用程式從REST API V1移轉至REST API V2，使用者端應用程式可以繼續使用相同的方法取得`Bearer`存取權杖值，就像之前一樣。

[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)請求標頭包含使用者端應用程式存取受Adobe Pass保護的API所需的`Bearer`存取權杖。

在註冊階段期間，必須從Adobe Pass驗證取得授權標頭值。

如需詳細資訊，請參閱下列檔案：

* [Dynamic Client註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [擷取使用者端認證API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [擷取存取Token API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [動態使用者端註冊流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

#### 2.如何計算AP-Device-Identifier標頭的值？ {#headers-faq2}

>[!IMPORTANT]
>
> 萬一使用者端應用程式從REST API V1移轉至REST API V2，使用者端應用程式可繼續使用與先前相同的方法運算裝置識別碼值。

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)要求標頭包含由使用者端應用程式建立的串流裝置識別碼。

[AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)標題檔案提供主要平台如何計算值的範例，但使用者端應用程式可以根據自己的商業邏輯和需求選擇使用不同的方法。

#### 3.如何計算X-Device-Info標頭的值？ {#headers-faq3}

>[!IMPORTANT]
>
> 萬一使用者端應用程式從REST API V1移轉至REST API V2，使用者端應用程式可繼續使用與先前相同的方法運算裝置資訊值。

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)要求標頭包含與實際串流裝置相關的使用者端資訊（裝置、連線和應用程式），用來決定MVPD可能強制執行的平台特定規則。

[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)標題檔案提供主要平台如何計算值的範例，但使用者端應用程式可以根據自己的商業邏輯和需求選擇使用不同的方法。

如果[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)標頭遺失或包含不正確的值，則請求可能會分類為來自`unknown`平台。

這可能會導致請求被視為不安全，並受到更嚴格的規則約束，例如較短的驗證TTL。 此外，某些欄位（例如串流裝置`connectionIp`和`connectionPort`）是諸如Spectrum的[Home Base Authentication](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md)之類功能的必要欄位。

即使請求來自代表裝置的伺服器，[X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)標頭值也必須反映實際的串流裝置資訊。

+++

### 其他常見問答 {#misc-faqs-general}

+++其他常見問題集

#### 1.我可以探索REST API V2請求和回應並測試API嗎？ {#misc-faq1}

是。

您可以透過我們專屬的[Adobe Developer](https://developer.adobe.com/adobe-pass/)網站探索REST API V2。 Adobe Developer網站可讓您不受限制地存取：

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

若要與[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)互動，您必須包含[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)標頭，以及透過`Bearer`DCR API[取得的](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)存取權杖。

若要使用[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)，需要具有REST API V2範圍的軟體陳述式。 如需詳細資訊，請參閱[動態使用者端註冊(DCR)常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)檔案。

#### 2.我可以使用具備OpenAPI支援的API開發工具來探索REST API V2請求和回應嗎？ {#misc-faq2}

是。

您可以從[Adobe Developer](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)網站下載[DCR API](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)和[REST API V2](https://developer.adobe.com/adobe-pass/)的OpenAPI規格檔案。

若要下載OpenAPI規格檔案，請按一下下載按鈕，將下列檔案儲存到本機電腦：

* [DCR API JSON](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [REST API V2 JSON](https://developer.adobe.com/adobe-pass/restApiV2.json)

接著，您可以將這些檔案匯入您偏好的API開發工具，探索REST API V2請求和回應並測試API。

#### 3.我仍可以使用託管於https://sp.auth-staging.adobe.com/apitest/api.html的現有API測試工具嗎？ {#misc-faq3}

不適用。

移轉至REST API V2的使用者端應用程式應使用託管於https://developer.adobe.com/adobe-pass/的新測試工具。 Adobe Developer網站可讓您不受限制地存取：

* [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

若要與[REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)互動，您必須包含[Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)標頭，以及透過`Bearer`DCR API[取得的](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)存取權杖。

若要使用[DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)，需要具有REST API V2範圍的軟體陳述式。 如需詳細資訊，請參閱[動態使用者端註冊(DCR)常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)檔案。

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

#### &#x200B;4. REST API V2是否預設啟用增強式錯誤代碼？ {#migration-faq4}

是。

移轉至REST API V2的使用者端應用程式預設會自動受益於此功能，提供更詳細且精確的錯誤資訊。

如需詳細資訊，請參閱[增強錯誤碼](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2)檔案。

#### 5.移轉至REST API V2時，現有的整合是否需要變更設定？ {#migration-faq5}

不適用。

移轉至REST API V2的使用者端應用程式不需要變更現有MVPD整合的設定。 此外，他們將繼續對現有[服務提供者](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider)和[MVPD](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd)使用相同的識別碼。

此外，Adobe Pass驗證用來與MVPD端點通訊的通訊協定維持不變。

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
| 擷取使用中整合的MVPD清單 | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 驗證階段常見問題集 {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++驗證階段常見問題集

##### 1.驗證階段需要哪些高階API移轉？ {#authentication-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取註冊代碼（驗證代碼） | [張貼<br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [張貼<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查註冊代碼（驗證代碼） | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二個熒幕繼續(MVPD)驗證（應用程式） | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [張貼<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 啟動(MVPD)驗證 | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [GET <br/> /api/v1/checkauthn （第一個畫面）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn （第二個畫面）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者驗證Token （設定檔） | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### 預先授權階段常見問題集 {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++預先授權階段常見問題集

##### 1.預先授權階段需要哪些高階API移轉？ {#preauthorization-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [GET <br/> /api/v1/preauthorize （第一個畫面）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize （第二個畫面）](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [發佈<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 授權階段常見問題集 {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++授權階段常見問題集

##### 1.授權階段需要哪些高階API移轉？ {#authorization-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動(MVPD)授權 | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [張貼<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 擷取授權Token （授權決定） | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [張貼<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| 擷取短授權Token （媒體Token） | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [張貼<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### 登出階段常見問題集 {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++登出階段常見問題集

##### 1.登出階段需要哪些高階API移轉？ {#logout-phase-v1-to-v2-faq1}

從REST API V1移轉至REST API V2時，下表呈現需考量的高層級變更：

| 範圍 | REST API V1 | REST API V2 | 觀察 |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動登出 | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

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
| 完成動態使用者端註冊(DCR) | 提供軟體陳述式給建構函式 | [張貼<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[動態使用者端註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[動態使用者端註冊流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成動態使用者端註冊(DCR) | 提供軟體陳述式給建構函式 | [張貼<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[動態使用者端註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[動態使用者端註冊流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成動態使用者端註冊(DCR) | 提供軟體陳述式給建構函式 | [張貼<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[動態使用者端註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[動態使用者端註冊流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 完成動態使用者端註冊(DCR) | 提供軟體陳述式給建構函式 | [張貼<br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[動態使用者端註冊概述](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[動態使用者端註冊流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### 設定階段常見問題集 {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++設定階段常見問題集

##### 1.設定階段需要哪些高階API移轉？ {#configuration-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

###### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| 擷取使用中整合的MVPD清單 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### 驗證階段常見問題集 {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++驗證階段常見問題集

##### 1.驗證階段需要哪些高階API移轉？ {#authentication-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

###### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [發佈<br/> /api/v2/{serviceProvider}/工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [發佈<br/> /api/v2/{serviceProvider}/工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取註冊代碼（驗證代碼） | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [張貼<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查註冊代碼（驗證代碼） | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二個熒幕繼續(MVPD)驗證（應用程式） | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [張貼<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 啟動(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [發佈<br/> /api/v2/{serviceProvider}/工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [發佈<br/> /api/v2/{serviceProvider}/工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取註冊代碼（驗證代碼） | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [張貼<br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查註冊代碼（驗證代碼） | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 在第二個熒幕繼續(MVPD)驗證（應用程式） | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [張貼<br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 啟動(MVPD)驗證 | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [發佈<br/> /api/v2/{serviceProvider}/工作階段](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本驗證流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| 檢查使用者驗證狀態 | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| 擷取使用者中繼資料資訊 | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | 使用下列其中一項： <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | 使用者端應用程式可一次將這些API的回應用於多個用途： <br/> <ul><li>檢查使用者驗證狀態</li><li>擷取使用者設定檔</li><li>擷取使用者中繼資料資訊</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### 預先授權階段常見問題集 {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++預先授權階段常見問題集

##### 1.預先授權階段需要哪些高階API移轉？ {#preauthorization-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

###### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [發佈<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [發佈<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [發佈<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| 範圍 | SDK | REST API V2 | 觀察 |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取預先授權的資源（預先授權決定） | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [發佈<br/> /api/v2/{serviceProvider}/decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本預先授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### 授權階段常見問題集 {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++授權階段常見問題集

##### 1.授權階段需要哪些高階API移轉？ {#authorization-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

###### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取短授權Token （媒體Token） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [張貼<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取短授權Token （媒體Token） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [張貼<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取短授權Token （媒體Token） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [張貼<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 擷取短授權Token （媒體Token） | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [張貼<br/> /api/v2/{serviceProvider}/decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | 使用者端應用程式可一次將此API的回應用於多個用途： <br/> <ul><li>啟動(MVPD)授權</li><li>擷取授權決定</li><li>擷取短媒體Token</li></ul> <br/>如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本授權流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### 登出階段常見問題集 {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++登出階段常見問題集

##### 1.登出階段需要哪些高階API移轉？ {#logout-phase-sdk-to-v2-faq1}

從SDK移轉至REST API V2時，下表呈現需考量的高層級變更：

###### AccessEnabler JavaScript SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動登出 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動登出 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動登出 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| 範圍 | SDK | REST API V2 | 觀察 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 啟動登出 | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | 如需詳細資訊，請參閱下列檔案： <br/> <ul><li>[主要應用程式內執行的基本登出流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
