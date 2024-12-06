---
title: Android 10應用程式上的Access Enabler Android SDK單一登入(SSO)
description: Android 10應用程式上的Access Enabler Android SDK單一登入(SSO)
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 0%

---

# Android 10應用程式上的Access Enabler Android SDK單一登入(SSO) {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀

透過Access Enabler Android SDK，使用Adobe Pass作業系統的裝置上可使用Android Authentication支援的應用程式之間的單一登入(SSO)。 為了在Android裝置上提供單一登入(SSO)，Access Enabler Android SDK 3.2.1版（最新）和舊版都使用儲存在Android儲存實作中的共用資料庫檔案，所有Adobe Pass驗證支援的應用程式都可以存取。

不過，Google在最新的Android 10發行版本中做了一些變更，「以讓使用者更能掌控其檔案，並限制檔案雜亂，依預設，針對Android 10 （API層級29）及更高版本的應用程式可取得外部儲存裝置的範圍存取權，或設定範圍的儲存裝置。 這類應用程式只能看見其應用程式特定目錄`\[...\]`。 有關這些Android 10儲存體變更的更多詳細資訊，顯示在[Android](https://developer.android.com/training/data-storage/files/external-scoped)的資料和檔案儲存檔案。

由於這些變更，Access Enabler Android版本&#x200B;**3.2.1 SDK （最新）**&#x200B;和舊版所提供的單一登入(SSO)可能會在Android 10裝置上受到影響，如下節所述。

## 行為

視應用程式的&#x200B;**[!UICONTROL target SDK level]**&#x200B;或&#x200B;**android：requestLegacyExternalStorage**&#x200B;資訊清單屬性的使用情況而定，Access Enabler Android 3.2.1 SDK版（最新）和舊版所提供的單一登入(SSO)目前會依下列方式運作：

- 您的應用程式目標為&#x200B;**Android 9 （API層級28）**&#x200B;或更低的&#x200B;**-\>**&#x200B;單一登入(SSO) **將可運作**
- 您的應用程式以&#x200B;**Android 10** **（API層級29）**&#x200B;為目標，並將&#x200B;**將**&#x200B;應用程式資訊清單檔案&#x200B;**-\>**&#x200B;單一登入(SSO) **中的** requestLegacyExternalStorage值設定為true **將可運作**
- 您的應用程式以&#x200B;**Android 10** **（API層級29）**&#x200B;為目標，且&#x200B;**未將**&#x200B;應用程式資訊清單檔案&#x200B;**-\>**&#x200B;單一登入(SSO) **中的** requestLegacyExternalStorage值設定為true ****&#x200B;將無法運作

>[!TIP]
>
> 在Adobe Pass Authentication Access Enabler Android SDK與範圍設定儲存體完全相容之前，您可以根據應用程式的Target SDK層級或requestLegacyExternalStorage資訊清單屬性，暫時選擇退出，如公開[Android檔案](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage)中所述。
