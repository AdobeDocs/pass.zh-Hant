---
title: 在Adobe Pass驗證量度中使用無使用者端deviceType引數的好處
description: 在Adobe Pass驗證量度中使用無使用者端deviceType引數的好處
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# 在Adobe Pass驗證量度中使用無使用者端deviceType引數的好處 {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>

## 內容

雖然是選用的，但來自無使用者端API的引數`deviceType` （如果存在）用於透過[軟體權利檔案服務監視](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)公開的Adobe Pass驗證度量。

考慮到`deviceType`引數及其Adobe Pass驗證量度上的&#x200B;**優點**&#x200B;之間的連線最初並未陳述，此技術備註的範圍是新增關於它們的更多資訊。

## 說明

自第一個版本以來，`deviceType`引數就存在於無使用者端API中，但其對Adobe Pass驗證量度的影響已新增到較新的版本中。



>[!IMPORTANT]
>
>如果引數`deviceType`設定正確，則在軟體權利檔案服務監視中會有下列&#x200B;**優點**：它提供使用無使用者端時，每個裝置型別](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type)有[劃分的量度，因此可以針對Roku、AppleTV、Xbox等執行不同型別的分析。


如需軟體權利檔案服務監視API的詳細資訊，請參閱[向下鑽研樹狀結構](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md#drill-down_tree)，其中說明ESM 2.0中可用的[維度](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#esm_dimensions) （資源）。

>[!NOTE]
>
>此技術備註的內容也已新增至[無使用者端API](#clientless_device_type)。




## 實施

為了充分利用Adobe Pass驗證量度，目前使用中有2種型別的[無使用者端API](#web_srvs_summary)，且需要設定正確的`deviceType`：

1. 以`regcode`作為必要引數的API，將透過下列API呼叫使用建立`regcode`時設定的`deviceType`引數：
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. 以`deviceType`作為選用引數的API：
   - [\&lt;SP\_FQDN\>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/authorize](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/media](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logout](#init_logout)

我們的建議是使用`deviceType`引數，並為所有API傳遞正確的無使用者端裝置型別。
