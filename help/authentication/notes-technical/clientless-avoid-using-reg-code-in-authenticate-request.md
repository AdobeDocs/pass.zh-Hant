---
title: 避免在/authenticate請求中使用'&'reg_code
description: 避免在/authenticate請求中使用'&'reg_code
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# 避免在/authenticate請求中使用&#39;&amp;&#39;reg_code {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>



## 問題

IE 9瀏覽器會將&#39;\&amp;reg&#39;解譯為特殊命令，並將其轉換為®。

## 說明

如果`/authenticate`要求組成如下……


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


...它將由IE瀏覽器解譯，如下所示，並將以此格式傳送給Adobe：


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


要求者\_id會解譯為univision®\_code=EKAFMFI，因為沒有&#39;&amp;&#39;，且Adobe找不到可與Token建立關聯的`regCode`引數。  完全有可能不會建立AuthN權杖，在這種情況下`/checkauthn`呼叫將找不到任何權杖。



## 解決方案

下列其中一個選項應該可以解決此問題：

1. 請避免在其他查詢字串引數之間使用`&reg_code`引數。  請改為將其移至請求URL中的第一個查詢字串引數，使請求URL如下所示：


       &lt;FQDN>authenticate？reg_code =EKAFMFI&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   如此一來，`&reg`引數就不會被錯誤解譯。

1. 將`&reg_code`標準化為使用`&amp;reg_code`。

1. 如果AuthN權杖建立失敗，Adobe可能會引入新功能，以將錯誤代碼傳回第2個畫面以回應驗證呼叫。
