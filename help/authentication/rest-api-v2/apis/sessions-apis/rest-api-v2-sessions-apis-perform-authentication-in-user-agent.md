---
title: 在使用者代理程式中執行驗證
description: REST API V2 — 在使用者代理程式中執行驗證
exl-id: d615dde0-71a8-4b6c-a12e-1e3b5e20728c
source-git-commit: ca8eaff83411daab5f136f01394e1d425e66f393
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 3%

---

# 在使用者代理程式中執行驗證 {#perform-authentication-in-user-agent}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> REST API V2實作受到[節流機制](/help/authentication/throttling-mechanism.md)檔案的限制。

## 請求 {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路徑</td>
      <td>/api/v2/authenticate/{serviceProvider}/{code}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">路徑引數</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">服務提供者</td>
      <td>在上線流程中與服務提供者相關聯的內部唯一識別碼。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">程式碼</td>
      <td>在串流裝置上建立驗證工作階段後取得的驗證代碼。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">標頭</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>使用者端應用程式的使用者代理。</td>
      <td>可選</td>
   </tr>
</table>

## 回應 {#response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">程式碼</th>
      <th style="background-color: #EFF2F7;">文字</th>
      <th style="background-color: #EFF2F7;">說明</th>
   </tr>
   <tr>
      <td>302</td>
      <td>找到</td>
      <td>
        回應內文包含位置重新導向，可繼續此流程，直到到達MVPD登入頁面為止
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>錯誤請求</td>
      <td>
        請求無效，使用者端需要修正請求，然後再試一次。
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>不允許的方法</td>
      <td>
        HTTP方法無效，使用者端需要使用請求資源所允許的HTTP方法，然後再試一次。 如需詳細資訊，請參閱<a href="#request">要求</a>區段。
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>內部伺服器錯誤</td>
      <td>
        伺服器端發生問題。
      </td>
   </tr>
</table>

### 成功 {#success}

成功的回應是一連串一或多個重新導向，直到到達MVPD登入頁面為止。

### 錯誤 {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">標頭</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">狀態</td>
      <td>400， 405， 500</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>text/html</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">內文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">-</td>
      <td>-</td>
      <td>-</td>
   </tr>
</table>

## 範例 {#samples}

### 1.在使用者代理程式中執行驗證

>[!BEGINTABS]

>[!TAB 要求]

```HTTPS
GET /api/v2/authenticate/REF30/8KHP9RW HTTP/1.1

    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
```

>[!TAB 回應]

```HTTPS
HTTP/1.1 302 Found

Location :  https://sp.auth.adobe.com/adobe-services/authenticate/saml?noflash=true&mso_id=Cablevision&requestor_id=REF30&no_iframe=false&domain_name=adobe.com&redirect_url=http%3A%2F%2Fadobe.com%2Fapitest%2Flive.html
```

>[!ENDTABS]
