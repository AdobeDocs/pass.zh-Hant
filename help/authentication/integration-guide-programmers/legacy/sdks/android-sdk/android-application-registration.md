---
title: Android應用程式註冊
description: Android應用程式註冊
exl-id: 6238bd87-ac97-4a5c-9d92-3631f7b2d46a
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 0%

---

# （舊版） Android應用程式註冊 {#android-application-registration}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

>[!IMPORTANT]
>
> 請務必隨時瞭解彙總在[產品公告](/help/authentication/product-announcements.md)頁面中的最新Adobe Pass驗證產品公告和淘汰時間表。

## 簡介 {#intro}

從3.0版的Android AccessEnabler SDK開始，我們將透過Adobe伺服器變更驗證機制。 我們不使用公開金鑰和秘密系統來簽署requestorID，而是引入軟體陳述式字串的概念，可用來取得存取權杖，稍後再用於SDK對伺服器進行的所有呼叫。 除了軟體宣告之外，您還需要為應用程式建立深層連結。

如需詳細資訊，請參閱[動態使用者端註冊概述](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)。

## 什麼是軟體宣告？ {#what}

軟體陳述式是包含應用程式相關資訊的JWT權杖。 每個應用程式都應該有專屬的軟體宣告，供我們的伺服器用來識別Adobe系統中的應用程式。

初始化`AccessEnabler` SDK時，需要傳遞軟體陳述式。 它可用來向Adobe註冊應用程式。 註冊後，SDK會接收使用者端ID和使用者端密碼，用於取得存取權杖。 SDK對Adobe伺服器的任何呼叫都需要有效的存取Token。 SDK負責註冊應用程式、取得和重新整理存取權杖。

>[!NOTE]
>
>軟體陳述式是應用程式專屬的，個別軟體陳述式不能用於多個應用程式。 請注意，程式設計師層級的軟體陳述式有相同的限制，它們只能用於單一應用程式，無論是單一通道還是多通道。

## 如何取得軟體宣告 {#how-to-get-ss}

以下是取得軟體宣告的方法。

### 如果您能存取Adobe的TVE控制面板

1. 開啟瀏覽器並導覽至[Adobe Pass TVE儀表板](https://experience.adobe.com/#/pass/authentication)。

1. 導覽至&#x200B;**[!UICONTROL Channels]**&#x200B;區段，然後選取您的頻道。

1. 導覽至&#x200B;**[!UICONTROL Registered Applications]**&#x200B;標籤。

1. 按一下&#x200B;**[!UICONTROL Add new application]**。

1. 命名應用程式並指定版本。

1. 選取可使用應用程式的平台(此案例中為Android)。

1. 從已為程式設計師設定的網域清單中選擇，以提供&#x200B;**[!UICONTROL Domain Name]**。

1. 將您的變更推播到伺服器，然後導覽回到您頻道的&#x200B;**[!UICONTROL Registered Applications]**&#x200B;索引標籤。

   您應該會看到包含所有已註冊應用程式的清單。 在您建立的應用程式上選取&#x200B;**[!UICONTROL Download]**。 您可能需要等待幾分鐘，軟體宣告才可供下載。

   下載文字檔。 將其內容當做您的軟體宣告使用。

如需詳細資訊，請參閱[動態使用者端註冊管理](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md#dynamic-client-registration-management)。

### 如果您沒有Adobe TVE儀表板的存取權

將票證提交至`tve-support@adobe.com`。 包括必要的資訊，例如頻道、應用程式名稱、版本和平台。 我們的支援團隊會有人為您建立軟體宣告。

## 如何使用軟體宣告 {#how-to-use-ss}

取得軟體陳述式後，您必須將它當做Access Enabler建構函式中的引數傳遞。 我們建議將軟體宣告託管於遠端位置。 如此一來，您就可以輕鬆撤銷及變更軟體陳述式，而不需發行新版的應用程式。

## 為您的應用程式建立並使用深層連結 {#create}

在Android上，使用與您建立軟體宣告時選取的網域名稱相反的深層連結值

已建立的深層連結在Android裝置上應具有一個唯一值。 當多個應用程式使用相同的深層連結值時，驗證和登出流程將會干預。

## 如何使用軟體宣告及深層連結 {#use-both}

在應用程式的資源檔`strings.xml`中新增下列程式碼：

```JAVA
    <string name="software_statement">softwarestatement value</string>
    <string name="redirect_uri">com.domain_name</string>
```
