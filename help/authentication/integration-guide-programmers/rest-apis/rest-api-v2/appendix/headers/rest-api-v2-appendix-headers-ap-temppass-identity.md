---
title: 頁首 — AP-TempPass-Identity
description: REST API V2 — 標題 — AP-TempPass-Identity
exl-id: a6238a58-a3f1-495d-a9d1-82475f5ffc60
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 2%
---
# 頁首 — AP-TempPass-Identity {#header-ap-temppass-identity}

>[!NOTE]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

<b>AP-TempPass-Identity</b>要求標頭包含用來達成提升TempPass的使用者身分資訊。

## 語法 {#syntax}

<table style="table-layout:auto">
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

X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
