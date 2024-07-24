---
title: 在預備中設定您的環境及測試
description: 在預備中設定您的環境及測試
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: 3a6a5633c728398a3847ee3e341e82aba915f0d9
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# 在「資格前」設定您的環境和測試{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>本頁面上的內容僅供資訊之用。 使用此API需要Adobe目前的授權。 不允許未經授權的使用。

此技術說明的目的在於協助我們的合作夥伴設定環境，並開始測試部署於Adobe資格前環境的新組建。

由於有兩種組建： ***製作*** 和 ***中轉***，在本檔中，我們將專注於製作設定，並提到所有步驟都與中繼步驟相同，只有 URL 不同。

步驟 1 和 2 是在其中一部測試計算機上設定測試環境，步驟 3 即驗證基本流程，而步驟 4 和 5 會呈現一些測試指南。

>[!IMPORTANT]
>
> 每次您要變更測試環境時，請務必執行步驟 1 和 2 （從階段切換到生產描述檔或其他方式）


## 步驟1. 將傳遞網域解析為IP {#resolving-pass-domain-to-an-ip}

* 若要尋找可用於欺騙的負載平衡器IP，請執行以下命令：

* 在Windows **上的**

  ```cmd
  C:\>nslookup sp-prequal.auth.adobe.com
  ...
  Addresses:  52.13.71.11
              54.184.208.150
  ```

```Choose any IP from **addresses** section (e.g. `52.13.71.11)```

* **在 Linux/Mac 上**

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

>[!NOTE]
>
>網域排除在答案之外，因為它們不相關，而且可能因使用者而有所不同。

>[!IMPORTANT]
>
> 這些IP位址未來可能會有所變動，而不同地理區域的使用者可能並不相同。


## 步驟 2.  欺騙要製作的資格前環境 {#spoofing-the-prequalification-environment}

* *編輯 c：\\windows\System32\\drivers\etc\\* hosts 檔案 （Windows） 或 */etc/hosts* 檔案 （在 Macintosh/Linux/Android 上），然後新增下列專案：

* Spoof 生產描述檔
   * 52.13.71.11 entitlement.auth.adobe.com sp.auth.adobe.com api.auth.adobe.com

**在Android上詐騙：**&#x200B;若要在Android上詐騙，您必須使用Android模擬器。

* 一旦設定好詐騙，您就可以將一般URL用於生產和測試設定檔： （即`http://sp.auth-staging.adobe.com`和`http://entitlement.auth-staging.adobe.com`），而且您實際上將會點選*新組建的&#x200B;*預先資格環境/生產*。


## 步驟3.  確認您指向正確的環境 {#Verify-you-are-pointing-to-the-right-environment}

**這是一個簡單的步驟：**

* 載入 [權益預先質量的環境](https://entitlement-prequal.auth.adobe.com/environment.html) 和 [權益](https://entitlement.auth.adobe.com/environment.html)。 應傳回相同的回應。


## 步驟 4.  使用程式人員的網站執行簡單的驗證/授權流程 {#peform-a-simple-auth-flow}

* 此步驟需要程式人員的網站位址和一些有效的 MVPD 認證 （經過驗證及授權的使用者）。

## 步驟 5.  使用程式代碼的網站執行情境測試 {#perform-scenario-testing-using-programmer-website}

* 完成環境設定並確保基本驗證授權流程正常運作后，您可以繼續測試更複雜的情況。


## 步驟6.  使用API測試網站執行測試 {#perform-testing-using-api-testing-site}

* 如果您想更深入測試Adobe Pass驗證，建議您使用[API測試網站](http://entitlement-prequal.auth.adobe.com/apitest/api.html)。

您可以在[找到更多有關API測試網站的詳細資料。如何使用Adobe的API測試網站](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md)來測試驗證和授權流程。
