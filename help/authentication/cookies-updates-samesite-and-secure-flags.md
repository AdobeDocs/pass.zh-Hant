---
title: Cookie更新 — SameSite和Secure標幟
description: Cookie更新 — SameSite和Secure標幟
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 2dbb45aebb1a00863a9344114963f6df95763dfc
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 0%

---

# Cookie更新 — SameSite和Secure標幟 {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>


## 更新 {#Updates}

本節重點說明Chrome瀏覽器和Adobe Pass驗證所引進的處理第三方Cookie的變更。



### Chrome 80更新 {#Chrome}

從Chrome 80版（82版除外）開始，未指定 *SameSite* 屬性會被視為是 *SameSite=Lax*. 因此，需要在跨網站內容中傳送的Cookie必須明確指定 *SameSite=None*，且也必須標示 *安全* 屬性並傳遞到 *HTTPS*. 如需這些更新的詳細資訊，請參閱官方Chromium頁面： <https://www.chromium.org/updates/same-site> 以及來自 <https://web.dev/samesite-cookies-explained/>.


### Adobe Pass驗證更新 {#Pass-Updates}

Adobe Pass Authentication Service目前仰賴從瀏覽器角度視為第三方Cookie的一些Cookie （包括Chrome），以便與某些平台和版本的Adobe Pass Authentication SDK搭配運作。 因此，為符合即將到來的變更，以及繼續在跨網站內容中，從這些舊版SDK傳遞這些Cookie，Adobe Pass驗證服務會在中實作必要的變更 *adobe-pass-2.55.1* 版本。

這些變更來自 *adobe-pass-2.55.1* 版本包含新增 *安全* 和 *SameSite=None* 使用從版本80及更高版本（除了版本82）開始的Chrome瀏覽器時，其所有Cookie的屬性會傳回至所有Adobe Pass驗證SDK。

下一節將列出平台和Adobe Pass Authentication SDK版本的一些潛在問題，以防一位使用者使用Chrome瀏覽器80及更高版本（版本82除外）。

## 疑難排解 {#Troubleshooting}

瀏覽本節時，請記住，所有Adobe Pass驗證服務Cookie都必須 *安全* 屬性設定於 *adobe-pass-2.55.1* 適用於所有瀏覽器的版本，而 *SameSite=None* 屬性僅可針對Chrome 80版及更新版本（82版除外）設定。


### 一般疑難排解 {#General}

1. 請務必注意，有些使用者代理程式已知與 *SameSite=None* 屬性。

   - 從Chrome 51到Chrome 66的Chrome版本（兩端包含）。 這些Chrome版本會拒絕Cookie，使用 *SameSite=None*. 這也會影響舊版的Chromium衍生瀏覽器以及Android WebView。 根據Cookie當時的規格版本，此行為正確，但隨著規格中新增了「無」值，此行為已在Chrome 67及更新版本中更新。 (在Chrome 51之前，SameSite屬性會被完全忽略，所有Cookie都會被視為是 *SameSite=None*.)
   - Android 12.13.2版之前的UC瀏覽器版本。舊版會拒絕包含下列專案的Cookie： *SameSite=None*. 根據當時Cookie規格的版本，此行為正確，但規格中新增了「無」值，因此較新版本的UC瀏覽器已更新此行為。
   - macOS 10.14上的Safari和嵌入式瀏覽器版本，以及iOS 12上的所有瀏覽器。 這些版本會錯誤處理以標籤的Cookie *SameSite=None* 就像已標籤一樣 *SameSite=嚴格*. 較新版本的iOS和MacOS已修正此錯誤。


1. 請注意，Cookie具有 *安全* 屬性必須傳送到 *HTTPS*，否則Cookie將不會連線Adobe Pass驗證服務。

   - AccessEnabler JavaScript SDK：
      - 強制與通訊 *sp.auth.adobe.com* 使用 *HTTPS* 適用於版本 *2.35* 和 *3.5.0*，然後才開始匯入動態使用者端註冊。
   - AccessEnabler iOS/tvOS SDK：
      - 強制與通訊 *sp.auth.adobe.com* 使用 *HTTPS* 適用於之前的版本 *3.0.0*，然後才開始匯入動態使用者端註冊。
   - AccessEnabler Android SDK：
      - 強制與通訊 *sp.auth.adobe.com* 使用 *HTTPS* 適用於之前的版本 *3.0.0*，然後才開始匯入動態使用者端註冊。
   - AccessEnabler FireOS SDK：
      - 強制與通訊 *sp.auth.adobe.com* 使用 *HTTPS* 適用於版本 *2.0.4*.

</br>

### AccessEnabler JavaScript SDK 2.35版疑難排解 {#235-Troubleshooting}

Chrome 80及更高版本（82版除外）中可能會影響使用者的驗證流程。 為了確保使用者不會因上述更新而遇到驗證問題，您可以：

- 檢查 *JSESSIONID* Cookie已在瀏覽器中設定，且具有 *SameSite=None* 和 *安全* 屬性集。
- 檢查 *JSESSIONID* 來自的cookie *https://sp.auth.adobe.com/authenticate/saml* 網路請求符合 *JSESSIONID* 來自的cookie *https://sp.auth.adobe.com/session* 網路要求。


### AccessEnabler JavaScript SDK 3.5.0版疑難排解 {#350-Troubleshooting}

Chrome 80及更高版本（82版除外）中可能會影響使用者的驗證流程。 為了確保使用者不會因上述更新而遇到驗證問題，您可以：

- 檢查 *JSESSIONID* Cookie已在瀏覽器中設定，且具有 *SameSite=None* 和 *安全* 屬性集。
- 檢查 *JSESSIONID* 來自的cookie *https://sp.auth.adobe.com/authenticate/saml* 網路請求符合 *JSESSIONID* 來自的cookie *https://sp.auth.adobe.com/session* 網路要求。
- 檢查 *pass\_sfp* Cookie已在瀏覽器中設定，且具有 *SameSite=None* 和 *安全* 屬性集。
- 檢查 *pass\_sfp* Cookie設定於 *https://sp.auth.adobe.com/session* 網路要求。


Chrome 80及更高版本（82版除外）中可能會影響使用者的授權流程。 為了確保使用者在成功通過驗證後沒有觀看受保護資源的困難，您可以透過以下方式更新資源：

- 檢查 *pass\_sfp* Cookie已在瀏覽器中設定，且具有 *SameSite=None* 和 *安全* 屬性集。
- 檢查 *pass\_sfp* Cookie設定於 *https://sp.auth.adobe.com/adobe-services/authorize* 網路要求。
- 檢查 *pass\_sfp* Cookie設定於 *https://sp.auth.adobe.com/adobe-services/shortAuthorize* 網路要求。
