---
title: 預檢功能，如何啟用、疑難排解或解決問題
description: 預檢功能，如何啟用、疑難排解或解決問題
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 0%

---

# （舊版）預檢功能：如何啟用、疑難排解或解決問題 {#preflight-feature}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

Adobe Pass驗證計算preAuthorizeResources的方式已變更。 PreAuthorization API有新的實作。 此實作會取代僅包含進行多個授權呼叫的舊解決方案。
PreAuthorization API的外部介面未變更，程式設計師的應用程式中不需要更新。

預檢資源的計算方式有三種：

* **將方法分叉並加入MVPD**：這涉及Adobe對MVPD進行多次授權呼叫（使用者端仍須進行一個預檢呼叫）。
* **頻道組合**： MVPD會在SAML驗證回應中公開登入使用者的頻道組合，且Adobe會據此傳回授權資源。 SAML追蹤器中的SAML authN回應應該會公開該清單。
* **多管道授權**：使用者端和Adobe授權兩者都會針對一組資源對MVPD發出單一呼叫。

不論MVPD為何，使用者端應用程式都會對預檢端點(checkPreauthorizedResources API)進行單一呼叫，並傳遞一組資源ID。 根據MVPD支援的上述方法之一，Adobe將傳回預先授權的resourceID。

如果Preflight是以fork &amp; join方法為基礎，則Adobe Pass驗證後端會檢查其設定中「最大預先授權呼叫」的值集。 這是由Adobe設定。

「最大預先授權呼叫」設定的預設值為「5」，這表示在分叉及加入MVPD的Preflight中最多只能傳送5個資源。 傳遞超過5個資源會產生例外狀況，並會傳回null清單。 這是預期行為。 如果MVPD不支援管道排列或多管道授權，我們可以將此設定為任何值，但必須諮詢他們是否為多個分支和加入授權呼叫會增加其載入時間。

因此，在啟用MVPD的Preflight/疑難排解時，請檢視以下事項：

* MVPD支援的方法（分支和聯結、頻道排列或多頻道）。
* 如果只支援分支並加入，則需要詢問程式設計師在Preflight呼叫中將傳送多少資源ID。
* 您需要諮詢MVPD，並需要瞭解建立分叉與加入授權呼叫的「n」個數的影響。 之後，如果值大於5，則必須在config中設定值。

**限制**

請注意，對於某些MVPD （例如AT&amp;T &amp; TWC），如果任何resourceIDs是在預檢呼叫中傳送的resourceID清單中的偽ID或無法辨識的ID，則我們不會從預檢呼叫中取得任何resourceID，即使我們在該清單中也有有效且已授權的資源。
