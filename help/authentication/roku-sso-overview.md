---
title: Roku SSO概觀
description: 關於Roku SSO
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# Roku SSO概觀 {#overview}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 簡介 {#roku-sso-intro}

本檔案說明在Roku裝置上利用單一登入功能所需的資訊。 Adobe Pass驗證會與Roku合作，改善登入使用者體驗，並促進電視訂閱者跨所有電視應用程式的單一登入。

此解決方案是以Adobe Pass驗證的無使用者端REST API為基礎，因此大部分程式設計師不需要更新其應用程式即可受益於單一登入。

## 啟用Roku SSO {#enable-roku-sso}

Roku SSO預設為啟用，除非程式設計師或MVPD要求停用SSO。

每個程式設計師都可以在Roku平台上啟用/停用SSO以進行特定整合。

### 程式設計師應該如何做才能讓Roku SSO運作 {#make-roku-sso-work}

對於在使用者端裝置上實作REST API的程式設計師應用程式，Roku SSO將可在沒有任何變更的情況下運作。 Roku OS會在任何要求上新增兩個HTTP標頭至Adobe Pass驗證端點。

若是程式設計師的應用程式為REST API實作Server to Server解決方案，程式設計師應聯絡Roku團隊並要求設定以將這兩個標頭傳送至所有API流程上的網域。

Roku提供的訂閱者ID，用來取代應用程式所傳遞的任何裝置ID，以確保跨應用程式（和跨裝置） SSO。

如需所需標題格式的特定詳細資訊，請聯絡您的Adobe代表。

### 可能的問題 {#possible-issues}

程式設計師應確認其目前根據Adobe的無客戶端REST API進行的實施不會阻礙Roku的平台 — SSO。 請參閱以下的可能問題清單以及應如何解決這些問題。

| 問題 | 可能的原因 | 可能的解決方案 |
|-|-|-|
| 未將Roku SSO標頭傳送至Adobe | 對Adobe Pass驗證網域進行的呼叫使用HTTP而非HTTPS | 使用HTTPS |
| SSO權杖未顯示/未更新MVPD標誌 | UI依賴本機儲存 | 應用程式應在檢查驗證後更新UI （和本機儲存空間，如有需要） |
| 無AuthZ時觸發登出 | 應用程式設計 | 應用程式應更新為永不執行幕後登出 |

## 常見問題集 {#faq}

* **SSO如何運作？**

  SSO將可在與同一Roku使用者相關聯的所有Roku裝置上，使用由Adobe Pass驗證提供支援的所有程式設計人員應用程式。
並非所有MVPD都允許Roku SSO。

* **驗證TTL是否會有任何變更？**

  第一個有效的驗證Token將用於執行SSO，在此情況下，將透過SSO驗證的所有其他應用程式將使用相同的TTL，直到它過期為止。 因此，從一個應用程式導覽至另一個應用程式時，第二個應用程式會共用第一個驗證之應用程式的TTL。

* **其他Adobe功能會如以往般運作嗎？**

  所有Adobe Pass驗證功能都會如常運作。

* **程式設計師的選擇加入/選擇退出程式是否可受益於Roku平台上的SSO？**

  這將是AdobeTVE儀表板中的設定變更。 每個程式設計師都可以在Roku平台上啟用/停用SSO以進行特定整合。
