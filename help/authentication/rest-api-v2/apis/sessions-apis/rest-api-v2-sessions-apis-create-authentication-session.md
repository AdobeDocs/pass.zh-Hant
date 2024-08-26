---
title: 建立驗證工作階段
description: REST API V2 — 建立驗證工作階段
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '968'
ht-degree: 1%

---


# 建立驗證工作階段 {#create-authentication-session}

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
      <td>/api/v2/{serviceProvider}/sessions</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>POST</td>
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
      <th style="background-color: #EFF2F7;">主體引數</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>
        上線流程中與身分提供者相關聯的內部唯一識別碼。
        <br/><br/>
        如果串流裝置平台在提供值方面有所限制，則應用程式必須繼續驗證工作階段並提供有效值。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        執行MVPD登入的應用程式之原始網域。
        <br/><br/>
        如果串流裝置平台在提供值方面有所限制，則應用程式必須繼續驗證工作階段並提供有效值。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        MVPD的驗證流程完成時，使用者代理程式導覽的最終重新導向URL。
        <br/><br/>
        值必須以URL編碼。
        <br/><br/>
        如果串流裝置平台在提供值方面有所限制，則應用程式必須繼續驗證工作階段並提供有效值。
        </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">標頭</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Authorization</td>
      <td>在<a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">授權</a>標標頭檔案中說明了持有人權杖承載的產生。</td>
      <td><i>必填</i></td>
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
      <td style="background-color: #DEEBFF;">AP-Device-Identifier</td>
      <td>在<a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>標標頭檔案中說明裝置識別碼裝載的產生。</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         在<a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>標題檔案中會說明裝置資訊承載的產生。
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
      <td style="background-color: #DEEBFF;">X-Forwarded-For</td>
      <td>
         串流裝置的IP位址。
         <br/><br/>
         強烈建議一律將它用於伺服器對伺服器的實作，尤其是當呼叫是由程式設計人員服務（而非串流裝置）進行時。
         <br/><br/>
         對於使用者端對伺服器實作，會以隱含方式傳送串流裝置的IP位址。
      </td>
      <td>可選</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Adobe-Subject-Token</td>
      <td>
        在<a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Platform-Subject-Token</a>標標頭檔案中會說明Platform Identity方法單一登入裝載的產生Adobe。
        <br/><br/>
        如需使用平台身分識別啟用單一登入流程的詳細資訊，請參閱<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">使用平台身分識別流程單一登入</a>檔案。
      </td>
      <td>可選</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        服務權杖方法的單一登入裝載產生過程在<a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a>標標頭檔案中進行了說明。
        <br/><br/>
        如需使用服務權杖啟用單一登入流程的詳細資訊，請參閱<a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md">使用服務權杖流程進行單一登入</a>檔案。
      <td>可選</td>
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

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">程式碼</th>
      <th style="background-color: #EFF2F7;">文字</th>
      <th style="background-color: #EFF2F7;">說明</th>
   </tr>
   <tr>
      <td>200</td>
      <td>確定</td>
      <td>
        回應內文包含執行驗證所需後續動作的資訊。
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>錯誤請求</td>
      <td>
        請求無效，使用者端需要修正請求，然後再試一次。 回應本文可能包含遵守<a href="../../../enhanced-error-codes.md">增強錯誤碼</a>檔案的錯誤資訊。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未獲授權</td>
      <td>
        存取權杖無效，使用者端需要取得新的存取權杖並重試。 如需詳細資訊，請參閱<a href="../../../dcr-api/dynamic-client-registration-overview.md">動態使用者端註冊概觀</a>檔案。
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
        伺服器端發生問題。 回應本文可能包含遵守<a href="../../../enhanced-error-codes.md">增強錯誤碼</a>檔案的錯誤資訊。
      </td>
   </tr>
</table>

### 成功 {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">標頭</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">狀態</td>
      <td>200</td>
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
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  串流裝置完成驗證流程所需執行的動作。
                  <br/><br/>
                  可能的值包括：
                  <ul>
                    <li><b>驗證</b><br/>串流裝置或其他裝置需要在使用者代理程式中開啟提供的URL。</li>
                    <li><b>繼續</b><br/>串流裝置或其他裝置需要提供遺漏的引數，並使用程式碼繼續驗證工作階段。</li>
                    <li><b>授權</b><br/>串流裝置可以直接進行決定流程。</li>
                  </ul>
               <td><i>必填</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  串流裝置必須執行的互動型別，才能使用「actionName」屬性所指定的動作繼續流程。
                  <br/><br/>
                  可能的值包括：
                  <ul>
                    <li><b>互動式</b><br/>流程繼續使用使用者代理程式導覽至提供的URL。</li>
                    <li><b>直接</b><br/>流程會繼續使用使用者端實作可用的HTTP使用者端直接呼叫提供的URL。</li>
                  </ul>
               <td><i>必填</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>需要提供遺失的引數，才能完成基本驗證流程。</td>
               <td>可選</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>使用者端應用程式需要導覽的URL。</td>
               <td>可選</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">程式碼</td>
               <td>可用於次要應用程式的驗證代碼，以繼續驗證工作階段。</td>
               <td><i>必填</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>可用於追蹤使用者活動的不透明識別碼。</td>
               <td><i>必填</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>上線流程中與身分提供者相關聯的內部唯一識別碼。</td>
               <td>可選</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">服務提供者</td>
               <td>在上線流程中與服務提供者相關聯的內部唯一識別碼。</td>
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
      <td>400， 401， 405， 500</td>
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
      <td>錯誤提供附加資訊以遵守<a href="../../../enhanced-error-codes.md">增強型錯誤碼</a>檔案。</td>
      <td><i>必填</i></td>
   </tr>
</table>

## 範例 {#samples}

### 1.建立驗證工作階段，同時提供所有必要引數的值

>[!BEGINTABS]

>[!TAB 要求]

```JSON
POST /api/v2/REF30/sessions
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 回應]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "authenticate",
   "actionType" : "interactive",
   "url" : "/v2/authenticate/REF30/8ER640M",
   "code" : "8ER640M",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]

### 2.建立驗證工作階段，同時提供「無」或某些必要引數的值

>[!BEGINTABS]

>[!TAB 要求]

```JSON
POST /api/v2/REF30/sessions
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:
```

>[!TAB 回應]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "resume",
   "actionType" : "direct",
   "url" : "/v2/REF30/sessions/8ER640M",
   "code" : "8ER640M",
   "sessionId" : "3453453354jkopey",
   "missingParameters" : ["mvpd","domain","redirectUrl"],
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]

### 3.在提供mvpd引數的值時建立驗證工作階段，而且其有效的已驗證設定檔已經存在

>[!BEGINTABS]

>[!TAB 要求]

```JSON
POST /api/v2/REF30/sessions
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 回應]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "profile",
   "actionType" : "direct",
   "url" : "/v2/REF30/profiles/8ER640M",
   "code" : "8ER640M",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]

### 4.在提供mvpd引數的值時建立驗證工作階段，且已發生降級

>[!BEGINTABS]

>[!TAB 要求]

```JSON
POST /api/v2/REF30/sessions
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 回應]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "authorize",
   "actionType" : "direct",
   "url" : "/v2/REF30/decisions/authorize",
   "code" : "8ER640M",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]
