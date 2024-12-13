---
title: 憑證問答
description: 憑證問答
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# （舊版）憑證常見問答 {#certificates-q}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

</br>

**問題1：**&#x200B;是否可以跨iOS和Android註冊憑證？

**A：** iOS和Android的憑證在目前的設定中是相同的。 原生憑證可用於兩種平台。

</br>

**問題2：**&#x200B;相同的iOS憑證能否在生產與中繼環境中作為主要與備份憑證使用？ 如果不建議使用，您可以提供說明嗎？

**A：**&#x200B;設定與主要和備份憑證相同的憑證沒有意義。 我們擁有主要和備份憑證的概念，因此我們可以為程式設計師設定多個憑證，以備主要憑證過期或被撤銷時使用。 擁有備份憑證可讓程式設計師有時間變更主要憑證，而不會影響發行環境。 但您可以對生產設定檔和中繼設定檔使用相同的一組主要和備份憑證。

</br>

**問題3：**&#x200B;使用新彈性TempPass的網頁是否需要新的憑證？

**A：**&#x200B;憑證（以及任何憑證）是在Media Company &amp; Programmer層級設定。 FlexibleTempPass是MVPD，您不需要為其設定任何憑證，因此，如果您整合現有的程式設計師與彈性的TempPass，將會使用已在程式設計師/媒體公司層級設定的憑證。
