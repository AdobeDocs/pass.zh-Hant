---
title: 在Adobe Pass驗證量度中使用無使用者端deviceType引數的好處
description: 在Adobe Pass驗證量度中使用無使用者端deviceType引數的好處
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 0%

---

# 在Adobe Pass驗證量度中使用無使用者端deviceType引數的好處 {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>

## 內容

雖然是選用專案，但引數 `deviceType` 來自無使用者端API （如果存在）會用於透過公開的Adobe Pass驗證量度 [軟體權利檔案服務監視](/help/authentication/entitlement-service-monitoring-overview.md).

考量以下兩者之間的連線： `deviceType` 引數及其 **優點** 關於最初沒有說明的Adobe Pass驗證量度，本技術說明的範圍是新增更多關於它們的資訊。

## 說明

此 `deviceType` 自第一個版本以來，無使用者端API中就存在引數，但最近發行的版本已新增其對Adobe Pass驗證量度的關聯。



>[!IMPORTANT]
>
>若引數 `deviceType` 已正確設定，則有下列專案 **優點** 在軟體權利檔案服務監視中：它提供以下量度 [依裝置型別劃分](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) 使用無使用者端時，因此可針對Roku、AppleTV、Xbox等執行不同型別的分析。


如需軟體權利檔案服務監控API的詳細資訊，請參閱 [向下鑽研樹狀結構、](/help/authentication/entitlement-service-monitoring-api.md#drill-down_tree) 這說明 [維度](/help/authentication/entitlement-service-monitoring-overview.md#esm_dimensions) （資源）可在ESM 2.0中使用。

>[!NOTE]
>
>此技術備忘稿的內容也已新增至 [無使用者端API](#clientless_device_type).




## 實施

若要充分運用Adobe Pass驗證量度，共有兩種型別 [無使用者端API](#web_srvs_summary) 目前正在使用且需要具備正確設定檔的 `deviceType` 設定：

1. API具有 `regcode` 作為必要引數，和將會使用 `deviceType` 建立時設定的引數 `regcode`，搭配下列API呼叫：
   - [\&lt;reggie _fqdn=&quot;&quot;>/reggie/v1/{requestorId}/regcode](#reg_serv)

1. 具有下列專案的API： `deviceType` 作為選用引數：
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/authorize](#init_authz)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/tokens/media](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/mediatoken](#short_media)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;sp _fqdn=&quot;&quot;>/api/v1/logout](#init_logout)

我們建議使用 `deviceType` 引數，並為所有API傳遞正確的無使用者端裝置型別。
