---
title: 在預備中設定您的環境及測試
description: 在預備中設定您的環境及測試
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: ca95bc45027410becf8987154c7c9f8bb8c2d5f8
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# 在預備中設定您的環境及測試{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

本技術說明旨在協助我們的合作夥伴設定其環境，並開始測試部署在Adobe資格預審環境上的新組建。

由於有兩種建置風格： ***生產***&#x200B;和&#x200B;***測試***，在本檔案中，我們將專注於生產設定，並提及測試的所有步驟都相同，只有URL不同。

步驟1和2是在其中一台測試機器上設定測試環境，步驟3是驗證基本流程，而步驟4和5是介紹一些測試准則。

>[!IMPORTANT]
>
> 每次想要變更測試環境（從測試環境切換至生產設定檔或其他方式）時，執行步驟1和2非常重要


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

```cmd
C:\>nslookup entitlement-prequal.auth.adobe.com 
...
Addresses:  52.26.79.43
            54.190.212.171
```

```Choose any IP from **addresses** section (e.g. `54.190.212.171)```


* 在Linux/Mac上&#x200B;****

```sh
    $ dig sp-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.13.71.11
    ............ 60 IN A      54.184.208.150
```

```Choose any IP from **A records (**e.g `52.13.71.11)```

```sh
    $ dig entitlement-prequal.auth.adobe.com
    
    ;; ANSWER SECTION:
    ...
    ............ 60 IN A      52.26.79.43
    ............ 60 IN A      54.190.212.171
```

```Choose any IP from **A records (**e.g `54.190.212.171)```

>[!NOTE]
>
>網域因不相關且可能因使用者而異，所以無法回答。

>[!IMPORTANT]
>
> 這些IP位址在未來可能會變更，而且對於不同地理區域的使用者來說，這些IP位址可能不相同。


## 步驟2.  將資格預審環境假造為生產 {#spoofing-the-prequalification-environment}

* 編輯&#x200B;*c：\\windows\\System32\\drivers\\etc\\hosts*&#x200B;檔案（在Windows中）或&#x200B;*/etc/hosts*&#x200B;檔案(在Macintosh/Linux/Android上)並新增下列專案：

* 偽造生產設定檔
   * 52.13.71.11 sp.auth.adobe.com api.auth.adobe.com
   * 54.190.212.171 entitlement.auth.adobe.com

**在Android上詐騙：**&#x200B;若要在Android上詐騙，您必須使用Android模擬器。

* 一旦設定好詐騙，您就可以將一般URL用於生產和測試設定檔： （即`http://sp.auth-staging.adobe.com`和`http://entitlement.auth-staging.adobe.com`），而且您實際上將會點選*新組建的&#x200B;*預先資格環境/生產*。


## 步驟3.  確認您指向正確的環境 {#Verify-you-are-pointing-to-the-right-environment}

**這是簡單的步驟：**

* 載入[軟體權利檔案前置環境](https://entitlement-prequal.auth.adobe.com/environment.html)和[軟體權利檔案](https://entitlement.auth.adobe.com/environment.html)。 它們應該會傳回相同的回應。


## 步驟4.  使用程式設計師的網站執行簡單的驗證/授權流程 {#peform-a-simple-auth-flow}

* 此步驟需要程式設計師的網站位址和一些有效的MVPD憑證（已驗證和授權的使用者）。

## 步驟5.  使用程式設計師的網站執行案例測試 {#perform-scenario-testing-using-programmer-website}

* 完成環境設定並確保基本驗證授權流程正常運作後，您就可以繼續測試更複雜的情境。


## 步驟6.  使用API測試網站執行測試 {#perform-testing-using-api-testing-site}

* 如果您想更深入測試Adobe Pass驗證，建議您使用[API測試網站](http://entitlement-prequal.auth.adobe.com/apitest/api.html)。

您可以在[找到更多有關API測試網站的詳細資料。如何使用Adobe的API測試網站](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)來測試驗證和授權流程。
