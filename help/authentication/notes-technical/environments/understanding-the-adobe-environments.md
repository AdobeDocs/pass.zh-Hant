---
title: 瞭解Adobe環境
description: 瞭解Adobe環境
exl-id: bb6cf37f-48cd-47bb-b3c2-f7a96e49b12d
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 0%

---

# 瞭解Adobe環境 {#understanding-the-adobe-environments}

>[!NOTE]
>
>此頁面上的內容僅供參考。 使用此API需要Adobe的目前授權。 不允許未經授權的使用。

提供描述Adobe環境的官方檔案[在預備中設定您的環境並進行測試](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)：

Adobe環境，用幾個詞概括：

Adobe有兩個環境： **資格預審**&#x200B;和&#x200B;**版本**。

* 在「資格預審」環境中，我們正在準備要發行的新組建。

* 目前的版本編號在版本環境上。

每個環境都有兩個設定檔： **暫存**&#x200B;和&#x200B;**生產**。

* 中繼設定檔會連線至MVPDs中繼伺服器
* 生產設定檔會連線至MVPDs生產設定檔。

擁有這兩個設定檔的原因是，在測試設定檔上，我們正在準備新的合作夥伴上線，他們希望使用即將推出的組建（資格預審）或發行版本（更穩定）來測試系統。

如果合作夥伴想要測試新版本，則需執行額外的幾個步驟。 請參閱[設定您的環境，並在先決條件](/help/authentication/notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)中進行測試。

依照上述步驟，即可確保即將發行的版本將會在資格預審環境中進行測試。
