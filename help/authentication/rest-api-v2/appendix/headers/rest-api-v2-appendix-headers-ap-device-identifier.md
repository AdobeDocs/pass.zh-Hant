---
title: 頁首 — AP-Device-Identifier
description: REST API V2 — 標題 — AP-Device-Identifier
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: 8f4fb5d6cc8b45b300010438c56d4af2e8fc0a76
workflow-type: tm+mt
source-wordcount: '413'
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
      <td>
            裝置識別碼是由使用者端應用程式建立並管理的穩定且唯一的識別碼組成。
            <br/>
            使用者端應用程式必須防止使用者動作（例如解除安裝應用程式、重新安裝或升級）所導致的值變更。
      </td>
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

## 逐步指南 {#cookbooks}

>[!IMPORTANT]
>
> 說明檔案資源僅供參考之用。
>
> 檔案資源並非詳盡無遺，且可能需要額外的修改才能在您的專案中運作。
> 
> 無論您實際實作為何，`AP-Device-Identifier`標頭都必須包含格式化的值，如[指示](#directives)區段中所述。

### 瀏覽器 {#browsers}

若要為瀏覽器中執行的裝置建置`AP-Device-Identifier`標頭，您的使用者端應用程式需要根據可用的資料（例如瀏覽器、裝置或使用者特定資料），運算穩定且唯一的識別碼。

_(*)建議整合提供瀏覽器或裝置指紋識別機制的程式庫或服務。_

### 行動裝置 {#mobile-devices}

#### iOS和iPadOS {#ios-ipados}

若要為執行[iOS或iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes)的裝置建置`AP-Device-Identifier`標題，您可以參考下列檔案：

* [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)的Apple開發人員檔案。

_(*)建議在作業系統提供的值上套用SHA-256雜湊函式。_

#### Android {#android}

若要為執行[Android](https://developer.android.com/about/versions)的裝置建置`AP-Device-Identifier`標頭，您可以參考下列檔案：

* [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)的Android開發人員檔案。

_(*)建議在作業系統提供的值上套用SHA-256雜湊函式。_

### 電視連線裝置 {#tv-connected-devices}

#### tvOS {#tvos}

若要為執行[tvOS](https://developer.apple.com/documentation/tvos-release-notes)的裝置建置`AP-Device-Identifier`標題，您可以參考下列檔案：

* [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)的Apple開發人員檔案。

_(*)建議在作業系統提供的值上套用SHA-256雜湊函式。_

#### Fire OS {#fireos}

若要為執行[Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html)的裝置建置`AP-Device-Identifier`標頭，您可以參考下列檔案：

* [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID)的Android開發人員檔案。

_(*)建議在作業系統提供的值上套用SHA-256雜湊函式。_

#### Roku OS {#rokuos}

若要為執行[Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md)的裝置建置`AP-Device-Identifier`標題，您可以參考下列檔案：

* [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string)的Roku開發人員檔案。

_(*)建議在作業系統提供的值上套用SHA-256雜湊函式。_
