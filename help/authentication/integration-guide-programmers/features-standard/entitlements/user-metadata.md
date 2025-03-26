---
title: 使用者中繼資料
description: 使用者中繼資料
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '1902'
ht-degree: 0%

---

# 使用者中繼資料 {#user-metadata}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

使用者中繼資料是指由MVPD維護並透過Adobe Pass驗證[REST API V2](#apis)提供給程式設計師的使用者專屬[屬性](#attributes) （例如郵遞區號、家長分級、使用者ID等）。

驗證流程完成後，使用者中繼資料即可使用，但在授權流程期間，某些中繼資料屬性可能會更新，具體取決於MVPD和有問題的特定中繼資料屬性。

使用者中繼資料可用來增強使用者的個人化，但也可用來進行分析。 例如，程式設計師可能會使用使用者的郵遞區號來傳送當地語系化的新聞或天氣更新，或強制家長監護。

當MVPD提供不同格式的資料時，Adobe Pass驗證會標準化使用者中繼資料值。 此外，對於某些屬性（例如郵遞區號），可以使用程式設計師的憑證將值加密為[加密](#encryption)。

Adobe Pass驗證可讓程式設計師檢閱可在其MVPD整合中使用的使用者中繼資料，並透過[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)進行[管理](#management)。

## 使用者中繼資料屬性 {#attributes}

下表列出程式設計師可以使用的部分使用者中繼資料屬性：

| 索引鍵 | 型別 | 範例 | 需要加密 | 說明 | 詳細資料 |
|------------------|---------|--------------------------------------------------------------|---------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `userID` | 字串 | 「1o7241p」 | 否 | 帳戶識別碼。 | 屬性值可以是家庭識別碼或子帳戶識別碼。 如果MVPD支援子帳戶，且目前使用者不是主要帳戶持有者，`userID`值將與`householdID`不同。 |
| `upstreamUserID` | 字串 | 「1o7241p」 | 否 | 用於並行監視的帳戶識別碼。 | 屬性值可用來在MVPD和程式設計師網站與應用程式間強制執行並行限制。 `upstreamUserID`值與大部分MVPD的`userID`值相同。 |
| `householdID` | 字串 | 「1o7241p」 | 否 | 家長監護的帳戶識別碼。 | 屬性值可用來區分家庭和子帳戶的使用情況。 有時候，如果無法提供真實的分級，家長監護可使用此選項作為替代選項；如果使用者以家庭帳戶登入，他們可以觀看，否則不會顯示分級內容。 在MVPD中，其呈現方式會有許多變數（例如，家庭使用者ID、戶長ID、戶長旗標等），如果MVPD不支援子帳戶，則其將與`userID`相同。 |
| `primaryOID` | 字串 | 「uuidd1e19ec9-012c-124f-b520-acaf118d16a0」 | 否 | 帳戶識別碼。 | 屬性是AT&amp;T專屬的。當`typeID`值設為「主要」時，`primaryOID`值與`userID`值相同。 |
| `typeID` | 字串 | &quot;主要&quot; | 否 | 指出目前使用者為主要或次要帳戶持有人的屬性。 | 屬性是AT&amp;T專屬的。當`typeID`值設為「主要」時，`primaryOID`值與`userID`值相同。 |
| `is_hoh` | 字串 | &quot;1&quot; | 否 | 指出目前使用者是否為戶主的屬性。 | 屬性是Synacor專屬的。 |
| `hba_status` | 布林值 | &quot;true&quot; | 否 | 指出目前使用者是否透過HBA驗證的屬性。 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `allowMirroring` | 布林值 | &quot;true&quot; | 否 | 指出目前裝置是否可映象熒幕的屬性。 | 該屬性專屬於Spectrum。 |
| `zip` | 陣列 | \[&quot;77754&quot;， &quot;12345&quot;\] | 是 | 使用者的郵遞區號。 | 屬性值可用來傳送本地化新聞、天氣更新或體育賽事。 `zip`值代表需要與MVPD簽訂法律協定的敏感資料。 加密後，`zip`金鑰的表示形式將為`String`，而不是`Array`。 |
| `encryptedZip` | 字串 | 「」 | 是 | 使用者的加密郵遞區號。 | 屬性是Comcast專屬的。 |
| `channelID` | 陣列 | \[&quot;channel-1&quot;， &quot;channel-2&quot;\] | 否 | 使用者有權檢視的管道清單。 | 屬性值可用於從彙總多個網路的入口網站篩選各種管道。 我們的建議是使用[預先授權API](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)，而非此使用者中繼資料屬性，來篩選掉使用者無法使用的管道。 |
| `maxRating` | 物件 | { MPAA： &quot;NR&quot;， VCHIP： &quot;X&quot;， URL： &quot;http://manage.my/parental&quot; } | 否 | 目前使用者的最大家長分級。 | 屬性值可用於根據「MPAA」或「VCHIP」評等篩選不適合目前使用者的內容。 |
| `language` | 字串 | &quot;English&quot; | 否 | 語言設定。 | 屬性值可用於根據使用者的語言偏好設定顯示訊息。 |

程式設計師可使用的使用者中繼資料屬性取決於MVPD提供的內容。 下表列出各種MVPD所提供的屬性：

|                         | **已簽署法律合約（僅限zip）** | **使用者ID於AuthN** | **AuthN**&#x200B;的上游使用者ID | **AuthN/Z**&#x200B;上的家庭ID | **AuthN**&#x200B;上的主要OID | **AuthN**&#x200B;上的型別ID | **AuthN的戶長** | **HBA狀態** | **允許在AuthZ**&#x200B;上映象 | AuthN/Z **上的**&#x200B;郵遞區號 | **管道ID於AuthN** | **在AuthN/Z**&#x200B;分級 | **語言** | **onNet** | **inHome** | **附註** |
|-------------------------|---------------------------------------|----------------------|-------------------------------|-----------------------------|--------------------------|----------------------|--------------------------------|----------------|------------------------------|-------------------------|-------------------------|-----------------------|--------------|-----------|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| **正式名稱** | 不適用 | `userID` | `upstreamUserID` | `householdID` | `primaryOID` | `typeID` | `is_hoh` | `hba_status` | `allowMirroring` | `zip` | `channelID` | `maxRating` | `language` | `onNet` | `inHome` |                                                                                                                                           |
| **需要加密** | 不適用 | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| **敏感** | 不適用 | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| Adobe IdP | **是** | **是** | **是** | **是（僅限AuthN）** | **是** | **是** | **是** | **否** | **否** | **是（僅限AuthN）** | **是** | **是（僅限AuthN）** | **否** | **否** | **否** | 不需要法律協定。 |
| Synacor | **是** | **是** | **是** | **是（僅限AuthN）** | **否** | **否** | **是** | **否** | **否** | **是（僅限AuthN）** | **是** | **是（僅限AuthN）** | **否** | **否** | **否** | 法律協定未涵蓋所有代理的MVPD。 這是對Synacor的一般支援，可能不會彙總到其所有MVPD。 |
| 碟子 | **否** | **是** | **是** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是（僅限AuthN）** | **是** | **是（僅限AuthN）** | **否** | **否** | **否** | 它與所有Synacor MVPD共用相同的清單，加上`upstreamUserID`。 |
| Comcast | **否** | **是** | **是** | **是（僅限AuthZ）** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **是（僅限AuthZ）** | **否** | **否** | **否** |                                                                                                                                           |
| AT&amp;T | **是** | **是** | **是** | **是（僅限AuthN）** | **是** | **是** | **否** | **否** | **否** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** | 已簽署法律合約。 |
| DTV | **是** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| COX | **否** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| Cablevision | **是** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **是（僅限AuthN）** | **是** | **否** | **否** | **否** | **否** | 已簽署法律合約。 |
| 頻譜 | **是** | **是** | **是** | **是（僅限AuthN）** | **否** | **否** | **否** | **是** | **是** | **是（僅限AuthN）** | **否** | **是（僅限AuthN）** | **否** | **否** | **否** |                                                                                                                                           |
| 憲章 | **是** | **是** | **是** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是（僅限AuthN）** | **否** | **是（僅限AuthN）** | **否** | **否** | **否** |                                                                                                                                           |
| Verizon | **否** | **是** | **是** | **否** | **否** | **否** | **否** | **是** | **否** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| 宏達國際電子 | **否** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **是** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| 羅傑斯 | **否** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| RCN | **是** | **是** | **是** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是（僅限AuthN）** | **否** | **是（僅限AuthN）** | **否** | **否** | **否** |                                                                                                                                           |
| 東連結 | **否** | **是** | **是** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是（僅限AuthN）** | **是** | **是（僅限AuthN）** | **否** | **否** | **否** |                                                                                                                                           |
| Cogeco | **否** | **是** | **是** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| Videotron | **否** | **是** | **是** | **是*** | **否** | **否** | **否** | **否** | **否** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** | 它公開與`userID`具有相同值的`householdID`。 |
| Proxy Masslon | **是** | **是** | **是** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** | 已簽署法律合約。 |
| Proxy Clearleap | **是** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **是（僅限AuthN）** | **否** | **是（僅限AuthZ）** | **是** | **否** | **否** | 已簽署法律合約。 |
| Proxy GLDS | **否** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **是（僅限AuthN）** | **否** | **否** | **否** | **否** | **否** |                                                                                                                                           |
| 其他MVPD | **否** | **是** | **是** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | **否** | 尚無法律協定，敏感中繼資料不適用於生產。 對於所有MVPD，`userID`不需額外工作即可使用。 |

>[!IMPORTANT]
>
> 必須先與MVPD簽署法律協定，才能使用敏感的使用者中繼資料（例如郵遞區號）。

## 使用者中繼資料加密 {#encryption}

若要加密和解密使用者中繼資料屬性，程式設計師必須產生憑證（公開/私密金鑰組），並透過[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)自行設定](#management)憑證，或與Adobe Pass驗證代表共用公開金鑰。[

請依照下列步驟操作，確認憑證已產生並正確設定：

1. 下載並安裝OpenSSL工具組(http://www.openssl.org)。

1. 產生憑證申請檔(CSR)：

   * 產生金鑰組。 在命令終端機上執行下列動作：

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * 產生CSR。 在命令終端機上執行下列動作：

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     系統會提示您輸入私密金鑰的密碼。

   * 建立您的私密金鑰和密碼的備份復本。 範例CSR：

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. 將CSR傳送給憑證授權單位(CA) （例如Verisign）。

1. CA會以.p7b格式（PKCS#7、密碼編譯訊息語法標準）將憑證傳送給您。

1. 部署.p7b憑證。 使用私密金鑰將PKCS#7 (.p7b)檔案轉換為PKCS#12 （PFX檔案、個人資訊交換語法標準），並產生PEM檔案（串連的憑證容器檔案）：

   * 將PKCS#7檔案轉換為暫存PEM檔案。 在命令列上執行下列動作：

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * 將暫存PEM檔案轉換為PFX檔案。 在命令列上執行下列動作：

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * 將暫存PEM檔案轉換為最終PEM檔案。 在命令列上執行下列動作：

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. 使用PEM檔案來[透過[Adobe Pass TVE Dashboard](https://experience.adobe.com/#/pass/authentication)設定](#management)憑證，或傳送PEM檔案給Adobe Pass驗證代表。

   * 請參閱下一節以取得有關如何透過[Adobe Pass TVE控制面板](https://experience.adobe.com/#/pass/authentication)管理憑證的詳細資料。

   * Adobe Pass驗證同時支援主要和備份憑證。 如果您的主要憑證遭到任何危害，您可以撤銷該憑證，然後切換至次要憑證。 這將確保憑證之間的順利轉換，並對客戶的影響降至最低。

## 使用者中繼資料管理 {#management}

>[!IMPORTANT]
>
> 如果您無法存取Adobe Pass TVE儀表板，請透過我們的[Zendesk](https://adobeprimetime.zendesk.com)建立票證，並要求您的技術客戶經理(TAM)為您進行適當的變更。

Adobe Pass TVE Dashboard是供Adobe Pass驗證客戶（程式設計師）管理其設定和資料的工具。 此自助儀表板可啟用[Adobe Pass TVE儀表板使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)檔案中說明的一系列功能。

若要檢閱和管理MVPD所提供的使用者中繼資料屬性，請依照[TVE整合儀表板使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#user-metadata)檔案中的步驟操作。

若要檢閱和管理用於加密使用者中繼資料屬性的憑證，請依照[程式設計師的TVE儀表板使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#certificates)或[頻道的TVE儀表板使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#certificates)檔案中的步驟操作。

## REST API V2 {#rest-api-v2}

您可以使用以下API擷取使用者中繼資料屬性：

* [擷取設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [擷取特定mvpd的設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [擷取特定程式碼的設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

請參閱上述API的&#x200B;**回應**&#x200B;和&#x200B;**範例**&#x200B;區段，瞭解使用者中繼資料屬性的結構。

>[!IMPORTANT]
>
> 使用者中繼資料在驗證流程完成後即可使用，因此使用者端應用程式不需要查詢個別端點來擷取[使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)資訊，因為它已包含在設定檔資訊中。

如需如何及何時整合上述API的詳細資訊，請參閱下列檔案：

* [主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

在授權流程期間，某些中繼資料屬性可能會根據MVPD和特定的中繼資料屬性而更新。 因此，使用者端應用程式可能需要再次查詢上述API，以擷取最新的使用者中繼資料。

>[!MORELIKETHIS]
>
> [驗證階段常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
