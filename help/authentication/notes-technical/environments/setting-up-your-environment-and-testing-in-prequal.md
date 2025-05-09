---
title: 在預備中設定您的環境及測試
description: 在預備中設定您的環境及測試
exl-id: f822c0a1-045a-401f-a44f-742ed25bfcdc
source-git-commit: ca95bc45027410becf8987154c7c9f8bb8c2d5f8
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---

# 在資格預審中設置環境和測試{#setting-up-your-environment-and-testing-in-prequal}

>[!NOTE]
>
>本頁面上的內容僅供參考。 使用此 API 需要 Adobe Systems 的最新許可證。 不允許未經授權的使用。

本技術說明的目的是幫助我們的合作伙伴設置他們的環境，並開始測試部署在Adobe Systems資格預審環境上的新版本編號。

由於有兩種版本編號風格：生產和&#x200B;**&#x200B;**&#x200B;**&#x200B;暫&#x200B;***存，***&#x200B;因此在此文件我們將聚焦生產設置，並提到所有步驟對於暫存都是相同的，只是 URL 不同。

步驟1和2是在其中一台測試機上設置測試環境，步驟3是對基本流程的驗證，步驟4和5提供了一些測試指南。

>[!IMPORTANT]
>
> 每次要更改測試環境（從暫存切換到生產設定檔或相反）時，執行步驟 1 和 2 非常重要


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


* **在 Linux/Mac 上**

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
>被排除在答案之外的網域，因為它們不相關，並且可能因用戶而異用戶。

>[!IMPORTANT]
>
> 這些IP位址將來可能會更改，並且對於不同地理區域的使用者，它們可能不同。


## 步驟 2.  欺騙要生產的資格預審環境 {#spoofing-the-prequalification-environment}

* *編輯 c：\\windows\\System32\\drivers\\etc\\hosts* 檔案 （在 Windows 中） 或 */etc/hosts* 檔案 （在 Macintosh/Linux/Android 上），並新增下列項目：

* 詐騙生產設定檔
   * 52.13.71.11 sp.auth.adobe.com api.auth.adobe.com
   * 54.190.212.171 entitlement.auth.adobe.com

**在Android上詐騙：**&#x200B;若要在Android上詐騙，您必須使用Android模擬器。

* 一旦設定好詐騙，您就可以將一般URL用於生產和測試設定檔： （即`http://sp.auth-staging.adobe.com`和`http://entitlement.auth-staging.adobe.com`），而且您實際上將會點選*新組建的&#x200B;*預先資格環境/生產*。


## 步驟3.  驗證您指向正確的環境 {#Verify-you-are-pointing-to-the-right-environment}

**這是一個簡單的步驟：**

* 加載 [權利環境](https://entitlement-prequal.auth.adobe.com/environment.html) 和 [權利](https://entitlement.auth.adobe.com/environment.html)。 它們應返回相同的回應。


## 步驟 4.  使用程式師的網站執行簡單的身份驗證/授權流程 {#peform-a-simple-auth-flow}

* 此步驟需要程式師的網站位址和一些有效的 MVPD 憑據（用戶它已經過身份驗證和授權）。

## 步驟 5.  使用程式設計師的網站執行案例測試 {#perform-scenario-testing-using-programmer-website}

* 完成環境設定並確保基本驗證授權流程正常運作後，您就可以繼續測試更複雜的情境。


## 步驟6.  使用API測試網站執行測試 {#perform-testing-using-api-testing-site}

* 如果您想更深入測試Adobe Pass驗證，建議您使用[API測試網站](http://entitlement-prequal.auth.adobe.com/apitest/api.html)。

您可以在 API 測試 網站上找到更多詳細信息，請参閱 [如何使用 Adobe Systems 的 API 測試 站點](/help/authentication/integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)測試Authentication和授權流。
