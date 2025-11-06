---
title: 程式設計師快速入門手冊
description: 程式設計師快速入門手冊
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# 程式設計師快速入門手冊 {#programmer-kickstart-guide}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

本Kickstart指南適用於計畫將Adobe®傳遞驗證整合至其網站或應用程式的內容提供者（程式設計人員）。

本檔案概述確保順利且有效率地開始整合程式的重要初始步驟。 此課程旨在釐清客戶的期望，並指引我們與合作夥伴協力達成成功整合。

Adobe提供一系列資源，協助您將Adobe Pass驗證整合至您的網站或應用程式。 請參考&#x200B;**「您將提供」**&#x200B;及&#x200B;**「Adobe將會提供」**&#x200B;以下各節的提及內容。

## 設定程式 {#setup-process}

設定程式包括下列步驟：

![Adobe®通過驗證整合程式](../assets/progr-flow-int-lifecycle.png)

*Adobe®通過驗證整合程式*

**您將在啟動階段提供**：

* **服務提供者（要求者識別碼）**

  這是字串，可唯一識別向Adobe Pass驗證提出要求的網站或應用程式的品牌。 字串本身為任意字串，但需在Adobe與程式設計師之間商定

* **頻道資訊**

  這是一組字串，用來識別服務提供者要求的內容管道。 在許多情況下，頻道與服務提供者是一樣的。 然而，單一識別碼可以代表多個內容管道。 這些頻道名稱字串應該與對應的有線電視頻道一致。 請注意，某些MVPD可能會在驗證和/或授權程式期間驗證此值。

* **網域名稱**

  此清單將包含實際列在Adobe中的網域名稱，以代表服務提供者。 這樣可確保只有您授權的網域才能使用您的中繼資料存取Adobe Pass驗證。 請務必提供並清楚識別生產和中繼（測試）環境的網域名稱，因為這些名稱可能不同。

**您將透過MVPD提供**：

* **認證集**

  這些憑證用於透過MVPD驗證和授權使用者，或僅驗證使用者。 一般而言，這些認證包含使用者名稱和密碼，MVPD會針對設定檔（測試環境和生產環境）提供這兩個專案。

* **資源識別碼**

  這些是服務提供者想要保護的內容頻道、節目、集數或資產的唯一識別碼。 這些識別碼用於請求授權決策，且必須與MVPD商定。

>[!IMPORTANT]
>
> 程式設計師負責與MVPD協調，以完成任何必要的業務協定。 同時，Adobe Pass驗證將與MVPD共同作業，以確保技術整合已正確建立：
>
> * **新MVPD**
>
>     如果MVPD未與Adobe整合，則必須根據MVPD的特定需求來開發自訂程式碼。 在此開發完成之前，將無法使用MVPD，並且無法繼續使用該MVPD進行產品測試。
>
> * **現有的MVPD**
>
>     如果MVPD已與Adobe整合，可大幅簡化連線流程。 在多數情況下，連線能力可透過設定調整快速建立，無需進行大量開發。
>
> 所有整合都需要共同品質保證(QA)工作，包括MVPD的測試，因為一般使用者最終是MVPD的客戶。 協調測試週期通常取決於MVPD的資源可用性，這可能會導致延遲。

## 存取客戶支援 {#access-customer-support}

**Adobe將透過** Zendesk[提供](https://tve.zendesk.com/home)我們的客戶支援系統存取權。 若要存取Zendesk，您必須在https://tve.zendesk.com/home註冊並建立帳戶。 您可以註冊的使用者數目沒有限制。 註冊後，您可以在任何提交的票證上檢視和分享註解。

Adobe Pass驗證團隊可協助您在整合過程中遇到任何問題或技術問題。 請透過[tve-support@adobe.com](mailto:tve-support@adobe.com)聯絡我們。

## 存取檔案 {#access-documentation}

**Adobe將透過** Adobe Experience League[提供](https://experienceleague.adobe.com/en/docs/pass/authentication/home)對公開檔案的存取權。

Adobe Pass驗證團隊針對[程式設計師整合指南](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md)區段下的可用功能和API提供完整檔案。 請參考本節下的目錄，以取得每個主題的詳細資訊連結。

## 存取測試工具 {#access-testing-tool}

**Adobe將透過** Adobe Developer[網站提供](https://developer.adobe.com/adobe-pass/)我們的API探索工具存取權。

## 存取設定管理工具 {#access-configuration-management-tool}

**Adobe將提供**&#x200B;存取自助服務工具，以透過[Adobe Pass TVE Dashboard](https://experience.adobe.com/pass/authentication)管理您的設定和資料。

Adobe Pass驗證團隊在[TVE控制面板使用手冊](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)區段下，提供使用TVE控制面板的完整檔案。 請參考本節下的目錄，以取得每個主題的詳細資訊連結。
