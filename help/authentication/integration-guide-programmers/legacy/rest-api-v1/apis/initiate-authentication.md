---
title: 啟動驗證
description: 啟動驗證
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---

# （舊版）起始驗證 {#initiate-authentication}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

>[!NOTE]
>
> REST API實作已由[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)限制

## REST API端點 {#clientless-endpoints}

&lt;REGGIE_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>：

* 生產 — [api.auth.adobe.com](http://api.auth.adobe.com/)
* 正在暫存 — [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## 說明 {#description}

透過通知MVPD選擇事件來啟動驗證流程。 在Adobe Pass驗證資料庫上建立記錄，並在從MVPD收到成功回應時進行調節。



| 端點 | 呼叫</br>者 | 輸入   </br>引數 | HTTP </br>方法 | 回應 | HTTP </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/驗證 | 驗證模組 | 1. requestor_id （必要）</br>2。  mso_id （必要）</br>3。  reg_code （必要）</br>4。  domain_name （必要）</br>5。  noflash=true - </br>    （必要，剩餘引數）</br>6。  no_iframe=true （必要，剩餘引數）</br>7。  額外的引數（選擇性）</br>8。  redirect_url （必要） | GET | 系統會將登入網頁應用程式重新導向至MVPD登入頁面。 | 302 （完整重新導向實作） |

{style="table-layout:auto"}


| 輸入引數 | 說明 |
| --- | --- |
| requestor_id | 此作業有效的程式設計師要求者。 |
| mso_id | 此操作有效的MVPD ID。 |
| reg_code | Reggie服務產生的註冊代碼。 |
| domain_name | 原始網域。 |
| redirect_url | 驗證完成後的登入Webapp重新導向url。 |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**重要：必要引數 —**&#x200B;無論使用者端實作為何，上述所有引數都是必要引數。
>
>
>範例：
>
>```
>domain_name=loginwebapp.com
>mso_id=sampleMvpdId
>reg_code=RO0885W
>requestor_id=sampleRequestorId
>noflash=true
>redirect_url=http://loginwebapp.com
>```

>[!IMPORTANT]
> 
>**重要：選用引數**
>
>呼叫也可能包含可啟用其他功能的選用引數，例如：
>
> * generic\_data — 允許使用[促銷臨時傳遞](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **附註** {#notes}

* `domain_name`引數的值必須設定為向Adobe Pass驗證註冊的其中一個網域名稱。

* [避免在/authenticate請求中使用&#39;&amp;&#39;reg\_code （技術說明）](/help/authentication/integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)

* `redirect_url`引數必須是順序中的最後一個引數

* `redirect_url`引數的值必須以URL編碼
