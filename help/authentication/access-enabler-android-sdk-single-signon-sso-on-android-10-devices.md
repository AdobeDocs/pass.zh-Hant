---
title: Android 10應用程式上的Access Enabler Android SDK單一登入(SSO)
description: Android 10應用程式上的Access Enabler Android SDK單一登入(SSO)
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# Android 10應用程式上的Access Enabler Android SDK單一登入(SSO) {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀

使用Android作業系統的裝置可透過Access Enabler Android SDK在Adobe Pass驗證支援的應用程式之間使用單一登入(SSO)。 為了在Android裝置上提供單一登入(SSO)，Access Enabler Android SDK 3.2.1版（最新）和舊版使用儲存在Android儲存實作中的共用資料庫檔案，所有Adobe Pass驗證支援的應用程式都可以存取。

然而，Google在最新的Android 10版本中推出了一些變更，「讓使用者更能掌控其檔案，並限制檔案雜亂，依預設，針對Android 10 （API層級29）及更高版本的應用程式可取得外部儲存裝置或設定範圍的儲存空間。 這類應用程式只能看見其應用程式特定目錄 `\[...\]`「。 有關這些Android 10儲存空間變更的更多詳細資訊，請參見 [適用於Android的資料和檔案儲存檔案](https://developer.android.com/training/data-storage/files/external-scoped).

這些變更導致Access Enabler Android版本提供的單一登入(SSO) **3.2.1 SDK （最新）** 和舊版可能會在Android 10裝置上受到影響，如下節所述。

另請參閱 [Roku SSO概觀](/help/authentication/roku-sso-overview.md).

## 行為

根據您應用程式的 **[!UICONTROL target SDK level]** 或使用方式 **android：requestLegacyExternalStorage** 資訊清單屬性Access Enabler Android 3.2.1 SDK （最新）版和舊版所提供的單一登入(SSO)目前行為如下：

- 您的應用程式目標 **Android 9 （API層級28）** 或以下 **-\>** 單一登入(SSO) **會運作**
- 您的應用程式目標 **Android 10** **（API層級29）** 並且會 **設定** 的值 **requestLegacyExternalStorage設為true** 在應用程式的資訊清單檔案中 **-\>** 單一登入(SSO) **會運作**
- 您的應用程式目標 **Android 10** **（API層級29）** 並且會 **未設定** 的值 **requestLegacyExternalStorage設為true** 在應用程式的資訊清單檔案中 **-\>** 單一登入(SSO) **將無法運作**


>[!TIP]
>
> 在Adobe Pass Authentication Access Enabler Android SDK與範圍設定儲存體完全相容之前，您可以根據應用程式的Target SDK層級或requestLegacyExternalStorage資訊清單屬性（如公開內容所述），暫時選擇退出 [Android檔案](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage).
