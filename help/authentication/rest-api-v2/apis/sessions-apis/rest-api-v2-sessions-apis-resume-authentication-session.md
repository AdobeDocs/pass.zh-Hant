---
title: 繼續驗證工作階段
description: REST API V2 — 繼續驗證工作階段
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '755'
ht-degree: 1%

---


# 繼續驗證工作階段 {#resume-authentication-session}

>[!NOTE]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> REST API V2實作受到[節流機制](/help/authentication/throttling-mechanism.md)檔案的限制。

## 請求 {#request}

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">路徑</td>
      <td>/api/v2/{serviceProvider}/sessions/{code}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">方法</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">路徑引數</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">主體引數</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">標頭</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Authorization</td>
      <td>產生持有人權杖承載在<a href="../../../dynamic-client-registration-api.md">動態使用者端註冊</a>檔案中進行了說明。</td>
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 10%;">程式碼</th>
      <th style="background-color: #EFF2F7; width: 20%;">文字</th>
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
        存取權杖無效，使用者端需要取得新的存取權杖並重試。 如需詳細資訊，請參閱<a href="../../../dynamic-client-registration-api.md">動態使用者端註冊</a>檔案。
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">標頭</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">狀態</td>
      <td>200</td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">內文</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         具有以下屬性的JSON物件：
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 20%;">屬性</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7; width: 15%;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  串流裝置完成驗證流程所需執行的動作。
                  <br/><br/>
                  可能的值包括：
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">值</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">驗證</td>
                        <td>串流裝置或其他裝置需要在使用者代理程式中開啟提供的URL。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">重試</td>
                        <td>串流裝置或其他裝置需要提供遺失的引數，然後使用程式碼再次嘗試恢複驗證工作階段。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">授權</td>
                        <td>串流裝置可以直接繼續進行決定流程。</td>
                     </tr>
                  </table>
               <td><i>必填</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  串流裝置必須執行的互動型別，才能使用「actionName」屬性所指定的動作繼續流程。
                  <br/><br/>
                  可能的值包括：
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 30%;">值</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">直接</td>
                        <td>使用可用於使用者端實作的HTTP使用者端，以直接呼叫提供的URL來繼續此流程。</td>
                     </tr>
                     <tr>
                        <td style="background-color: #DEEBFF;">互動式</td>
                        <td>流程會使用使用者代理程式繼續導覽至提供的URL。</td>
                     </tr>
                  </table>
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

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">標頭</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
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
      <th style="background-color: #EFF2F7; width: 15%;">內文</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7; width: 10%;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">錯誤</td>
      <td>錯誤提供附加資訊以遵守<a href="../../../enhanced-error-codes.md">增強型錯誤碼</a>檔案。</td>
      <td><i>必填</i></td>
   </tr>
</table>

## 範例 {#samples}

### 1.繼續驗證工作階段，不提供所有必要引數的值

>[!BEGINTABS]

>[!TAB 要求]

```JSON
POST /api/v2/REF30/sessions/8BLW4RW
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com
```

>[!TAB 回應]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "retry",
   "actionType" : "interactive",
   "url" : "/v2/REF30/sessions/8BLW4RW",
   "missingParameters" : ["redirectUrl"]
   "code" : "8BLW4RW",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]
