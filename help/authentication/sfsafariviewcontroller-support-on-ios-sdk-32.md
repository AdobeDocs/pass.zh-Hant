---
title: iOS SDK 3.2+上的SFSafariViewController支援
description: iOS SDK 3.2+上的SFSafariViewController支援
exl-id: 6691550f-c36f-4fae-aa77-082ca7d8a60a
source-git-commit: 929d1cc2e0466155b29d1f905f2979c942c9ab8c
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# iOS SDK 3.2+ {#sfsafariviewcontroller-support-on-ios-sdk-3.2}上的SFSafariViewController支援

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>


**由於安全性需求，某些MVPD的登入頁面必須以SFSafariViewController呈現，而非Web檢視。**

有些MVPD會要求其登入頁面必須以SFSafariViewController之類的安全瀏覽器控制項顯示。 它們會主動封鎖網頁檢視，因此為了能夠使用它們進行驗證，我們必須使用SVC。

## 相容性 {#compatiblity}

從iOS SDK 3.1版開始，AccessEnabler SDK會根據伺服器設定，自動顯示SFSafariViewController中特定MVPD的登入頁面。

SDK 3.1版會自動從應用程式的根檢視控制器顯示SFSafariViewController。 雖然這可簡化實作人員的登入頁面管理，但在某些情況下，由於應用程式的特定實作（例如已可見的強制回應視窗控制器），無法從根檢視控制器呈現SFSafariViewController。

在這種情況下，3.2版讓程式設計師能夠手動管理SVC。

## 手動SVC管理 {#manual-svc-management}

若要手動管理SVC，實作人員必須執行下列步驟：


1. 在AccessEnabler初始化之後呼叫&#x200B;**setOptions([&quot;handleSVC&quot;：true])** （請確定在驗證開始之前執行此呼叫）。 這會啟用「手動」SVC管理，SDK不會自動呈現SVC，而是在需要時呈現     呼叫&#x200B;**導覽(toUrl：*{url}* useSVC：true)**。

1. 在實作中實作選擇性回呼&#x200B;**`navigateToUrl:useSVC:`**，您必須使用提供的URL使用SFSafariViewController執行個體建立svc執行個體，並在熒幕上顯示：

   ```obj-c
   func navigate(toUrl url: String!, useSVC: Bool) {
       svc =  SFSafariViewController(url: URL(string: url)!)
       svc.delegate = self
       myController.present(svc, animated: true)
       }
   ```

   ***附註：***

   - *您可以用任何想要的方式自訂SFSafariViewController。 例如，在iOS 11+上，您可以將「完成」標籤變更為「取消」。*
   - *為了能夠關閉svc，您需要它的參考，請勿在&#x200B;**navigateToUrl：useSVC***的範圍中建立它
   - *對「myController」使用您自己的檢視控制器*


1. 在您應用程式的&#x200B;**應用程式（\_app： UIApplication的委派實作中，開啟URL： URL，選項： \[UIApplicationOpenURLOptionsKey： Any\]） -\> Bool**，新增程式碼以關閉svc。 您應該已經有呼叫&#x200B;**accessEnabler.handleExternalURL()**&#x200B;的程式碼。 新增以下專案：

   ```obj-c
   if(svc != nil) {
       svc.dismiss(animated: true)
   }
   ```

   同樣地，svc是您在步驟2建立的SFSafariViewController的參照。


1. 從&#x200B;**SFSafariViewControllerDelegate**&#x200B;實作&#x200B;**safariViewControllerDidFinish(\_ controller： SFSafariViewController)**，以便在使用者使用[完成]按鈕取消svc時捕獲。 在此函式中，若要通知SDK驗證已取消，您必須呼叫：

   ```obj-c
   accessEnabler.setSelectedProvider(nil)
   ```
