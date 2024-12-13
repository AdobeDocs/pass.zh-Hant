---
title: AccessEnabler iOS/tvOS 3.7.0升級路徑
description: AccessEnabler iOS/tvOS 3.7.0升級路徑
exl-id: f15c7414-ec9b-4e21-b457-1ecf59f47441
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# （舊版） AccessEnabler iOS/tvOS 3.7.0升級路徑 {#accessenabler-iostvos-370-upgrade-path}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

</br>

來自[新AccessEnabler 3.7.0](/help/authentication/notes-releases/authn-rn-ios-tvos-370.md)版的鑰匙圈儲存變更，與AccessEnabler 3.7.0以下版本的鑰匙圈儲存實作不相容。

一個採用新AccessEnabler 3.7.0版的應用程式的升級路徑，會從舊版鑰匙圈儲存移轉所有代號。 因此，在AccessEnabler架構更新程式期間，使用者&#x200B;**不應該遺失驗證/授權工作階段**。

## 已知限制

實作人員可能會遇到如下所述的一些限制。


1. 一般(Adobe) SSO在使用AccessEnabler 3.7.0版的應用程式與使用AccessEnabler 3.7.0以下版本的應用程式之間無法運作，即使對於同一廠商所開發的應用程式亦然。

   >[!IMPORTANT]
   >
   >* 系統層級(Apple) SSO不受影響！
   >
   >* 如果兩個應用程式都是由同一家廠商開發，且使用3.7.0以下的AccessEnabler版本，一般(Adobe)SSO將繼續運作！
   >
   >* 如果兩個應用程式都是由同一家廠商開發，並且使用AccessEnabler 3.7.0版，一般(Adobe)SSO將可正常運作！


1. 如果使用AccessEnabler 3.7.0版將某個應用程式降級為較低版本的AccessEnabler，則不會移轉新產生的權杖。 因此，一般使用者可能會遇到驗證/授權工作階段遺失的情況，而未預期會遺失。

   >[!IMPORTANT]
   >
   >* 透過系統層級(Apple) SSO驗證的一般使用者不受影響！
   >* 使用AccessEnabler 3.7.0版更新至新應用程式前已驗證的一般使用者不會受到影響！

1. 如果使用AccessEnabler 3.7.0版將某個應用程式降級為較低版本的AccessEnabler，則不會確認已刪除的權杖。 因此，一般使用者可能會體驗到驗證/授權工作階段的存在，而不需要它。
