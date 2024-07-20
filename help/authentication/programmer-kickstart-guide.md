---
title: 程式設計師快速入門手冊
description: 程式設計師快速入門手冊
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '972'
ht-degree: 0%

---

# 程式設計師快速入門手冊 {#programmer-kickstart-guide}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 簡介 {#prog-kickstart-guide-intro}

歡迎使用Adobe Pass Authentication for TV Everywhere。 我們期待與您合作。

>[!NOTE]
>
>這是程式設計師（內容提供者）的Kickstart指南。 如果您是多頻道視訊程式設計經銷商(MVPD)，請務必參閱[MVPD快速入門手冊](/help/authentication/mvpd-kickstart-guide.md)。


Adobe Pass驗證聯絡人：

* 支援 — 針對所有問題、事件或功能要求，`tve-support@adobe.com`
* 您的專案開始之前，系統會將「啟用」聯絡人指派給專案。

下列資訊概述讓我們開始穩定有效率工作的一些重要步驟。 我們的目標是針對我們如何與合作夥伴合作達成整合提供說明和期望。 請注意「您將提供」/「Adobe將會提供」下方的各個專案章節。 這些清單會以檢查清單的方式列出，或在我們完成專案時提供指南。

本檔案假設程式設計師已註冊與選定的MVPD合作夥伴合作。

## 發行排程 {#release-schedule}

已規劃Adobe快速入門開發週期，因此您可以檢視排程發行的時間，以及如何透過我們的開發系統推廣每個版本。

每個衝刺完成之後，即可進行QE，或在UAT伺服器上看到新的實作。

大約每6週會對Adobe「資格預審」伺服器發佈一次。 （這是執行最終QE時，我們保留建議下一個版本的伺服器。） 這些組建將包含自上次放置以來在短跑中完成的所有工作。 此時，合作夥伴可使用兩週QE視窗來測試此版本。

假設在前兩個星期的測試期間沒有發生嚴重問題，此版本將會升級至即時生產。 這表示整合可在Adobe發行環境中使用，但合作夥伴會在公開發行時選擇。

<!--For the latest release schedule information, see the Release Calendar.-->

## 支援檔案 {#supp-doc}

Adobe將提供：

* 部署指南： **`https://tve.zendesk.com/entries/498741-tve-deployment-guide`**
* 存取我們的Zendesk客戶支援系統。 您也可以在這裡找到有關某些流程的範例、資訊和影片教學課程。 若要在Zendesk上存取此檔案以及張貼在該處的其他檔案，您必須在`https://tve.zendesk.com/home`註冊並建立帳戶。 您可以註冊的使用者數量沒有限制。  您可以在任何已歸檔票證上檢視和共用評論。 所有支援問題皆應傳送給`tve-support@adobe.com`。
* [程式設計師整合指南](/help/authentication/programmer-integration-guide-overview.md)
* 媒體權杖驗證器程式庫： `https://tve.zendesk.com/entries/471323-media-token-validator-library`。

## 測試環境設定 {#test-env-setup}

Adobe會先使用Adobe測試網站設定您，其中Adobe會作為測試用途的MVPD。 您的團隊接著可以設定測試網站，以呼叫AdobeAPI。 使用預設的MVPD選取器，並選取「Adobe」作為idP。

您將提供：

1. 要求者ID。 這是字串，可唯一識別向Adobe Pass驗證提出要求的網站或應用程式的品牌。 字串本身是任意的，但需要在Adobe和程式設計師之間商定
1. 頻道資訊。 這是一組字串，可識別要求者ID所要求的內容管道。 在許多情況下，管道和請求者ID相同。 但同一ID可能會要求您擁有多個內容管道。 頻道名稱字串應該與有線電視頻道相對應。 有些MVPD會透過AuthN和/或AuthZ通訊協定驗證此值。
1. 網域名稱（允許用於該請求者ID）。 這將是一個實際網域名稱清單，將由Adobe列出以接受請求者ID。 這樣可確保只有您核准的網域才能存取含有您中繼資料的Adobe Pass驗證。 注意：對生產有效的網域名稱在測試/測試中可能不同，應該提供和識別這兩個名稱。

Adobe將設定帳戶，而Adobe將提供：

* 存取測試網站的登入和密碼

## 使用MVPD進行設定 {#setup-mvpd}

本節說明當您從Adobe測試網站移轉至MVPD使用時，需要哪些專案。

您將提供（透過MVPD）：

* **兩組認證**：
   * AuthN + AuthZ ：已驗證和授權使用者的登入/密碼
   * AuthN + Non-AuthZ ：已驗證但未授權的使用者登入/密碼
* **資源識別碼**。 這是將會透過AuthZ通訊協定以MVPD驗證的特定內容識別碼。 可以在頻道、節目、集數或資產層級，應該與您的MVPD商定。

Adobe Pass驗證支援MRSS型中繼資料結構，這表示資源ID可以視需要而具體，且可包含特定MVPD的唯一識別碼。

**新的MVPD整合**：請務必記住，您選擇的MVPD在完成任何整合時都會發揮不可或缺的作用。 Adobe需要根據每個MVPD的規格為其撰寫程式碼。 在這些步驟完成之前，您將無法從對話方塊中選取該MVPD，或完成您的產品測試。 Adobe需要提前排程這項工作，以配合下一個可用的衝刺。 （如需目前排程資訊，請參閱發行行事曆。）

**現有的MVPD整合**：如果您選擇的MVPD已使用Adobe設定，則連線步驟應該會簡單得多（更快），而且通常可透過變更設定來達成連線。

>[!NOTE]
>
>MVPD仍需啟用程式設計師，並簽署任何相關的商業交易。

使用MVPD的&#x200B;**QE**：所有整合都將涉及聯合QE，由於一般使用者最終是MVPD的客戶，因此許多使用者在推送「即時」之前已設定測試週期。 由於這涉及MVPD資源的排程，因此這是可能的延遲區域。

<!--
>[RELATEDINFORMATION]
>[MVPD Kickstart Guide](help\authentication\mvpd-kickstart-guide.md)
-->
