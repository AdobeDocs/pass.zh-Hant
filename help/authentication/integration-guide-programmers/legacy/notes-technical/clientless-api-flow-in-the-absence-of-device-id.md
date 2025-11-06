---
title: 缺少裝置ID時的無使用者端API流程
description: 缺少裝置ID時的無使用者端API流程
exl-id: 6549a6d6-03a9-4d95-99fb-d3ada832323d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# （舊版）缺少裝置ID時的無使用者端API流程 {#clientless-api-flow-in-the-absence-of-device-id}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

</br>


## 問題

並非所有智慧型裝置應用程式都能提供唯一的裝置ID。  由於deviceId是必要引數，若未傳遞，服務會傳回400錯誤。


## 臨時解決方案/因應措施

對於沒有裝置ID的使用者端：

1. 第一次使用`deviceId=dummy`呼叫註冊代碼服務
1. 從回應中擷取UUID。 UUID可在註冊代碼回應（XML和JSON回應格式）的「id」元素中使用。
1. 再次致電註冊服務。 這次傳遞`deviceId=<uuid obtained in step #2>`
1. 在主控台UI上顯示步驟3中取得的註冊代碼


完成這些步驟後，Adobe Pass驗證會使用UUID做為裝置ID。 將此裝置ID (UUID)儲存在裝置的本機儲存空間中。 萬一使用者產生新的註冊代碼，您應再次執行步驟1到4，然後將先前儲存的裝置ID (UUID)取代為新裝置ID。



## 永久解決方案

在未來版本中，Adobe會變更，方法是在建立登入程式碼時，將`deviceId`設為選用裝載，並在`deviceId`不存在時，使用UUID做為權杖金鑰而非`deviceId`。

<!--
## Related Information

- [Clientless API Reference](/help/authentication/rest-api-reference.md)
-->
