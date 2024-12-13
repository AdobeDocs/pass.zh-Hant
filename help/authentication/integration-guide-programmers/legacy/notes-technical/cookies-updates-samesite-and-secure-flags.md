---
title: Cookie更新 — SameSite和Secure標幟
description: Cookie更新 — SameSite和Secure標幟
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '956'
ht-degree: 0%

---

# （舊版） Cookie更新 — SameSite和Secure標幟 {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

</br>


## 更新 {#Updates}

本節重點說明Chrome瀏覽器和Adobe Pass驗證所推出的處理第三方Cookie的變更。



### Chrome 80更新 {#Chrome}

從Chrome 80版（82版除外）開始，未指定&#x200B;*SameSite*&#x200B;屬性的Cookie會被視為是&#x200B;*SameSite=Lax*。 因此，需要在跨網站內容中傳遞的Cookie必須明確指定&#x200B;*SameSite=None*，且必須標示&#x200B;*Secure*&#x200B;屬性並透過&#x200B;*HTTPS*&#x200B;傳遞。 如需這些更新的詳細資訊，請參閱官方Chromium頁面： <https://www.chromium.org/updates/same-site>以及<https://web.dev/samesite-cookies-explained/>。


### Adobe Pass驗證更新 {#Pass-Updates}

Adobe Pass Authentication Service目前仰賴從瀏覽器觀點視為第三方Cookie的一些Cookie (包括Chrome)，以便與某些平台和版本的Adobe Pass Authentication SDK搭配運作。 因此，為了符合即將到來的變更並繼續從這些舊版SDK在跨網站內容中傳送這些Cookie，Adobe Pass驗證服務會在&#x200B;*adobe-pass-2.55.1*&#x200B;版本中實作必要的變更。

從&#x200B;*adobe-pass-2.55.1*&#x200B;版進行的這些變更，涉及在使用80版及以上版本的Chrome瀏覽器（82版除外）時，為傳回至所有Adobe Pass驗證SDK的所有Cookie新增&#x200B;*Secure*&#x200B;和&#x200B;*SameSite=None*&#x200B;屬性。

下一節將列出平台和Adobe Pass Authentication SDK版本的一些潛在問題，以防一位使用者使用Chrome瀏覽器80及更高版本（版本82除外）。

## 疑難排解 {#Troubleshooting}

瀏覽本節時，請記住，所有Adobe Pass驗證服務Cookie都必須針對所有瀏覽器在&#x200B;*adobe-pass-2.55.1*&#x200B;版本中設定&#x200B;*安全*&#x200B;屬性，而&#x200B;*SameSite=None*&#x200B;屬性僅針對Chrome 80版及更高版本（82版除外）設定。


### 一般疑難排解 {#General}

1. 請注意，某些使用者代理程式已知與&#x200B;*SameSite=None*&#x200B;屬性不相容。

   - 從Chrome 51到Chrome 66的Chrome版本（兩端包含）。 這些Chrome版本將拒絕&#x200B;*SameSite=None*&#x200B;的Cookie。 這也會影響舊版的Chromium衍生瀏覽器以及Android WebView。 根據Cookie當時的規格版本，此行為正確，但隨著規格中新增了「無」值，此行為已在Chrome 67及更新版本中更新。 (在Chrome 51之前，SameSite屬性會被完全忽略，所有Cookie都會被視為是&#x200B;*SameSite=None*。)
   - Android 12.13.2版之前的UC瀏覽器版本。較舊版本會拒絕&#x200B;*SameSite=None*&#x200B;的Cookie。 根據當時Cookie規格的版本，此行為正確，但規格中新增了「無」值，因此較新版本的UC瀏覽器已更新此行為。
   - macOS 10.14上的Safari和嵌入式瀏覽器版本，以及iOS 12上的所有瀏覽器。 這些版本會錯誤地將&#x200B;*SameSite=None*&#x200B;標籤的Cookie視為標籤為&#x200B;*SameSite=Strict*。 較新版本的iOS和MacOS已修正此錯誤。


1. 需要注意的是，具有&#x200B;*Secure*&#x200B;屬性的Cookie必須透過&#x200B;*HTTPS*&#x200B;傳送，否則該Cookie將無法連線Adobe Pass驗證服務。

   - AccessEnabler JavaScript SDK：
      - 在引入動態使用者端註冊之前，與&#x200B;*sp.auth.adobe.com*&#x200B;的通訊必須使用&#x200B;*HTTPS* （針對版本&#x200B;*2.35*&#x200B;和&#x200B;*3.5.0*）。
   - AccessEnabler iOS/tvOS SDK：
      - 引入動態使用者端註冊之前，與&#x200B;*sp.auth.adobe.com*&#x200B;的通訊必須針對&#x200B;*3.0.0*&#x200B;之前的版本使用&#x200B;*HTTPS*。
   - AccessEnabler Android SDK：
      - 引入動態使用者端註冊之前，與&#x200B;*sp.auth.adobe.com*&#x200B;的通訊必須針對&#x200B;*3.0.0*&#x200B;之前的版本使用&#x200B;*HTTPS*。
   - AccessEnabler FireOS SDK：
      - 與&#x200B;*sp.auth.adobe.com*&#x200B;的通訊必須使用&#x200B;*HTTPS* （版本&#x200B;*2.0.4*）。

</br>

### AccessEnabler JavaScript SDK 2.35版疑難排解 {#235-Troubleshooting}

在Chrome 80及更高版本（82版除外）中，使用者的驗證流程可能會受到影響。 為了確保使用者不會因上述更新而遇到驗證問題，您可以：

- 檢查瀏覽器中是否已設定&#x200B;*JSESSIONID* Cookie，以及是否已設定&#x200B;*SameSite=None*&#x200B;和&#x200B;*Secure*&#x200B;屬性。
- 檢查&#x200B;*https://sp.auth.adobe.com/authenticate/saml*&#x200B;網路請求中的&#x200B;*JSESSIONID* Cookie是否符合來自&#x200B;*https://sp.auth.adobe.com/session*&#x200B;網路請求的&#x200B;*JSESSIONID* Cookie。


### AccessEnabler JavaScript SDK 3.5.0版疑難排解 {#350-Troubleshooting}

在Chrome 80及更高版本（82版除外）中，使用者的驗證流程可能會受到影響。 為了確保使用者不會因上述更新而遇到驗證問題，您可以：

- 檢查瀏覽器中是否已設定&#x200B;*JSESSIONID* Cookie，以及是否已設定&#x200B;*SameSite=None*&#x200B;和&#x200B;*Secure*&#x200B;屬性。
- 檢查&#x200B;*https://sp.auth.adobe.com/authenticate/saml*&#x200B;網路請求中的&#x200B;*JSESSIONID* Cookie是否符合來自&#x200B;*https://sp.auth.adobe.com/session*&#x200B;網路請求的&#x200B;*JSESSIONID* Cookie。
- 檢查瀏覽器中是否已設定&#x200B;*pass\_sfp* Cookie，以及是否已設定&#x200B;*SameSite=None*&#x200B;和&#x200B;*Secure*&#x200B;屬性。
- 檢查&#x200B;*pass\_sfp* Cookie是否已設定在&#x200B;*https://sp.auth.adobe.com/session*&#x200B;網路要求中。


在Chrome 80及更高版本（82版除外）中，使用者的授權流程可能會受到影響。 為了確保使用者在成功通過驗證後沒有觀看受保護資源的困難，您可以透過以下方式更新資源：

- 檢查瀏覽器中是否已設定&#x200B;*pass\_sfp* Cookie，以及是否已設定&#x200B;*SameSite=None*&#x200B;和&#x200B;*Secure*&#x200B;屬性。
- 檢查&#x200B;*pass\_sfp* Cookie是否已設定在&#x200B;*https://sp.auth.adobe.com/adobe-services/authorize*&#x200B;網路要求中。
- 檢查&#x200B;*pass\_sfp* Cookie是否已設定在&#x200B;*https://sp.auth.adobe.com/adobe-services/shortAuthorize*&#x200B;網路要求中。
