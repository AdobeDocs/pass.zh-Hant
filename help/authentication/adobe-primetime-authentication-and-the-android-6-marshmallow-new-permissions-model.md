---
title: Adobe Pass驗證和Android 6 「Marshmallow」新許可權模型
description: Adobe Pass驗證和Android 6 「Marshmallow」新許可權模型
exl-id: 3c96769e-b25b-48ab-bb74-40f13d4e5a84
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 0%

---

# Adobe Pass驗證和Android 6 「Marshmallow」新許可權模型 {#adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

</br>

新的Android 6 Marshmallow發行版本引進了一些許可權模型的更新，這可能會影響使用現有Adobe Pass Authentication SDK 1.8版及舊版的應用程式行為。

作為新功能，新的Android作業系統提供[精細的控制許可權，控制應用程式在安裝時和執行階段所需的許可權](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html)。

>[!IMPORTANT]
>
>下列變更將&#x200B;**只影響專為Android 6.0** (targetSdkVersion=23)開發的應用程式。 升級至Android 6.0時，不會影響使用者裝置上已安裝的舊版應用程式。


具體來說，對於在Android Studio中使用[API層級23](http://developer.android.com/sdk/api_diff/23/changes.html)開發且使用Adobe Pass Authentication SDK的應用程式，開發人員將需要撰寫自訂程式碼（請參閱下方的程式碼片段） [以觸發允許/拒絕許可權對話方塊](https://developer.android.com/training/permissions/requesting.html)。

以下是用來要求裝置外部儲存裝置的寫入存取權的程式碼節選：

```java
// Here, thisActivity is the current activity
if (ContextCompat.checkSelfPermission(thisActivity,
                Manifest.permission.WRITE_EXTERNAL_STORAGE)
        != PackageManager.WRITE_EXTERNAL_STORAGE) {

    // Should we show an explanation?
    if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
            Manifest.permission.WRITE_EXTERNAL_STORAGE)) {

        // Show an expanation to the user *asynchronously* -- don't block
        // this thread waiting for the user's response! After the user
        // sees the explanation, try again to request the permission.

    } else {

        // No explanation needed, we can request the permission.

        ActivityCompat.requestPermissions(thisActivity,
                new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE},
                MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE);

        // MY_PERMISSIONS_REQUEST_WRITE_EXTERNAL_STORAGE is an
        // app-defined int constant. The callback method gets the
        // result of the request.
    }
}
```




**從使用者的角度來看**，在安裝時，使用者會看到一個視窗，提示他們確認檔案的讀取/寫入許可權（請參閱下圖2）。 這會產生下列兩個結果之一：

1. 如果使用者&#x200B;**確認**&#x200B;許可權，則會保留一般驗證流程，且代號會儲存在全域儲存體中。 只要權杖有效，使用者將在應用程式中或跨應用程式中使用Adobe Pass驗證保持驗證。
1. 如果使用者&#x200B;**拒絕**&#x200B;許可權，儲存區中的寫入動作將會失敗，而且使用者只有在結束應用程式之後才會驗證。 請注意，某些應用程式在前景和背景之間切換時會重新初始化，這樣使用者就會在執行此動作時登出。 不會儲存Token，使用者每次使用應用程式時都需要進行驗證。


>[!TIP]
>
>Adobe Pass Authentication SDK 1.9目前正在開發引入儲存彈性的功能。新SDK的目標發行日期為10月&#x200B;**日的最後一週**。 當無法使用一般儲存體時，應用程式將會回復到在應用程式的沙箱儲存體中寫入。 針對在API層級23中開發的應用程式，此規範涵蓋使用者不接受全域儲存中的讀取/寫入許可權的情況。 代號會依應用程式個別儲存，這表示使用Adobe Pass驗證的應用程式之間的單一登入功能將會停用。


![](assets/android-permissions-request.png)

*圖：針對目標API層級23*&#x200B;所編寫的應用程式，其許可權要求對話方塊

>[!IMPORTANT]
>
> Adobe建議&#x200B;**其合作夥伴使用API Level 22 (targetSdkVersion=22)或更舊版本開發應用程式，以確保驗證程式**&#x200B;中最佳的使用者體驗。
