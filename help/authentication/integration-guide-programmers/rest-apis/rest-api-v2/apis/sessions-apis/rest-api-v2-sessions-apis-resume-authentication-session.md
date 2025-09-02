---
title: 繼續驗證工作階段
description: REST API V2 — 繼續驗證工作階段
exl-id: 66c33546-2be0-473f-9623-90499d1c13eb
source-git-commit: 7ac04991289c95ebb803d1fd804e9b497f821cda
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 1%

---

# 繼續驗證工作階段 {#resume-authentication-session}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> REST API V2實作受到[節流機制](/help/authentication/integration-guide-programmers/throttling-mechanism.md)檔案的限制。

>[!MORELIKETHIS]
>
> 請確定也造訪[REST API V2常見問題集](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)。

## 請求 {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
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
        執行MVPD登入之應用程式的原始網域。
        <br/><br/>
        如果串流裝置平台在提供值方面有所限制，則應用程式必須繼續驗證工作階段並提供有效值。
      </td>
      <td><i>必填</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        MVPD驗證流程完成後，使用者代理程式導覽的最終重新導向URL。
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
      <td style="background-color: #DEEBFF;">AP-Visitor-Identifier</td>
      <td>
        <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md">AP-Visitor-Identifier</a>標標頭檔案中會說明產生訪客識別碼承載的流程。
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
        請求無效，使用者端需要修正請求，然後再試一次。 回應本文可能包含遵守<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">增強錯誤碼</a>檔案的錯誤資訊。
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>未獲授權</td>
      <td>
        存取權杖無效，使用者端需要取得新的存取權杖並重試。 如需詳細資訊，請參閱<a href="../../../rest-api-dcr/dynamic-client-registration-overview.md">動態使用者端註冊概觀</a>檔案。
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
        伺服器端發生問題。 回應本文可能包含遵守<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">增強錯誤碼</a>檔案的錯誤資訊。
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
                    <li><b>重試</b><br/>串流裝置或其他裝置需要提供遺漏的引數，然後使用程式碼重試繼續驗證工作階段。</li>
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
               <td style="background-color: #DEEBFF;">reasonType</td>
               <td>
                  說明「actionName」的原因型別。
                  <br/><br/>
                  可能的值包括：
                  <ul>
                    <li><b>無</b><br/>使用者端應用程式必須繼續驗證。</li>
                    <li><b>已驗證</b><br/>使用者端應用程式已透過基本存取流程驗證。</li>
                    <li><b>暫時</b><br/>使用者端應用程式已透過暫存存取流程驗證。</li>
                    <li><b>已降級</b><br/>使用者端應用程式已透過已降級的存取流程進行驗證。</li>
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
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>驗證程式碼無效之前的時間戳記（以毫秒為單位）。</td>
               <td>可選</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>驗證代碼無效的時間戳記（以毫秒為單位）。</td>
               <td>可選</td>
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
      <td style="background-color: #DEEBFF;"></td>
      <td>
            回應內文可能會提供其他錯誤資訊，這些資訊會遵守<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">增強型錯誤碼</a>檔案。
            <br/><br/>
            使用者端應用程式必須實作錯誤處理機制，以便能夠正確處理此API最常傳回的錯誤代碼：
            <ul>
                <li>invalid_authentication_session</li>
                <li>invalid_parameter_code</li>
                <li>等等。</li>
            </ul>
            以上清單並非詳盡無遺。 使用者端應用程式必須能夠處理<a href="../../../../features-standard/error-reporting/enhanced-error-codes.md">公開檔案</a>中定義的所有增強型錯誤碼。
      </td>
      <td><i>必填</i></td>
   </tr>
</table>

## 範例 {#samples}

### 1.繼續驗證工作階段，但不遺失引數

>[!BEGINTABS]

>[!TAB 要求]

```HTTPS
POST /api/v2/REF30/sessions/8BLW4RW HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 回應]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authenticate",
    "actionType": "interactive",
    "reasonType": "none",
    "url": "/api/v2/authenticate/REF30/8ER640M",
    "code": "8ER640M",
    "sessionId": "1b614390-6610-4d14-9421-6565f6e75958",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]

### 2.繼續遺失引數的驗證工作階段

>[!BEGINTABS]

>[!TAB 要求]

```HTTPS
POST /api/v2/REF30/sessions/8BLW4RW HTTP/1.1
 
    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com
```

>[!TAB 回應]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{           
    "actionName": "retry",
    "actionType": "direct",
    "reasonType": "none",
    "url": "/api/v2/REF30/sessions/8BLW4RW",
    "missingParameters": ["redirectUrl"]
    "code": "8BLW4RW",
    "sessionId": "1b614390-6610-4d14-9421-6565f6e75958",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30",
    "notBefore": "1733735289035",
    "notAfter": "1733737089035"
}
```

>[!ENDTABS]

### 3.當有效的設定檔已存在時，恢複驗證工作階段

>[!BEGINTABS]

>[!TAB 要求]

```HTTPS
POST /api/v2/REF30/sessions/8BLW4RW HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 回應]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authorize",
    "actionType": "direct",
    "reasonType": "authenticated",
    "url": "/api/v2/REF30/decisions/authorize/Cablevision",
    "sessionId": "1b614390-6610-4d14-9421-6565f6e75958",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30"
}
```

>[!ENDTABS]

### 4.使用基本或促銷的TempPass繼續驗證工作階段（不需要）

>[!BEGINTABS]

>[!TAB 要求]

```HTTPS
POST /api/v2/REF30/sessions/8BLW4RW HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
 
Body:

mvpd=TempPass_TEST40&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 回應]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authorize",
    "actionType": "direct",
    "reasonType": "temporary"
    "url": "/api/v2/REF30/decisions/authorize/TempPass_TEST40",
    "sessionId": "1b614390-6610-4d14-9421-6565f6e75958",
    "mvpd": "TempPass_TEST40",
    "serviceProvider": "REF30"
}
```

>[!ENDTABS]

### 5.在套用降級時繼續驗證工作階段

>[!BEGINTABS]

>[!TAB 要求]

```HTTPS
POST /api/v2/REF30/sessions/8BLW4RW HTTP/1.1

    Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJjNGZjM2U3ZS0xMmQ5LTQ5NWQtYjc0Mi02YWVhYzhhNDkwZTciLCJuYmYiOjE3MjQwODc4NjgsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpOmNsaWVudDp2MiIsImV4cCI6MTcyNDEwOTQ2OCwiaWF0IjoxNzI0MDg3ODY4fQ.DJ9GFl_yKAp2Qw-NVcBeRSnxIhqrwxhns5T5jU31N2tiHxCucKLSQ5guBygqkkJx6D0N_93f50meEEyfb7frbHhVHHwmRjHYjkfrWqHCpviwVjVZKKwl8Y3FEMb0bjKIB8p_E3txX9IbzeNGWRufZBRh2sxB5Q9B7XYINpVfh8s_sFvskrbDu5c01neCx5kEagEW5CtE0_EXTgEb5FSr_SfQG3UUu_iwlkOggOh_kOP_5GueElf9jn-bYBMnpObyN5s-FzuHDG5Rtac5rvcWqVW2reEqFTHqLI4rVC7UKQb6DSvPBPV4AgrutAvk30CYgDsOQILVyrjniincp7r9Ww
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com&redirectUrl=https%3A%2F%2Fadobe.com
```

>[!TAB 回應]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json;charset=UTF-8

{
    "actionName": "authorize",
    "actionType": "direct",
    "reasonType": "degraded",
    "url": "/api/v2/REF30/decisions/authorize/Cablevision",
    "sessionId": "1b614390-6610-4d14-9421-6565f6e75958",
    "mvpd": "Cablevision",
    "serviceProvider": "REF30"
}
```

>[!ENDTABS]
