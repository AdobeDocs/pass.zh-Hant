---
title: 標頭 — AD-Service-Token
description: REST API V2 — 標題 — AD-Service-Token
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 1%

---


# 標頭 — AD-Service-Token {#header-ad-service-token}

>[!NOTE]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

<b>AD-Service-Token</b>要求標頭包含不重複的使用者識別碼，此識別碼是從Adobe Pass驗證系統外部執行的身分服務所取得的`JWS`。

此標頭是專為使用服務權杖方法的已啟用單一登入(SSO)流程中所設計。

如需有關使用服務權杖方法的單一登入(SSO)啟用流程的詳細資訊，請參閱[使用服務權杖流程的單一登入](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)檔案。

## 語法 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AD服務權杖</b>： &lt;unique_user_identifier&gt;</td>
   </tr>
   <tr>
      <td>頁首型別</td>
      <td>請求標頭</td>
   </tr>
   <tr>
      <td>標準</td>
      <td>否</td>
   </tr>
</table>

## 指令 {#directives}

<b>唯一使用者識別碼</b>

JSON Web簽章(`JWS`)是包含唯一使用者識別碼資訊的已簽署JSON Web權杖(`JWT`)。

`JWT`有下列屬性：

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">屬性</th>
      <th style="background-color: #EFF2F7;">說明</th>
   </tr>
   <tr>
      <td>iss</td>
      <td>與提供外部身分服務給應用程式以實現單一登入(SSO)的實體相關聯的唯一識別碼。</td>
   </tr>
   <tr>
      <td>sub</td>
      <td>外部身分服務傳回的使用者唯一識別碼。</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>對象，應為「Adobe」。</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>針對目前JWT在時間戳記時發出。</td>
   </tr>
   <tr>
      <td>費用</td>
      <td>目前JWT的到期時間戳記。</td>
   </tr>
</table>

`JWT`必須使用`SHA256withRSA`演演算法簽署。

`JWT`必須使用私密金鑰簽署，私密金鑰是一對RSA私密金鑰的一部分 — 由外部識別服務管理的公開金鑰。

該配對的公開金鑰必須傳遞給Adobe Pass驗證，才能識別使用上述私密金鑰簽署的`JWT`權杖。

## 範例 {#examples}

```JSON
// JWT
// Header
// {
//  "alg": "RS256",
//  "kid": "qapEaY0hYNvphytwII3Sae_cAKyLS7GZOqtT_a4ajeo"
// }
// Payload data
// {
//  "sub": "Jane",
//  "name": "Jane Smith",
//  "iat": 1516239022,
//  "iss": "adobe",
//  "exp": 1720152820,
//  "aud": "adobe",
//  "jti": "3b2fb040-30a9-43d7-b647-d00ac495bab"
// }
 
// JWS
// eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA

AD-Service-Token: eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA
```
