---
title: 在選擇對話方塊中允許MVPD
description: 在選擇對話方塊中允許MVPD
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# 在選擇對話方塊中允許MVPD {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 問題 {#issue}

程式設計師在將資訊公開給一般使用者之前，可能會想要測試或檢查新MVPD整合的使用者體驗。

## 解決方案 {#solution}

在 `displayProviderDialog()` 回呼，Adobe Pass驗證會傳回與所選程式設計人員（請求者ID）整合的所有MVPD。 但程式設計師可以在MVPD的傳回陣列上套用篩選，並且只顯示同時在這兩個清單中的專案。

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

<!--
**Related Information**
* [Prevent MVPDs from appearing in the Selection Dialog](/help/authentication/prevent-mvpd-selectn-dialog.md)
* **Code Samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
