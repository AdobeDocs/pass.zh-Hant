---
title: 動態使用者端註冊(DCR)字彙表
description: 動態使用者端註冊(DCR)字彙表
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# 動態使用者端註冊(DCR)字彙表 {#rest-api-dcr-glossary}

>[!IMPORTANT]
>
> 此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

本檔案提供整合Adobe Pass驗證動態使用者端註冊(DCR)時所使用的字詞定義。

>[!MORELIKETHIS]
> 
> * [REST API v2字彙表](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## 字彙表辭彙 {#glossary-terms}

### A {#a}

#### 存取權杖 {#access-token}

存取權杖是由Adobe Pass驗證產生的Token，這是旨在確儲存取受保護API的[動態使用者端註冊(DCR)](#dcr)程式的結果。

### C {#c}

#### 使用者端認證 {#client-credentials}

使用者端認證是一組在[動態使用者端註冊(DCR)](#dcr)程式期間產生的唯一值，其用途是取得[存取權杖](#access-token)。

#### 自訂配置 {#custom-scheme}

自訂配置是參考[程式設計員](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer)應用程式的唯一值，可從Adobe Pass [TVE儀表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)產生並下載，且可在iOS裝置上執行的應用程式中作為最終重新導向。

### 第{#d}天

#### DCR {#dcr}

動態使用者端註冊(DCR)是由[RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591)定義的授權機制，它以[RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)描述的OAuth 2.0授權架構為基礎。

DCR會傳遞給[程式設計師](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer)，作為Adobe Pass驗證服務，可進一步啟用對受保護API的存取。

如需詳細資訊，請參閱[動態使用者端註冊概觀](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)檔案。

### R {#r}

#### 已註冊的應用程式 {#registered-application}

註冊的應用程式是Adobe Pass驗證概念，它儲存有關[程式設計員](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer)應用程式的資訊，該應用程式需要繼續進行[動態使用者端註冊(DCR)](#dcr)程式。

### S {#s}

#### 軟體宣告 {#software-statement}

軟體陳述式是可從Adobe Pass [TVE儀表板](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard)下載的JSON Web權杖(JWT)，且要當作[動態使用者端註冊(DCR)](#dcr)程式的一部分使用。
