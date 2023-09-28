---
title: 如何使用Adobe的API測試網站測試驗證和授權流程
description: 如何使用Adobe的API測試網站測試驗證和授權流程
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# 如何使用Adobe的API測試網站測試驗證和授權流程 {#How-to-test-auth-flows}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

為了測試AuthN和AuthZ流程，我們已準備 **API測試網站** 隨時待命。 我們的支援團隊很樂意為您提供認證。 您可以聯絡我們： **support@tve.zendesk.com**.


## 第一部分 {#part-I}

如需針對發行版環境進行測試，請直接跳至第二部分。  若要在資格預審環境中測試，請參閱 [在預備中設定您的環境及測試](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

## 第二部分

完成第一部分後，請執行下列步驟：


1. 開啟網頁： [測試API測試](https://sp.auth-staging.adobe.com/apitest/api.html).
1. 從下列URL載入存取啟用程式：
   * [用於中繼的Access Enabler JavaScript](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * 或
   * [生產環境的Access Enabler JavaScript](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * 然後按一下「**載入存取啟用程式**」按鈕。
1. 現在將請求者ID值設為&quot;**要求者ID**」並按一下「setRequestor」按鈕。
1. 之後，按下「getAuthentication」按鈕，並等待顯示選擇器顯示。
1. 選擇&quot;**MVPD**」從選擇器。
1. 在「」輸入您的認證&#x200B;**MVPD**「登入頁面。
1. 重新導向回之後，重做步驟1至3
1. 在「setAuthenticationStatus」上重新執行步驟3後，您應該會看到值「1」。 如果驗證無法運作，則會顯示MVPD對話方塊。
1. 若要測試授權，請在資源輸入欄位中輸入&quot;**要求者ID**」並按一下「getAuthorization」按鈕。
1. 因此，在「setToken」 — \>「resource id」文字方塊中，將顯示資源，並在「setToken」 — \>「token」文字方塊中，顯示shortAuthorizationToken，表示authZ成功。
1. 現在您可以按一下「登出」按鈕來刪除代號。
