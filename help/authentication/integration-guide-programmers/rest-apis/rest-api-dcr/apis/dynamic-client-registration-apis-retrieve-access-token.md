---
title: 擷取存取權杖
description: 動態使用者端註冊API — 擷取存取Token
exl-id: 23287acf-5d56-46f0-b65e-79bf7d667708
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 1%

---

# 擷取存取權杖 {#retrieve-access-token}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> Dynamic Client Registration API實作已由[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)檔案限制。

## 請求 {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路徑</td>
      <td>/o/client/token</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">主體引數</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_id</td>
      <td>
            使用者端應用程式識別碼字串。
            <br/><br/>
            如需有關如何取得使用者端識別碼字串的詳細資訊，請參閱<a href="dynamic-client-registration-apis-retrieve-client-credentials.md">擷取使用者端認證</a> API檔案。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_secret</td>
      <td>
            使用者端應用程式密碼字串。
            <br/><br/>
            如需有關如何取得使用者端密碼字串的詳細資訊，請參閱<a href="dynamic-client-registration-apis-retrieve-client-credentials.md">擷取使用者端認證</a> API檔案。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">grant_type</td>
      <td>
            使用者端應用程式可用於使用者端權杖端點的授權型別字串（例如「client_credentials」）。
            <br/><br/>
            如需有關如何取得授權型別字串的詳細資訊，請參閱<a href="dynamic-client-registration-apis-retrieve-client-credentials.md">擷取使用者端認證</a> API檔案。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">標頭</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>
         所傳送資源的接受媒體型別。
         <br/><br/>
         它必須是application/x-www-form-urlencoded。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         在<a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>檔案中說明裝置資訊裝載的產生。
         <br/><br/>
         強烈建議您在應用程式的裝置平台允許明確提供有效值時，一律使用此值。
         <br/><br/>
         提供此屬性時，Adobe Pass驗證後端會以隱含方式將明確設定的值與擷取的值合併（預設為）。
         <br/><br/>
         若未提供，Adobe Pass驗證後端將會以隱含方式使用擷取的值（依預設）。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Accept</td>
      <td>
         使用者端應用程式接受的媒體型別。
         <br/><br/>
         若指定，則必須是application/json。
      </td>
      <td>可選</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>使用者端應用程式的使用者代理。</td>
      <td>可選</td>
   </tr>
</table>

## 回應 {#response}

### 成功 {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">標頭</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">狀態</td>
      <td>201</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">內文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         具有以下屬性的JSON物件：
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">屬性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">id</td>
               <td>可用於追蹤使用者活動的不透明識別碼。</td>
               <td><i>必填</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">access_token</td>
               <td>使用者端應用程式必須用於Authorization標頭的存取權杖值。</td>
               <td><i>必填</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">created_at</td>
               <td>核發存取權杖的時間。</td>
               <td><i>必填</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">expires_in</td>
               <td>存取Token過期前的秒數。</td>
               <td><i>必填</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token_type</td>
               <td>權杖型別（例如「持有者」）。</td>
               <td><i>必填</i></td>
            </tr>
         </table>
      </td>
      <td><i>必填</i></td>
</table>

### 錯誤 {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">標頭</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">狀態</td>
      <td>400</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Content-Type</td>
      <td>application/json</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">內文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">錯誤</td>
      <td>
        可能的值包括：
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">值</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_request</td>
               <td>
                    請求由於以下原因之一而無效：
                    <ul>
                        <li>請求遺漏必要的引數。</li>
                        <li>此請求包含不支援的引數值（授予型別除外）。</li>
                        <li>要求會重複引數。</li>
                        <li>請求包含多個認證。</li>
                        <li>此請求使用多種機制來驗證使用者端。</li>
                        <li>要求的格式錯誤。</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_client</td>
               <td>使用者端認證無效，使用者端需要取得新的使用者端認證，然後再試一次。 如需更多詳細資料，請參閱<a href="dynamic-client-registration-apis-retrieve-client-credentials.md">擷取使用者端認證</a> API檔案。</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unauthorized_client</td>
               <td>使用的授權型別無效。</td>
            </tr>
         </table>
      </td>
      <td><i>必填</i></td>
   </tr>
</table>

## 範例 {#samples}

### 擷取存取權杖 {#samples-retrieve-access-token}

>[!BEGINTABS]

>[!TAB 要求]

```HTTPS
POST /o/client/token HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
    
Body:
    
client_id=s6BhdRkqt3&client_secret=t7AkePiru4&grant_type=client_credentials
```

>[!TAB 回應 — 成功]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
  "id": "a932f8f0-210a-41a4-b2a8-377751f6b76f",  
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "created_at": 1723227212,
  "expires_in": 86400,
  "token_type": "bearer"
}
```

>[!TAB 回應 — 錯誤]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
