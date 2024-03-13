---
title: 刪除註冊記錄
description: 刪除註冊資源
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 1%

---

# 刪除註冊記錄 {#delete-registration-record}

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


## 說明 {#delete-record}

刪除登入程式碼記錄並釋出登入程式碼以供重複使用。

| 端點 | 已呼叫  </br>作者： | 輸入   </br>引數 | HTTP  </br>方法 | 回應 | HTTP  </br>回應 |
| --- | --- | --- | --- | --- | --- |
| &lt;reggie_fqdn>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>例如：</br></br>&lt;reggie_fqdn>/reggie/v1/regcode/ER45RTY | 串流應用程式</br></br>或</br></br>程式設計師服務 | 1.要求者ID  </br>    （路徑元件）</br>2.  註冊代碼  </br>    （路徑元件） | DELETE | 無 | 204 |

{style="table-layout:auto"}

</br>

| 輸入引數 | 說明 |
| --- | --- |
| 要求者 | 此作業有效的程式設計師要求者ID。 |
| 註冊代碼 | 串流裝置上顯示的註冊代碼值（要輸入驗證流程中）。 |

{style="table-layout:auto"}

</br>

### [返回REST API參考](/help/authentication/rest-api-reference.md)
