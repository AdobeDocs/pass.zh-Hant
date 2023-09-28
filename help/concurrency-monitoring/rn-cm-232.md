---
title: Adobe Pass Concurrency Monitoring 2.3.2發行說明
description: Adobe Pass Concurrency Monitoring 2.3.2發行說明
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---


# Adobe Pass Concurrency Monitoring 2.3.2發行說明 {#cm-232}

此頁面說明此版本的新功能、變更和已知問題：

發行日期： 2015年12月11日

## 新功能和改進專案 {#new-features}

* 「使用情況報表」中提供的新劃分。 如果與「並行監視」整合的應用程式傳送自訂中繼資料，則可使用新的劃分。
   * 應用程式 — 在呼叫URL中報告的應用程式ID
   * mvpd — 在呼叫URL中報告的MVPD
   * 管道 — 自訂中繼資料管道
   * 平台 — 自訂中繼資料applicationPlatform
* 與以下專案相關的新量度 **串流持續時間** 可在「使用情況報表」中使用。 新量度可用來建立串流持續時間的長條圖。 目前提供下列時間間隔（分鐘）：
   * duration_0-15
   * duration_15-30
   * duration_30-60
   * duration_60-120
   * duration_over-120

## 錯誤修正 {#bug-fixes}

不適用

## 已知問題 {#known-issues}

不適用
