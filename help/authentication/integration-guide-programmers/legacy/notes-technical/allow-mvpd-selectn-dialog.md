---
title: 在選擇對話方塊中允許MVPD
description: 在選擇對話方塊中允許MVPD
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# （舊版）在選取對話方塊中允許MVPD {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

## 問題 {#issue}

程式設計師在將資訊公開給一般使用者之前，可能會想要測試或檢查新MVPD整合的使用者體驗。

## 解決方案 {#solution}

在`displayProviderDialog()`回呼中，Adobe Pass驗證會傳回與所選程式設計員（要求者ID）整合的所有MVPD。 但程式設計師可以在MVPD的傳回陣列上套用篩選，並且只顯示同時在這兩個清單中的專案。

## 範例 {#example}

此範例示範如何在MVPD選取器對話方塊中僅顯示CableCompany_1和CableCompany_2，而不顯示CableCompany_NewIntegration。

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if ( isAllowListed(currentMvpd.ID) ) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isAllowListed(mvpdID) {
    // Implement allowlisting on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
}
```
