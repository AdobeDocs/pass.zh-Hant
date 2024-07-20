---
title: Adobe Pass Authentication 2.69發行說明
description: Adobe Pass Authentication 2.69發行說明
exl-id: d031c4c5-dbd5-4a77-b298-a53b992cc4c5
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---

# Adobe Pass Authentication 2.69發行說明 {#pt-authn-269-rn}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-269}

* [建置編號](#build-number-269)
* [新功能](#new-features-269)

### 建置編號 {#build-number-269}

Adobe Pass驗證： adobe-pass-**2.69**
發行日期： **02/27/2024 - 02/29/2024**

### 新功能 {#new-features-269}

#### 雜項 {#misc}

* 已修補安全性漏洞。
* 增強透過動態使用者端註冊(DCR)重設Temp Pass安全性層。
   * 您可以在這裡找到更多詳細資料： [重設臨時密碼](reset-temp-pass.md)
* 平台識別報告的增強功能。

#### REST API {#rest-apis}

* 正在開發新的REST API。
   * 即將推出的專屬版本將引入新的端點和流程，這些將在單獨的通知中宣佈。
   * 正在更新使用這些新API的檔案。

#### TVE控制面板 {#tve-dashboard}

* 新TVE Dashboard的持續開發。
   * 即將推出的專屬版本將推出新的TVE Dashboard，將以個別通知的形式宣佈。
   * 正在更新使用這個新TVE控制面板的檔案。

#### JavaScript SDK 4.7.0 {#js-sdk}

* 移除Access Enabler JavaScript SDK 2.0.1版（由於安全漏洞）。
   * 如需詳細資訊，請參閱連結： [Adobe Pass Authentication JavaScript 4.7.0發行說明](authn-rn-javascript-470.md)
