---
title: 頁首 — AP-Device-Identifier
description: REST API V2 — 標題 — AP-Device-Identifier
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---


# 頁首 — AP-Device-Identifier {#header-ap-device-identifier}

>[!NOTE]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

<b>AP-Device-Identifier</b>要求標頭包含由使用者端應用程式建立的串流裝置識別碼。

## 語法 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Device-Identifier</b>： &lt;type&gt; &lt;identifier&gt;</td>
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

<b>&lt;型別></b>

裝置識別碼型別。

只有一種支援的型別，如下所示。

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">型別</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>指紋</td>
      <td>裝置識別碼由不透明的識別碼組成，並由使用者端應用程式建立。</td>
   </tr>
</table>


<b>&lt;識別碼></b>

裝置識別碼的`Base64-encoded`值。

## 範例 {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```
