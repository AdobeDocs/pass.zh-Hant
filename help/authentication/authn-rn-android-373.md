---
title: Adobe Pass Authentication Android 3.7.3發行說明
description: Adobe Pass Authentication Android 3.7.3發行說明
exl-id: f335357e-c209-428d-af2a-2181551447d4
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '146'
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

* 變更以支援Android 14和以API層級34為目標的應用程式
   * 新增[Android 14執行階段登入的廣播接收器](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported)所需的旗標。
* 修正無法在模擬器API 32+上開啟ChromeCustomTabs以進行MVPD登入
   * 注意： SDK &lt;3.7.3上此問題的因應措施是在模擬器上開啟Chrome應用程式，並在嘗試進行MVPD登入前完成設定


## 發行套件 {#rel-pkg-android373}

您可以從[這裡](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library)下載Android SDK v3.7.3。
