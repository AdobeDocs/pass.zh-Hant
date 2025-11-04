---
title: 頁首 — AP-Visitor-Identifier
description: REST API V2 — 標題 — AP-Visitor-Identifier
exl-id: 216f398b-1cfa-4453-a81d-963675b33ec2
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---

# 頁首 — AP-Visitor-Identifier {#header-ap-visitor-identifier}

>[!NOTE]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

<b>AP-Visitor-Identifier</b>要求標頭包含使用者端應用程式在所有Adobe Experience Cloud解決方案中唯一識別訪客所需的`ECID`。

如需Adobe Pass驗證中ECID使用方式的詳細資訊，請參閱[在Adobe Pass驗證中使用Experience Cloud ID ](/help/authentication/integration-guide-programmers/features-standard/analytics/exp-cloud-id-authn.md)檔案。

## 語法 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP — 訪客識別碼</b>： &lt;visitor_identifier&gt;</td>
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

## 範例 {#examples}

```JSON
AP-Visitor-Identifier: "THE_ECID_VALUE"
```
