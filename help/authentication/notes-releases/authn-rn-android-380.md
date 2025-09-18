---
title: Adobe Pass Authentication Android 3.8.0發行說明
description: Adobe Pass Authentication Android 3.8.0發行說明
exl-id: 7f8a9b2c-3d4e-5f6g-7h8i-9j0k1l2m3n4o
source-git-commit: 2276066d453701dc5e034da29cb971b090688afe
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Adobe Pass Authentication Android 3.8.0發行說明 {#android-sdk-380-rn}

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

此頁面說明此版本的新功能、變更和已知問題：

## 建置編號 {#build-number-380}

Adobe Pass驗證： Android 3.8.0

發行日期： **09/18/2025**

## 版本總覽 {#release-overview-380}

* 修正SDK的儲存廣播接收器弱點。 惡意應用程式可能會提供錯誤的連結，以查詢Adobe Token的共用儲存空間。
然而，不會儲存任何重要資訊，而且任何使用者受到此漏洞影響的可能性很低。
   * 注意：由於此變更，使用者將會登出。

## 發行套件 {#release-package-380}

您可以從[這裡](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library)下載Android SDK v3.8.0。
