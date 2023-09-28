---
title: Adobe Pass Authentication Android 3.7.3發行說明
description: Adobe Pass Authentication Android 3.7.3發行說明
source-git-commit: 284ae59837fd42bc6b711b3041eb6b7ef79aaed3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Adobe Pass Authentication Android 3.7.3發行說明 {#android-sdk-373-release-notes}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

此頁面說明此版本的新功能、變更和已知問題：

## 建置編號 {#build-no-android-sdk-373}

Adobe Pass驗證： Android 3.7.3

發行日期： 2023年9月19日



## 版本總覽 {#overview-android-sdk-373}

* 變更支援Android 14和以API層級34為目標的應用程式
   * 新增所需的標幟 [Android 14執行階段登入廣播接收器](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported).
* 修正無法在模擬器API 32+上開啟ChromeCustomTabs以進行MVPD登入
   * 注意： SDK &lt;3.7.3上此問題的因應措施是在模擬器上開啟Chrome應用程式，並在嘗試進行MVPD登入前完成設定


## 發行套件 {#rel-pkg-android373}

您可從下載Android SDK v3.7.3 [此處](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library).
