---
title: Adobe Pass Authentication 2.63發行說明
description: Adobe Pass Authentication 2.63發行說明
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Adobe Pass Authentication 2.63發行說明 {#authn-263-rn}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

此頁面說明此版本的新功能、變更和已知問題：

## 伺服器端和Web使用者端 {#server-side-web-clients-263}

* [建置編號](#build-number-263)
* [版本總覽](#release-overview-263)

### 建置編號 {#build-number-263}

Adobe Pass驗證： adobe-pass-**2.63**

發行日期： **09/20/2022 - 09/22/2022**

### 版本總覽 {#release-overview-263}

#### 改善平台識別機制

* 自此發行版本開始，我們改善用來識別裝置的機制，不再依賴使用者端實施。 這可在平台層級套用商業規則時，提供更準確的詳細程度，並可更清楚瞭解ESM報表中的流量值。

* 隨後將發佈新的ESM版本，其中包含公開平台相關欄位的新報告和改良報告。

* 如需計畫變更的詳細資訊，請洽詢您的TAM。

#### MVPD自我降級

此功能讓MVPD可以在高流量情況下，當個別端點的負載變得過高時，暫時略過自己的驗證和授權端點。

#### 在授權呼叫的標頭中新增代理識別碼

此功能在授權呼叫的標頭中新增Synacor代理MVPD的ID。 如此一來，Synacor就能為每個被代理的個人(例如 根據代理的MVPD路由至不同的網域)。

#### TVE控制面板

在此版本中，我們修正了MVPD層級設定的authN或authZ TTL未在設定報表中正確計算的問題。

#### JavaScript SDK 4.6.0

* 已移除`eval`函式的使用，因此使SDK符合內容安全性原則。
* 修正合作夥伴應用程式明確清除瀏覽器的本機儲存體時，驗證流程無法成功完成的問題。
