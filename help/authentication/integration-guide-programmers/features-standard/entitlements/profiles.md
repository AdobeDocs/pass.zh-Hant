---
title: 設定檔
description: 設定檔
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# 設定檔 {#profiles}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

當使用者透過付費電視提供者(MVPD)成功驗證時，設定檔是由Adobe Pass驗證[REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)所建立。

設定檔型別依所使用的驗證方法而異：

* **一般**

  透過基本驗證建立。

* **Apple SSO**

  使用Apple的視訊訂閱者帳戶架構，透過單一登入(SSO)建立。

* **平台SSO**

  使用平台身分透過單一登入(SSO)建立。

* **服務權杖SSO**

  使用服務權杖透過單一登入(SSO)建立。

設定檔會儲存讓使用者端應用程式能夠：

* 判斷使用者的驗證狀態。
* 識別所使用的驗證方法。
* 識別身分提供者。
* 存取[使用者中繼資料](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)。

設定檔會安全地儲存在Adobe Pass驗證的後端，並連結至請求的應用程式、裝置和服務提供者識別碼。 它們在有限的時間範圍內保持有效，如[驗證存留時間(TTL)](#authentication-ttl-management)所定義。

## 驗證存留時間(TTL)管理 {#authentication-ttl-management}

驗證存留時間(TTL)會定義使用者在需要重新驗證之前保持驗證狀態的時間。 此時間範圍是有限的，必須與MVPD代表商定。 TTL值可能因以下因素而異：

* 平台類別（例如桌上型電腦、行動裝置、電視連線裝置）
* 特定平台(例如iOS、Android、tvOS、Roku、FireTV)

您的組織管理員或代表您行事的Adobe Pass驗證代表可透過Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)檢視及變更驗證(authN) TTL。

如需詳細資訊，請參閱[TVE儀表板整合使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows)檔案。

## REST API V2 {#rest-api-v2}

您可以使用以下API擷取設定檔：

* [擷取設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [擷取特定mvpd的設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [擷取特定程式碼的設定檔](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

請參閱上述API的&#x200B;**回應**&#x200B;和&#x200B;**範例**&#x200B;區段，瞭解設定檔的結構。

如需如何及何時整合上述API的詳細資訊，請參閱下列檔案：

* [主要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [在次要應用程式內執行的基本設定檔流程](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

>[!MORELIKETHIS]
>
> [驗證階段常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
