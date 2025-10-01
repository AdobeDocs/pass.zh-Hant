---
title: 頁首 — AP-Partner-Framework-Status
description: REST API V2 — 標題 — AP-Partner-Framework-Status
exl-id: f589d948-e23e-43d4-81c2-8db0e7a40e93
source-git-commit: 5c912bbbe97fff65d38dbade32cd4554ad8c2fac
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# 頁首 — AP-Partner-Framework-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

<b>AP-Partner-Framework-Status</b>要求標頭包含從夥伴架構取得的狀態資訊，以便達成單一登入(SSO)。

## 語法 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status</b>： &lt;partner_framework_status_information&gt;</td>
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

<b>&lt;partner_framework_status_information></b>

包含下列屬性之JSON元素的`Base64-encoded`值：

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">屬性</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         這是必要屬性。
         <br/><br/>
         合作夥伴框架傳回並由應用程式處理的使用者許可權狀態資訊。
         <br/><br/>
         這是具有下列屬性的JSON元素：
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">屬性</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>accessstatus</td>
               <td>
                  這是必要屬性。
                  <br/><br/>
                  此為具有下列可能值的列舉：
                  <br/>
                  <ul>
                     <li><b>已授予</b><br/>使用者已允許應用程式存取訂閱資訊。</li>
                     <li><b>已拒絕</b><br/>使用者已拒絕應用程式存取訂閱資訊。</li>
                     <li><b>擱置中</b><br/>使用者尚未選擇允許應用程式存取訂閱資訊。</li>
                     <li><b>notDetermined</b><br/>不允許應用程式存取訂閱資訊。</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>錯誤</td>
               <td>
                  這是選擇性屬性。
                  <br/><br/>
                  如果在查詢使用者許可權狀態資訊時觸發了合作夥伴架構錯誤，則可以使用此項來傳遞合作夥伴架構錯誤。
                  <br/><br/>
                  這是具有下列屬性的JSON元素：
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">屬性</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>程式碼</td>
                        <td>如合作夥伴架構所定義，可唯一識別錯誤的字串。</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>包含合作夥伴框架所定義之錯誤描述的字串。</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
   <tr>
      <td>frameworkProviderInfo</td>
      <td>
         這是必要屬性。
         <br/><br/>
         由合作夥伴架構傳回並由應用程式處理的提供者登入狀態資訊。
         <br/><br/>
         這是具有下列屬性的JSON元素：
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">屬性</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>id</td>
               <td>
                  這是必要屬性。
                  <br/><br/>
                  這是mappingId，可識別在合作夥伴架構層級的驗證流程中使用的MVPD。
               </td>
            </tr>
            <tr>
               <td>expirationdate</td>
               <td>
                  這是必要屬性。
                  <br/><br/>
                  這是已驗證使用者設定檔的到期日，以防使用者已在合作夥伴框架層級使用支援的MVPD成功登入。
                  <br/><br/>
                  這必須是自Unix紀元以來以毫秒為單位的時間戳記(例如「1735689600000」)，以字串表示。
               </td>
            </tr>
            <tr>
               <td>錯誤</td>
               <td>
                  這是選擇性屬性。
                  <br/><br/>
                  若在查詢提供者登入狀態資訊時觸發了合作夥伴架構錯誤，則可使用此項來傳遞合作夥伴架構錯誤。
                  <br/><br/>
                  這是具有下列屬性的JSON元素：
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">屬性</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>程式碼</td>
                        <td>如合作夥伴架構所定義，可唯一識別錯誤的字串。</td>
                     </tr>
                     <tr>
                        <td>message</td>
                        <td>包含合作夥伴框架所定義之錯誤描述的字串。</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
</table>

## 範例 {#examples}

```JSON
// Partner framework status information
// {
//    "frameworkPermissionInfo": {
//        "accessStatus": "....",
//        "error": {
//            "code" : "....",
//            "message" : "...."
//        }
//     },
//    "frameworkProviderInfo" : {
//        "id" : "....",
//        "expirationDate" : "....",
//        "error" : {
//            "code" : "...",
//            "message" : "....."
//        }
//     }
// }  
 
// Base64-encoded
// ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAg
// ImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAg
// ICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAg
// ICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIs
// CiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
 
AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAgImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAgICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAgICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
```
