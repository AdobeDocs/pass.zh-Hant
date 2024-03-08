---
title: 使用iOS Authentication Access Enabler時Adobe Pass上的SSO
description: 使用iOS Authentication Access Enabler時Adobe Pass上的SSO
exl-id: 882f0abb-2e6e-461d-a375-3ab410991935
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '1121'
ht-degree: 0%

---

# 使用iOS Authentication Access Enabler時Adobe Pass上的SSO {#sso-on-ios-when-using-the-primetime-authentication-access-enabler}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>

## 概觀

Adobe Pass驗證支援的應用程式之間的單一登入(SSO)會根據基礎作業系統以不同方式運作。

本檔案地址 **iOS上的SSO**，使用Adobe Pass驗證時 **存取啟用程式**.

**存取啟用程式** **1.10** 是Adobe Pass Authentication iOS原生SDK的最新版本。 Adobe強烈建議您改用此版本，不要再使用較舊的版本。 如果您使用的是舊版Access Enabler，您可以下載最新版本 [此處](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

iOS上的SSO受下列條件支配：

- 應用程式必須使用相同的 **權杖儲存** （以Access Enabler建立的自訂作業範圍形式）。
- 應用程式必須產生相同的 **裝置ID** (Access Enabler會根據MAC位址或IDFV （視作業系統版本而定）計算裝置ID。)

## 行為

SSO行為如下：

- **iOS 6及以下版本**：SSO可在相同團隊或不同團隊開發的應用程式之間自動運作。 裝置ID是根據MAC位址計算（相同的值會在所有應用程式中產生），且儲存區域在所有應用程式中都是通用的(自訂作業範圍可在iOS 6及較低版本的應用程式中分享)。
   - **重要：** 請注意，iOS SDK 1.9.4版已 [將最低iOS部署目標提高至iOS 7。](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library)
- **iOS 7和更新版本**：SSO的運作條件如下：

1. 應用程式是使用相同的Apple發佈設定檔發佈，或是屬於相同團隊的設定檔發佈。 這是應用程式在iOS 7和更高版本上共用自訂作業底板的唯一方法。 在所有其他情況下，作業範圍會依應用程式而沙箱。 從 [*https://developer.apple.com/library/IOs/releasenotes/General/RN-iOSSDK-7.0/index.html*](https://developer.apple.com/library/ios/releasenotes/General/RN-iOSSDK-7.0/index.html)： \+\[`UIPasteboard pasteboardWithName:create:\`]和+\[`UIPasteboard pasteboardWithUniqueName`\]現在指定名稱是唯一的，僅允許相同應用程式群組中的應用程式存取作業面板。 如果開發人員嘗試以已存在的名稱建立剪貼簿，而他們不屬於相同應用程式套裝，則會取得專屬的私人剪貼簿。 請注意，這不會影響系統提供的作業範圍、一般和尋找。

1. 應用程式具有相同的套件ID首碼（最後一個元件以外的所有元件）。 只有共用相同套件ID首碼的應用程式會運算相同的IDFV。 從 [*https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice\_Class/index.html\#//apple\_ref/occ/instp/UIDevice/identifierForVendor*](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIDevice_Class/index.html#//apple_ref/occ/instp/UIDevice/identifierForVendor)：在IOS 7上，套件組合的所有元件（最後一個元件除外）都會用來產生廠商ID。 如果束ID只有單一元件，則會使用整個束ID。

現在來關注一下 **&#39;iOS 7和更高版本&#39;** 案例，因為這是實際使用者最常發生的情形：

這兩個條件（共用來自相同開發團隊的設定檔並具有相同的套件識別碼首碼）是SSO的必要條件。

以下是可能的組合及其產生的結果：

- **來自相同團隊和相同套件ID首碼的設定檔**：應用程式會共用相同的作業範圍儲存空間，且裝置識別碼(IDFV)相同。 使用者只需要驗證一次（在使用的第一個應用程式中），並且驗證狀態將在所有其他應用程式之間共用。 範例流程：

1. 使用者開啟應用程式A （含套裝ID） *com.x.y.AppA*)且已取消驗證
1. 使用者在應用程式A中執行驗證
1. 使用者開啟應用程式B （含套裝ID） *com.x.y.AppB*)並透過共用應用程式A的權益資料自動驗證（來自步驟2）
1. 使用者開啟應用程式A後仍會驗證（從步驟2）



- **來自相同團隊但不同套件ID首碼的設定檔**：應用程式將共用相同的作業範圍儲存空間，但會有不同的裝置ID (IDFV)。 使用者需要針對每個應用程式驗證一次。 範例流程：

1. 使用者開啟應用程式A （含套裝ID） *com.x.y.AppA*)且已取消驗證
1. 使用者在應用程式A中執行驗證
1. 使用者開啟應用程式B （含套裝ID） *com.z.AppB*)和Access Enabler會偵測第一個應用程式取得的權杖（因為儲存空間已共用），但因為裝置ID不同，所以不會嘗試透過SSO使用權杖
1. 使用者在應用程式B中執行驗證
1. 使用者開啟應用程式A後仍會驗證（從步驟2）



- **來自不同團隊的設定檔（在此案例中，套件ID不相關）**：應用程式會有不同的作業範圍儲存空間，且兩者之間的SSO將被停用。 使用者需要為每個應用程式驗證一次，且在不同應用程式之間切換時，驗證工作階段將持續存在。 範例流程：


1. 使用者開啟應用程式A且未驗證
1. 使用者在應用程式A中執行驗證
1. 使用者開啟應用程式B且未驗證
1. 使用者在應用程式B中執行驗證
1. 使用者開啟應用程式A並進行驗證（從步驟2）
1. 使用者開啟應用程式B並進行驗證（從步驟4）

**注意：** 請注意，上述的SSO條件適用於透過 **Apple App Store**. 如果應用程式部署在模擬器上（應用程式簽署不適用）、使用Xcode安裝或透過Ad Hoc設定檔發佈，您可能會獲得不同的結果。

**重要：** 附註(**關於AccessEnabler v1.8**)：上述第二個案例（來自相同團隊但不同套件ID首碼的設定檔）會對的使用者造成非常不愉快的使用者體驗。 **AccessEnabler v1.8** 跨由相同團隊（媒體公司）開發的應用程式。 在來自相同媒體公司的應用程式之間轉換時，使用者將會自動登出，因此，應用程式開發人員在決定套件ID和發佈設定檔時必須小心。 此案例的確切情況如下所示：

應用程式將共用相同的作業範圍儲存空間，但會有不同的裝置ID (IDFV)。 使用者需要針對每個應用程式驗證一次， **但是在應用程式之間切換時，將會清除驗證工作階段**. 範例流程：

1. 使用者開啟應用程式A （含套裝ID） *com.x.y.AppA*)且已取消驗證
1. 使用者在應用程式A中執行驗證
1. 使用者開啟應用程式B （含套裝ID） *com.z.AppB*)且應用程式A建立的權益資料會由Access Enabler自動清除(安全性機制會偵測應用程式B中目前計算的裝置ID與權益權杖（由應用程式A建立）中儲存的裝置ID之間的衝突)
1. 使用者在應用程式B中執行驗證
1. 使用者開啟應用程式A，應用程式B建立的權益資料會由Access Enabler自動清除（安全性機制，會偵測應用程式A中目前計算的裝置ID與應用程式B建立的權益權杖中儲存的裝置ID之間的衝突）
