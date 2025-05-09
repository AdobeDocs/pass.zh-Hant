---
title: 如何使用Adobe的API測試網站測試驗證和授權流程
description: 如何使用Adobe的API測試網站測試驗證和授權流程
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 65475d6da7a1b25cb2d8ebd6229a7cb360c7ab4a
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# （舊版）如何使用Adobe的API測試網站測試驗證和授權流程 {#How-to-test-auth-flows}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

為了測試AuthN和AuthZ流程，我們已準備好&#x200B;**API測試網站**，您可隨時使用。 我們的支援團隊很樂意為您提供認證。 您可以透過&#x200B;**tve-support@adobe.com**&#x200B;聯絡我們。


## 第一部分 {#part-I}

如需針對發行版環境進行測試，請直接跳至第二部分。  若要在資格預審環境中測試，請參閱[在資格預審中設定您的環境和測試](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)。

## 第二部分

完成第一部分後，請執行下列步驟：


1. 開啟網頁： [測試API測試](https://sp.auth-staging.adobe.com/apitest/api.html)。
1. 載入存取啟用碼：
   * 從下拉式功能表中選取您要存取它的位置（測試或生產），以及它是否應該處於偵錯模式
   * 輸入您要測試的軟體陳述式
   * 然後按一下[**載入存取啟用程式**]按鈕。
1. 現在將要求者識別碼值設為&quot;**requestorID**&quot;，然後按一下[setRequestor]按鈕。
1. 之後，按下「getAuthentication」按鈕，並等待顯示選擇器顯示。
1. 從選擇器中選取&quot;**MVPD**&quot;。
1. 在&quot;**MVPD**&quot;登入頁面上輸入您的認證。
1. 重新導向回之後，重做步驟1至3
1. 在「setAuthenticationStatus」上重新執行步驟3後，您應該會看到值「1」。 如果驗證無法運作，則會顯示MVPD對話方塊。
1. 若要測試授權，請在標示為「checkAuthorization」和「getAuthorization」的按鈕右側的輸入欄位中輸入您要授權的&#x200B;**資源**，然後按一下「getAuthorization」按鈕。
1. 因此，在「setToken」 — \>「resource id」文字方塊中，將顯示資源，並在「setToken」 — \>「token」文字方塊中，顯示shortAuthorizationToken，表示authZ成功。
1. 現在您可以按一下「登出」按鈕來刪除代號。
