---
title: 頁首 — X-Roku-Reserved-Roku-Connect-Token
description: REST API V2 — 標題 — X-Roku-Reserved-Roku-Connect-Token
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 0%

---

# 頁首 — X-Roku-Reserved-Roku-Connect-Token {#header-x-roku-reserved-roku-connect-token}

>[!NOTE]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

<b>X-Roku-Reserved-Roku-Connect-Token</b>要求標頭包含唯一的平台識別碼，即`JWS`或`JWE`，此識別碼是從Adobe Pass驗證系統外部執行的識別服務或程式庫所取得。

此標頭是專為運用平台身分識別方法的單一登入(SSO)啟用流程所設計。

如需有關使用平台身分方法啟用單一登入(SSO)流程的詳細資訊，請參閱[使用平台身分流程的單一登入](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)檔案。

## 語法 {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Roku-Reserved-Roku-Connect-Token</b>： &lt;unique_platform_id&gt;</td>
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

<b>唯一平台識別碼</b>

JSON Web簽章(`JWS`)或JSON Web加密(`JWE`)是包含唯一平台識別碼資訊的已簽署或加密JSON Web權杖(`JWT`)。

這適用於下列平台：

* [Roku SSO逐步指南(REST API V2)](../../../../features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)
