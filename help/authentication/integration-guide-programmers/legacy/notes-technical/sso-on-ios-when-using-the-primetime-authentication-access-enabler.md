---
title: 使用iOS Authentication Access Enabler時Adobe Pass上的SSO
description: 使用iOS Authentication Access Enabler時Adobe Pass上的SSO
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1122'
ht-degree: 0%

---

# （舊版） iOS在使用Adobe Pass Authentication Access Enabler時的SSO {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>

## 概觀

Adobe Pass驗證支援的應用程式之間的單一登入(SSO)會根據基礎作業系統以不同方式運作。

使用Adobe Pass驗證&#x200B;**Access Enabler**&#x200B;時，此檔案會在iOS **上處理** SSO。

**Access Enabler** **1.10**&#x200B;是Adobe Pass Authentication iOS原生SDK的最新版本。 Adobe強烈建議您改用此版本，不要再使用較舊的版本。 如果您使用舊版的Access Enabler，您可以在此下載最新版的[](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)。

iOS上的SSO受下列條件支配：

- 應用程式必須使用相同的&#x200B;**權杖存放區** （以Access Enabler建立的自訂作業範圍形式）。
- 應用程式必須產生相同的&#x200B;**裝置ID** (Access Enabler會根據MAC位址或IDFV （視作業系統版本而定）計算裝置ID)。

## 行為

SSO行為如下：

- **iOS 6及較低版本**： SSO會自動在同一個團隊或不同團隊開發的應用程式之間運作。 裝置ID是根據MAC位址計算（相同的值會在所有應用程式中產生），且儲存區域在所有應用程式中都是通用的(自訂作業範圍可在iOS 6及較低版本的應用程式中分享)。
   - **重要：**&#x200B;請注意，iOS SDK 1.9.4版本已[將最低iOS部署目標提高至iOS 7。](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7和更高版本**： SSO將在下列條件下運作：

1. 應用程式是使用相同的Apple發佈設定檔發佈，或是屬於相同團隊的設定檔發佈。 這是應用程式在iOS 7和更高版本上共用自訂作業底板的唯一方法。 在所有其他情況下，作業範圍會依應用程式而沙箱。 來自&#x200B;[*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html)： \+\[`UIPasteboard pasteboardWithName:create:\`]和+\[`UIPasteboard pasteboardWithUniqueName`\]的指定名稱現在是唯一的，僅允許相同應用程式群組中的那些應用程式存取作業面板。 如果開發人員嘗試以已存在的名稱建立剪貼簿，而他們不屬於相同應用程式套裝，則會取得專屬的私人剪貼簿。 請注意，這不會影響系統提供的作業範圍、一般和尋找。

1. 應用程式具有相同的套件ID首碼（最後一個元件以外的所有元件）。 只有共用相同套件ID首碼的應用程式會運算相同的IDFV。 從&#x200B;[*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor)：在IOS 7上，組合的所有元件（最後一個元件除外）都會用來產生廠商ID。 如果束ID只有單一元件，則會使用整個束ID。

現在來關注&#x200B;**&#39;iOS 7和更高版本&#39;**&#x200B;情境，因為這是真實使用者最常使用的情境：

這兩個條件（共用來自相同開發團隊的設定檔並具有相同的套件識別碼首碼）是SSO的必要條件。

以下是可能的組合及其產生的結果：

- **來自相同團隊的設定檔與相同的套件組合識別碼首碼**：應用程式將共用相同的作業範圍儲存空間，並具有相同的裝置ID (IDFV)。 使用者只需要驗證一次（在使用的第一個應用程式中），並且驗證狀態將在所有其他應用程式之間共用。 範例流程：

1. 使用者開啟應用程式A （套件識別碼為&#x200B;*com.x.y.AppA*）且已取消驗證
1. 使用者在應用程式A中執行驗證
1. 使用者開啟應用程式B （套件識別碼為&#x200B;*com.x.y.AppB*），並透過共用應用程式的權益資料來自動驗證
A （來自步驟2）
1. 使用者開啟應用程式A後仍會驗證（從步驟2）



- **來自相同團隊但不同套件ID首碼的設定檔**：應用程式將共用相同的作業範圍儲存，但會有不同的裝置ID (IDFV)。 使用者需要針對每個應用程式驗證一次。 範例流程：

1. 使用者開啟應用程式A （套件識別碼為&#x200B;*com.x.y.AppA*）且已取消驗證
1. 使用者在應用程式A中執行驗證
1. 使用者開啟應用程式B （套件識別碼為&#x200B;*com.z.AppB*），且Access Enabler偵測到第一個應用程式取得的權杖（因為儲存空間是共用的），但因為不同的裝置ID，所以不會嘗試透過SSO使用它
1. 使用者在應用程式B中執行驗證
1. 使用者開啟應用程式A後仍會驗證（從步驟2）



- **來自不同團隊的設定檔（此案例中套件組合識別碼不相關）**：應用程式會有不同的剪貼簿儲存空間，且它們之間會停用SSO。 使用者需要為每個應用程式驗證一次，且在不同應用程式之間切換時，驗證工作階段將持續存在。 範例流程：


1. 使用者開啟應用程式A且未驗證
1. 使用者在應用程式A中執行驗證
1. 使用者開啟應用程式B且未驗證
1. 使用者在應用程式B中執行驗證
1. 使用者開啟應用程式A並進行驗證（從步驟2）
1. 使用者開啟應用程式B並進行驗證（從步驟4）

**注意：**&#x200B;請注意，透過&#x200B;**Apple App Store**&#x200B;安裝應用程式時，上述SSO條件適用。 如果應用程式部署在模擬器上（應用程式簽署不適用）、使用Xcode安裝或透過Ad Hoc設定檔發佈，您可能會獲得不同的結果。

**重要：**&#x200B;注意（**關於AccessEnabler v1.8**）：上述的第二個案例（來自相同團隊但不同套件識別碼首碼的設定檔）會在相同團隊（媒體公司）開發的應用程式中，為&#x200B;**AccessEnabler v1.8**&#x200B;的使用者建立非常不愉快的使用者體驗。 在來自相同媒體公司的應用程式之間轉換時，使用者將會自動登出，因此，應用程式開發人員在決定套件ID和發佈設定檔時必須小心。 此案例的確切情況如下所示：

應用程式將共用相同的作業範圍儲存空間，但會有不同的裝置ID (IDFV)。 使用者需要為每個應用程式驗證一次，**，但是在應用程式之間切換時，將會清除驗證工作階段**。 範例流程：

1. 使用者開啟應用程式A （套件識別碼為&#x200B;*com.x.y.AppA*）且已取消驗證
1. 使用者在應用程式A中執行驗證
1. 使用者開啟應用程式B （套件識別碼為&#x200B;*com.z.AppB*），而應用程式A建立的權益資料會由Access Enabler自動清除（安全性機制會偵測應用程式B中目前計算的裝置ID與應用程式A建立的權益權杖中儲存的裝置ID之間是否有衝突）
1. 使用者在應用程式B中執行驗證
1. 使用者開啟應用程式A，應用程式B建立的權益資料會由Access Enabler自動清除（安全性機制，會偵測應用程式A中目前計算的裝置ID與應用程式B建立的權益權杖中儲存的裝置ID之間的衝突）
