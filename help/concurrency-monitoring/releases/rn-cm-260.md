---
title: Adobe Pass Concurrency Monitoring 2.6.0發行說明
description: Adobe Pass Concurrency Monitoring 2.6.0發行說明
exl-id: f24980e3-ffe8-4b5e-8adc-ae443baed40f
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---

# Adobe Pass Concurrency Monitoring 2.6.0發行說明 {#cm-260}


此頁面說明此版本的新功能、變更和已知問題：



## 發行日期： 2016年11月10日



## 新功能

此版本新增了終止現有資料流的功能，以便允許目前資料流開始（亦即終止資料流）。



**遠端終止**

* 在409 Conflict回應中，建議的「conflicts」欄位中列出的每個工作階段都會附帶terminationCode屬性。
* 系統會以衝突工作階段清單提示使用者，並允許使用者選擇要終止的工作階段
* 遠端工作階段只能透過在工作階段初始化嘗試中傳遞X-Terminate要求標頭（將選取的終止代碼作為值）來終止。
* 已為410 Gone回應定義新型別的「建議」，以指出已終止目前的工作階段。


如需詳細資訊，請參閱更新後的檔案。



>[!NOTE]
>
>用於列出作用中工作階段的工作階段定義已更新，以包含應用程式名稱和裝置名稱（而非應用程式ID）。




## 錯誤修正 {#bug-fixes}

移除伺服器回應中的重複標頭（此修正同時涉及CORS標頭和Date one）。




## 已知問題 {#known-issues}

不適用
