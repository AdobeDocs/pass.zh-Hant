---
title: 標題 — Adobe-Subject-Token
description: REST API V2 — 標題 — Adobe-Subject-Token
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 1%

---


# 標題 — Adobe-Subject-Token {#header-adobe-subject-token}

>[!NOTE]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 概觀 {#overview}

<b>Adobe — 主體 — 權杖</b>要求標頭包含唯一的平台識別碼，即`JWS`或`JWE`，其取得來源為在Adobe Pass驗證系統外部執行的身分服務或程式庫。

此標頭是專為運用平台身分識別方法的單一登入(SSO)啟用流程所設計。

如需有關使用平台身分方法啟用單一登入(SSO)流程的詳細資訊，請參閱[使用平台身分流程的單一登入](../../flows/single-sign-on-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)檔案。

## 語法 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Adobe — 主體 — 權杖</b>： &lt;unique_platform_identifier&gt;</td>
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

* [Amazon](../../../amazon-fireos-sso-using-clientless-api-cookbook.md)

## 範例 {#examples}

請參閱下列平台中所述的範例：

* [Amazon](../../../amazon-fireos-sso-using-clientless-api-cookbook.md)
