---
title: TVE儀表板環境
description: 瞭解TVE控制面板中不同環境的使用及運作方式。
exl-id: 591becb8-2f6c-46e0-b108-c64e6df69f89
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# 環境 {#environments}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

TVE儀表板提供各種可自訂的環境，以履行Adobe Pass驗證中的特定目的。 主要環境有兩種：

* **先決條件**：資格預審環境可作為在部署到生產環境之前準備和測試新組建的測試場。

* **版本**：版本環境裝載已完成的測試組建用於生產。

在每個環境中，有兩個不同的設定檔：

* **測試**：測試設定檔會連線至MVPD的測試伺服器，以便在上線之前測試和驗證整合。

* **生產**：生產設定檔連線到MVPD的生產設定檔，以進行實際生產活動。

## 使用案例

TVE Dashboard中的環境會在整個應用程式生命週期中提供各種使用案例。 這些環境可讓您：

### 預先測試

* 使用MVPD的暫存端點驗證Adobe Pass驗證伺服器的新未發行功能。
* 主要由Adobe Pass驗證產品團隊用於新增及驗證新的MVPD整合。

### 預先生產

* 使用MVPD的生產端點驗證Adobe Pass驗證伺服器的新未發行功能或設定。
* 使用MVPD的生產端點驗證每個管道的新應用程式版本。
* 在將每個設定變更推送至生產環境之前，先驗證這些變更。

### 發行分段

* 使用MVPD的暫存端點驗證每個管道的新應用程式版本。
* 在此環境中執行效能或容量測試。

### 發行生產

* 代表具有普遍適用於所有一般使用者之最新Adobe Pass版本組建的即時環境。
* 維護程式碼和設定的穩定性，並立即反映一般使用者應用程式中的設定變更。

## 切換環境 {#switch-environments}

依照步驟操作，以在Adobe Pass驗證TVE儀表板環境之間切換。

1. 使用您的程式設計師認證登入。

1. 從左側面板頂端的&#x200B;**環境**&#x200B;下拉式選單中選取必要的預備或生產環境。

   ![TVE儀表板環境下拉式清單](/help/authentication/assets/tve-dashboard/new-tve-dashboard/dashboard/dashboard-environment-menu.png)

   *Adobe Pass驗證TVE儀表板環境下拉式功能表*

>[!NOTE]
>
> 根據您的設定，每個環境中的設定可能會有所不同。
