---
title: 防止MVPD出現在「選取」對話方塊中
description: 防止MVPD出現在「選取」對話方塊中
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 0%

---

# （舊版）防止MVPD出現在「選取」對話方塊中

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

## 問題 {#issue-prevent-mvpd-sel-dialog}

您需要防止特定（「封鎖清單」）的MVPD出現在MVPD選擇器中。


## 解決方案 {#solution-prevent-mvpd-sel-dialog}

解決方案是在呼叫`displayProviderDialog()`時執行區塊清單。

例如，如果您希望CableCompany_1和CableCompany_2不要顯示在MVPD選取器中，您可以執行類似以下範例中顯示的操作。

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if (!isBlocklisted(currentMvpd.ID)) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isBlocklisted(mvpdID) {
    // Implement block-listing on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
} 
```
