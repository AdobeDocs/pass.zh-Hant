---
title: 瞭解使用者ID
description: 瞭解使用者ID
exl-id: 813a8501-db72-4850-a387-c8db6120db80
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 0%

---

# （舊版）瞭解使用者ID {#understanding-user-ids}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

在概念上，每個起始權利流程的使用者都與單一不重複的使用者ID相關聯。 不過，在軟體權利檔案流程的過程中，該使用者ID可能會以不同方式呈現，端視您從哪個API取得該ID而定。

短媒體權杖中的`sessionGUID`是使用者識別碼的安全形式，可透過`sendTrackingData()`呼叫取得。 在目前的所有整合中，這是使用者在不同時間和裝置上的永久GUID。 GUID的來源以來自MVPD之驗證回應的使用者ID開頭。 不過，有些MVPD未來可能會改變心意，並開始傳送暫時GUID。 如果程式設計師想要確保AuthN回應中的MVPD來源使用者ID是永久性的，他們應在與MVPD的合約中安排此ID。

以下是Adobe Pass驗證API中表示使用者ID的不同方式：

| 屬性 | 用途 | 雜湊 | 數位簽署 | 說明 |
| --- | --- | --- | --- | --- |
| sendTrackingData() GUID屬性 | 追蹤/分析 | 是 | 否 | - MVPD使用者ID，依Adobe進行雜湊處理。 使用者識別碼無法回溯至MVPD的來源。</br> </br> — 此形式的ID未經過數位簽署，因此防欺詐並不安全。 不過，這非常適合用於分析。 </br> </br> — 此形式的使用者ID是在Adobe Pass驗證在AuthN/AuthZ流程中產生的所有事件上從使用者端提供。 |
| 短媒體權杖的sessionGUID屬性 | 追蹤同時使用的詐騙專案 | 是 | 是 |  — 這和透過sendTrackingData()的使用者ID相同，不過這個ID經過數位簽章以保護其完整性，而且足以用於詐騙追蹤。</br> </br> — 此視訊會在使用我們的驗證程式庫後，在伺服器端處理，而且可以在發佈視訊資料流給使用者端之前，分析是否有詐騙模式。  這些工作的執行由程式設計師決定。 |
| getMetadata() userID屬性 | 帳戶連結、與MVPD的詐騙調查 | 否 | 否 |  — 此屬性可讓Adobe將實際來源MVPD使用者ID公開給程式設計師。</br> </br> — 在Adobe的設定中，可將其設定為加密或不加密(視MVPD偏好設定而定)。 如果已加密，則會使用提供給Adobe之程式設計師憑證的公開金鑰加以加密，因此使用者端不會明確看到該金鑰。</br> </br> — 這可讓程式設計師取得來自MVPD的實際使用者ID，因此可直接用來與MVPD連結帳戶或進行詐騙調查。 |


**結論**

* 一般而言，MVPD會提供永續性唯一識別碼<u>，並在成功驗證</u>時將其傳遞給Adobe。 通常在所有網路上都是一致的。 Comcast MVPD則屬於例外，它為每個頻道提供不同的使用者ID。

* MVPD使用者ID不包含PII，且不是帳號。 它不需要以加密形式公開，因為我們已透過所有MVPD驗證未傳送任何PII。

使用者ID的使用方式取決於使用案例：

* 如果您需要它來追蹤/分析，最實用的方式是從`sendTrackingData()`取得它。
* 若您在伺服器端需要串流發佈、詐騙或操作資料的憑證，您可以從Media Token驗證器取得。
* 如果您因帳戶連結和較深的詐騙行為而需要它，請洽詢您的Adobe聯絡人以取得可用性。
