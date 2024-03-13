---
title: 啟動驗證
description: 啟動驗證
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---

# 啟動驗證 {#initiate-authentication}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!NOTE]
>
> REST API實作受限於 [節流機制](/help/authentication/throttling-mechanism.md)

## REST API端點 {#clientless-endpoints}

&lt;reggie_fqdn>：

* 生產 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 分段 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>：

* 生產 —  [api.auth.adobe.com](http://api.auth.adobe.com/)
* 分段 —  [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## 說明 {#description}

透過通知MVPD選取事件來啟動驗證程式。 在Adobe Pass驗證資料庫上建立記錄，在從MVPD收到成功回應時進行調節。



| 端點 | 已呼叫  </br>作者： | 輸入   </br>引數 | HTTP  </br>方法 | 回應 | HTTP  </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate | 驗證模組 | 1. requestor_id （必要）</br>2.  mso_id （必要）</br>3.  reg_code （必要）</br>4.  domain_name （必要）</br>5.  noflash=true -  </br>    （必要，剩餘引數）</br>6.  no_iframe=true （必要， Residence引數）</br>7.  額外引數（選擇性）</br>8.  redirect_url （必要） | GET | 系統會將「登入Web應用程式」重新導向至MVPD登入頁面。 | 302 （完整重新導向實作） |

{style="table-layout:auto"}


| 輸入引數 | 說明 |
| --- | --- |
| requestor_id | 此作業有效的程式設計師要求者。 |
| mso_id | 此作業適用的MVPD ID。 |
| reg_code | Reggie服務產生的註冊代碼。 |
| domain_name | 原始網域。 |
| redirect_url | 驗證完成後的登入Webapp重新導向url。 |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**重要：必要引數 —** 無論使用者端實作為何，上述所有引數都是強制性的。
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
> * generic\_data — 允許使用 [促銷暫時傳遞](/help/authentication/promotional-temp-pass.md)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **附註** {#notes}

* 的值 `domain_name` 引數必須設定為在Adobe Pass驗證中註冊的其中一個網域名稱。 如需詳細資訊，請參閱 [註冊與初始化](/help/authentication/programmer-overview.md).

* [避免在/authenticate請求中使用&#39;&amp;&#39;reg\_code （技術說明）](/help/authentication/clientless-avoid-using-reg-code-in-authenticate-request.md)

* 此 `redirect_url` 引數必須是順序中的最後一個引數

* 的值 `redirect_url` 引數必須以URL編碼
