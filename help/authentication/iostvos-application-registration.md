---
title: iOS/tvOS應用程式註冊
description: iOS/tvOS應用程式註冊
exl-id: 89ee6b5a-29fa-4396-bfc8-7651aa3d6826
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 0%

---


# iOS/tvOS應用程式註冊 {#iostvos-application-registration}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

## 簡介 {#Intro}

從3.0版的iOS/tvOS AccessEnabler SDK開始，我們正在變更Adobe伺服器的驗證機制。 我們不使用公開金鑰和秘密系統來簽署requestorID，而是引入軟體陳述式字串概念，可用來取得存取權杖，稍後再用於SDK對伺服器進行的所有呼叫。 除了軟體宣告之外，您還需要應用程式的自訂URL配置。

如需詳細資訊，請參閱[動態使用者端註冊概述](./dcr-api/dynamic-client-registration-overview.md)。

## 什麼是軟體宣告？ {#Soft_state}

軟體陳述式是包含應用程式相關資訊的JWT權杖。 每個應用程式都應該有獨特的軟體宣告，我們的伺服器會使用這些宣告來識別Adobe系統中的應用程式。 初始化AccessEnabler SDK時，需要傳遞軟體陳述式，而且軟體陳述式將用來註冊具有Adobe的應用程式。 註冊後，SDK會收到使用者端ID和使用者端密碼，這些密碼將用於取得存取權杖。 SDK對伺服器進行的任何呼叫都需要有效的存取Token。 SDK負責註冊應用程式、取得和重新整理存取權杖。

**注意：**&#x200B;軟體陳述式是應用程式特定的，同一個軟體陳述式不能用於多個應用程式。 請注意，程式設計師層級的軟體陳述式也遵循相同原則，也就是它們只能用於單一應用程式 — 無論是單一通道還是多通道。 此限制也適用於自訂配置。

## 如何取得軟體宣告？ {#obtain}

### 如果您能存取Adobe的TVE控制面板：

- 開啟瀏覽器並導覽至<https://console.auth.adobe.com>
- 導覽至`Channels`區段並選取您的頻道。
- 導覽至`Registered Applications`標籤。
- 按一下`Add new application`。
- 提供應用程式的名稱和版本，並選取   提供此功能的平台。 在此案例中為iOS/tvOS。
- 將變更推播至伺服器，然後導覽回您管道的「已註冊應用程式」標籤。
- 您應該會看到包含所有已註冊應用程式的清單。 按一下   您剛建立的應用程式上的`Download`按鈕。 您可能需要等待幾分鐘，軟體宣告才可供下載。
- 將會下載文字檔。 將其內容當做您的軟體宣告使用。

如需詳細資訊，請參閱[動態使用者端註冊管理](./dcr-api/dynamic-client-registration-overview.md#dynamic-client-registration-management)。

### 如果您沒有AdobeTVE儀表板的存取權：

將票證提交至<tve-support@adobe.com>。 請包含所有必要的資訊，例如頻道、應用程式名稱、版本和平台，我們的支援團隊會為您建立軟體宣告。

## 如何使用軟體宣告？ {#use}

取得軟體陳述式後，您必須在Access Enabler建構函式中將其作為引數傳遞。 我們建議將軟體宣告託管於遠端位置。 如此一來，您就可以輕鬆地撤銷及變更軟體陳述式，而不需發行新版的應用程式。

## 為您的應用程式產生自訂URL配置 {#generating}

### 如果您能存取Adobe的TVE控制面板：

- 開啟瀏覽器並導覽至<https://console.auth.adobe.com>
- 導覽至`Channels`區段並選取您的頻道。
- 導覽至`Custom Schemes`標籤。
- 按一下`Generate a new custom scheme`。
- 將會為您的應用程式產生新的自訂配置。 例如： `adbe.1JqxQsYhQOCIrwPjaooY8w://`
- 將變更推送至伺服器。

### 如果您沒有AdobeTVE儀表板的存取權：

將票證提交至<tve-support@adobe.com>。 請包含管道ID，我們的支援團隊會有人為您建立自訂配置。

## 如何使用自訂配置 {#use_custom}

在應用程式的`info.plist`檔案中新增下列程式碼：

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>adbe.u-XFXJeTSDuJiIQs0HVRAg</string> // replace this with your custom scheme
            </array>
        </dict>
    </array>
```
