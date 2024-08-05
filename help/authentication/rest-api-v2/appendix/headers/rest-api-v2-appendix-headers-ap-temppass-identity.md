---
title: 頁首 — AP-TempPass-Identity
description: REST API V2 — 標題 — AP-TempPass-Identity
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 2%

---


# 頁首 — AP-TempPass-Identity {#header-ap-temppass-identity}

>[!NOTE]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

<b>AP-TempPass-Identity</b>要求標頭包含用來達成提升TempPass的使用者身分資訊。

## 語法 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-TempPass-Identity</b>： &lt;user_identity_information&gt;</td>
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

<b>&lt;user_identity_information></b>

與一般使用者相關聯的使用者身分資訊上的`Base64-encoded`值，需要授與促銷臨時存取權。

## 範例 {#examples}

```JSON
// Identity
// {"email": "example@domain.com"}

// Base64-encoded
// eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==

AP-TempPass-Identity: eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
